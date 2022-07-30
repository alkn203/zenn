---
title: "[phina.js]ゼルダ風のライフゲージを作ってみた"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
私の好きなタイトルであるゼルダの伝説の象徴といえばトライフォースですが、それと並んでハートのライフゲージがあります。今回は、それを**phina.js**で真似て作ってみました。

![heartguage.gif](/images/heartguage.gif)

[[runstantで確認]](https://runstant.com/alkn203/projects/898a044a)

# 設計
* ハートの最大値は20個とし、初期は3個とする
* 10個で折り返して表示する
* ダメージと回復時にアニメーションさせる

# 実装
**phina.js**には、ライフゲージに使える**phina.ui.Guage**という便利なクラスが用意されていますが、今回作るものとはアプローチが異なるので、設計思想だけを参考にクラスを作ります。

# HeartGuageクラス
今回作ったクラスです。現在、以下の自作拡張クラスに組み入れてます。



以下、関数ごとに説明していきます。

# コンストラクタ

```js
// コンストラクタ
  init: function(options) {
    options = ({}).$safe(options || {}, phina.ui.HeartGuage.defaults);
    // 親クラス初期化
    this.superInit(options);
    // グループ
    var emptyGroup = DisplayElement().addChildTo(this);
    var heartGroup = DisplayElement().addChildTo(this);

    var size = options.gridSize;
    var offset = options.offset;
    // ハートサイズ
    this.heartRadius = (size / 2) * 0.8;
    // ハートの色
    this.heartColor = options.heartColor;
    // 空ハートの色
    this.emptyColor = options.emptyColor;
    // 折り返し個数
    var col = options.colmun;
    // 初期最大値
    var max = options.defaultMax;
    // ハート最大値
    this.maxValue = options.maxValue;
    // アニメーションするかどうか
    this.animation = options.animation;
    // グリッド
    this.grid = Grid({
      width: size * col,
      colmuns: col,
      offset: offset
    });
    
    var self = this;
    // 初期ハート作成
    (max).times(function(i) {
      // 空ハート
      self.createHeart('empty').addChildTo(emptyGroup);
      // ハート中身
      self.createHeart().addChildTo(heartGroup);
    });
    // 参照用
    this.col = col;
    this.emptyGroup = emptyGroup;
    this.heartGroup = heartGroup;
    // 初期配置
    this.reposition(emptyGroup);
    this.reposition(heartGroup);
  },
```

* ハートは、予め用意されている**HearShape**クラスを利用して、空のハートと普通のハートの２つのグループを作りました。
* ハート配置用の**Grid**を作成して、初期のハートを作成しました。

# createHeart関数
ハートを作成する関数です。

```js
// ハートを作成して返す
  createHeart: function(type) {
    var color = (type === 'empty') ? this.emptyColor : this.heartColor;

    var heart = HeartShape({
      radius: this.heartRadius,
      fill: color,
      stroke: null,
    });
    return heart;
  },
```

* 空のハートか普通のハートかに応じて色を変えた**HeartShape**を作成して返します。

# reposition関数
ハートの配置を行う関数です。

```js
// ハート配置
  reposition: function(group) {
    var col = this.col;
    var grid = this.grid;
    var self = this;
    // ハート配置
    group.children.each(function(heart, i) {
      // インデックス位置設定
      var xIndex = i % col;
      var yIndex = (i / col).floor();
      // 位置指定
      heart.setPosition(grid.span(xIndex), grid.span(yIndex));
    });
  },
```

* ハートを**Grid**を使って配置します。インデックス値を計算で求める手法を使ってますが、この方法だと多重ループにしなくてよいのがメリットです。

# damage関数
ダメージ処理を行う関数です。

```js
// ダメージ
  damage: function(value) {
    // すでに空っぽなら何もしない
    if (this.isEmpty()) return;

    var children = this.heartGroup.children;
    var self = this;
    // 引数指定なしの場合は1ダメージ
    if (value === undefined) value = 1;
    // 残りハート数を超えないようにする
    if (value >= children.length) value = children.length;
    // アニメーション有効の場合
    if (this.animation) {
      (value).times(function(i) {
        // 末尾からインデックス指定
        var index = (children.length - 1) - i;
        var lost = children[index];
        // 点滅後削除
        lost.tweener.fadeOut(100).fadeIn(100)
                    .call(function() {
                      lost.remove();
                      // 空っぽ時イベント発火
                      if (self.isEmpty()) self.flare('empty');
                    }).play();
      });
    }
    else {
      (value).times(function(i) {
        self.heartGroup.children.pop();
      });

      if (self.isEmpty()) self.flare('empty');
    }
  },
```

* ダメージ値を引数にして、ダメージ値の分だけハート配列の末尾から削除しています。
* アニメーションが有効な場合は、**tweener**で点滅アニメーションさせてから削除するようにし、無効な場合は単純に**pop**を使ってすぐに削除します。
* 削除した後は**isEmpty**関数で空かどうか調べて、空の場合はイベントを発火するようにしています。

# recover関数
回復処理を行う関数です。

```js
// 回復
  recover: function(value) {
    // すでに満タンなら何もしない
    if (this.isFull()) return;

    var self = this;
    var group = this.heartGroup;
    // 引数指定なしの場合は1回復
    value = (value === undefined) ? 1 : value;
    // 空ハートとの差分
    var sub = this.emptyGroup.children.length - group.children.length;
    // 上限以上の回復はしない
    if (value >= sub) value = sub;
    // ハート追加
    (value).times(function(i) {
      var heart = self.createHeart().addChildTo(group);
      heart.setScale(0.1).tweener.scaleTo(1.0, 200).play();
    });
    // 再配置
    this.reposition(group);
    // 満タン時イベント発火
    if (this.isFull()) this.flare('full');
  },
```

* 回復値を引数にして、**addChild**でグループにハートを追加します。
* 追加するだけでは初期位置に表示されるので、**reposition**関数で配置し直します。

# ハート最大数増加
ハートの最大値を1つ増やす処理をします。

```js
 // ハート最大値を増やす
  gainHeart: function() {
    // 最大ハート数なら何もしない
    if (this.emptyGroup.children.length === this.maxValue) return;
    // 空ハートを追加
    this.createHeart('empty').addChildTo(this.emptyGroup);
    // 全回復
    var sub = this.emptyGroup.children.length - this.heartGroup.children.length;
    this.recover(sub);
    // 再配置
    this.reposition(this.emptyGroup);
    this.reposition(this.heartGroup);
  },
```

* まず空のハートをグループに追加します。
* 空のハートの数とハートの数の差分から回復する数を計算して、最大値までハートを回復させます。

# おわりに
簡単な説明になってしまいましたが、今回はゼルダ風のライフゲージを作ってみました。まだまだ改良の余地はあると思います。**phina.js**のソースは、クラス設計などをする上で参考になる部分が多いので、皆さんも自分のゲーム作成で役立てるような独自のクラスを作りに挑戦してみてください。

# 全体コード
:::details コードを見る
```js
phina.globalize();
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    
    var gauge = phina.ui.HeartGuage({offset: 100}).addChildTo(this);
    
    Button({text: 'damage'}).addChildTo(this)
                            .setPosition(this.gridX.center(), this.gridY.center())
                            .onpush = function() {
                              gauge.damage(2);  
                            };
    Button({text: 'recover'}).addChildTo(this)
                            .setPosition(this.gridX.center(), this.gridY.center(2))
                            .onpush = function() {
                              gauge.recover();  
                            };
    Button({text: 'gain'}).addChildTo(this)
                            .setPosition(this.gridX.center(), this.gridY.center(4))
                            .onpush = function() {
                              gauge.gainHeart();  
                            };
  
    gauge.on('empty', function() {
      console.log('empty!');
    });

    gauge.on('full', function() {
      console.log('full!');
    });
  },
});
/*
 * @class phina.ui.HeartGuage
 * @extends phina.display.DisplayElement
 */
phina.define("phina.ui.HeartGuage", {
  // 継承
  superClass: 'DisplayElement',
  // コンストラクタ
  init: function(options) {
    options = ({}).$safe(options || {}, phina.ui.HeartGuage.defaults);
    // 親クラス初期化
    this.superInit(options);
    // グループ
    var emptyGroup = DisplayElement().addChildTo(this);
    var heartGroup = DisplayElement().addChildTo(this);

    var size = options.gridSize;
    var offset = options.offset;
    // ハートサイズ
    this.heartRadius = (size / 2) * 0.8;
    // ハートの色
    this.heartColor = options.heartColor;
    // 空ハートの色
    this.emptyColor = options.emptyColor;
    // 折り返し個数
    var col = options.colmun;
    // 初期最大値
    var max = options.defaultMax;
    // ハート最大値
    this.maxValue = options.maxValue;
    // アニメーションするかどうか
    this.animation = options.animation;
    // グリッド
    this.grid = Grid({
      width: size * col,
      colmuns: col,
      offset: offset
    });
    
    var self = this;
    // 初期ハート作成
    (max).times(function(i) {
      // 空ハート
      self.createHeart('empty').addChildTo(emptyGroup);
      // ハート中身
      self.createHeart().addChildTo(heartGroup);
    });
    // 参照用
    this.col = col;
    this.emptyGroup = emptyGroup;
    this.heartGroup = heartGroup;
    // 初期配置
    this.reposition(emptyGroup);
    this.reposition(heartGroup);
  },
  // ハートを作成して返す
  createHeart: function(type) {
    var color = (type === 'empty') ? this.emptyColor : this.heartColor;

    var heart = HeartShape({
      radius: this.heartRadius,
      fill: color,
      stroke: null,
    });
    return heart;
  },
  // ハート配置
  reposition: function(group) {
    var col = this.col;
    var grid = this.grid;
    var self = this;
    // ハート配置
    group.children.each(function(heart, i) {
      // インデックス位置設定
      var xIndex = i % col;
      var yIndex = (i / col).floor();
      // 位置指定
      heart.setPosition(grid.span(xIndex), grid.span(yIndex));
    });
  },
  // ダメージ
  damage: function(value) {
    // すでに空っぽなら何もしない
    if (this.isEmpty()) return;

    var children = this.heartGroup.children;
    var self = this;
    // 引数指定なしの場合は1ダメージ
    if (value === undefined) value = 1;
    // 残りハート数を超えないようにする
    if (value >= children.length) value = children.length;
    // アニメーション有効の場合
    if (this.animation) {
      (value).times(function(i) {
        // 末尾からインデックス指定
        var index = (children.length - 1) - i;
        var lost = children[index];
        // 点滅後削除
        lost.tweener.fadeOut(100).fadeIn(100)
                    .call(function() {
                      lost.remove();
                      // 空っぽ時イベント発火
                      if (self.isEmpty()) self.flare('empty');
                    }).play();
      });
    }
    else {
      (value).times(function(i) {
        self.heartGroup.children.pop();
      });

      if (self.isEmpty()) self.flare('empty');
    }
  },
  // 回復
  recover: function(value) {
    // すでに満タンなら何もしない
    if (this.isFull()) return;

    var self = this;
    var group = this.heartGroup;
    // 引数指定なしの場合は1回復
    value = (value === undefined) ? 1 : value;
    // 空ハートとの差分
    var sub = this.emptyGroup.children.length - group.children.length;
    // 上限以上の回復はしない
    if (value >= sub) value = sub;
    // ハート追加
    // アニメーション有効の場合
    if (this.animation) {
      (value).times(function(i) {
        var heart = self.createHeart().addChildTo(group);
        // 拡大アニメーション
        heart.setScale(0.1).tweener.scaleTo(1.0, 200)
                                   .call(function() {
                                     // 満タン時イベント発火
                                     if (self.isFull()) self.flare('full');
                                   }).play();
      });
    }
    else {
      (value).times(function(i) {
        var heart = self.createHeart().addChildTo(group);
      });
      // 満タン時イベント発火
      if (this.isFull()) this.flare('full');
    }
    // 再配置
    this.reposition(group);
  },
  // ハート最大値を増やす
  gainHeart: function() {
    // 最大ハート数なら何もしない
    if (this.emptyGroup.children.length === this.maxValue) return;
    // 空ハートを追加
    this.createHeart('empty').addChildTo(this.emptyGroup);
    // 全回復
    var sub = this.emptyGroup.children.length - this.heartGroup.children.length;
    this.recover(sub);
    // 再配置
    this.reposition(this.emptyGroup);
    this.reposition(this.heartGroup);
  },
  // 満タンかをチェック
  isFull: function() {
    var len = this.heartGroup.children.length;
    return len > 0 && len === this.emptyGroup.children.length;
  },
  // 空っぽかをチェック
  isEmpty: function() {
    return this.heartGroup.children.length === 0;
  },
  
  _static: {
    defaults: {
      gridSize: 64,
      offset : 0,
      heartColor: 'red',
      emptyColor: 'gray',
      defaultMax: 3,
      maxValue: 20,
      colmun: 10,
      animation: true
    },
  }
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
  });
  // 実行
  app.run();
});
```
:::