---
title: "【phina.js】マリオのような敵を踏み潰すエフェクト"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

**鋭意執筆中**
https://zenn.dev/alkn203/books/phina-tips-rewrite

![](https://storage.googleapis.com/zenn-user-upload/42968747778e9fafc10a34a2.gif)

## はじめに
マリオシリーズが代表するように、敵を上から踏みつけた時に敵が潰れるエフェクトはアクションゲームではもはや定番になっています。
今回は、敵が潰れるエフェクトを**phina.js**で表現してみます。


## オブジェクトのoriginを理解する
* 今回の目的を実現するためには、オブジェクトの**origin**の変更を行う必要があります。
* **phina.js**のオブジェクトには**origin**というプロパティがあり、位置指定、回転、拡大縮小の時の基準となっています。
* **Vector2**クラス形式となっており、デフォルトは(0.5, 0.5)でオブジェクトの中心になっています。
  
![](https://storage.googleapis.com/zenn-user-upload/fb5bbfb4baf7ddde8d381453.gif)

## 敵が潰れるエフェクト
* 敵が潰れるアニメーションは、**tweener** を用います。
* **ScaleY**の値を変化させて縦に縮小させます。

```javascript
// 縦方向に縮小
bugbow.tweener.clear().to({scaleY: 0.1}, 200);
```

* 期待する結果としては下方向に潰れて欲しいところですが、今のままだと中心に向かって縮小され、思い通りの結果になりません。
* これは**origin**がオブジェクトの中心になっているのが原因です。

## originの変更と位置の調整

* 下に縮小するようにするためには、**origin** を(0.5, 1.0)に変更します。
* 変更した**origin**がオブジェクトの位置の基準となるため、変更した**origin**の分だけ上にずらして位置調整します。

```javascript
// origin変更
bugbow.setOrigin(0.5, 1.0);
// 位置調整
bugbow.y += bugbow.height / 2;
// 縦方向に縮小
bugbow.tweener.clear().to({scaleY: 0.1}, 200);
```

## おわりに
実際には、敵が潰れたコマ画像を用意してフレームを切り替えた方が効率的かもしれません。今回の内容は、あくまでも１つのアプローチと考えてもらえればと思います。

## サンプルコード
::: details コードを見る
```js
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tiles': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc/tiles.png',
    'bugbow': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc/bugbow.png',
  },
};
// 定数
var GRID_SIZE = 64; // グリッドのサイズ
// ステージデータ
var STAGE = ['4444444444',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4333333334',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004',
             '4000000004'];
             
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'skyblue';
    // グリッド
    this.gx = Grid(640, 10);
    this.gy = Grid(960, 15);
    // グループ
    this.objectGroup = DisplayElement().addChildTo(this);
    this.setStage(STAGE);

    var stone = RectangleShape({
      fill: 'gray',
      cornerRadius: 10,
    }).addChildTo(this);
    // 落下
    stone.setPosition(this.gx.center(), this.gy.center(-4)).physical.gravity.y = 0.5;
    
    var bugbow = Sprite('bugbow', GRID_SIZE, GRID_SIZE).addChildTo(this);
    bugbow.setPosition(this.gx.center(), this.gy.center(1));
    // 潰れイベント（1回限り）
    bugbow.one('stump', function() {
      // origin変更
      bugbow.setOrigin(0.5, 1.0);
      // 位置調整
      bugbow.y += bugbow.height / 2;
      // 縦方向に縮小
      bugbow.tweener.clear().to({scaleY: 0.1}, 200);
    });
    
    this.update = function() {
      // 当たり判定
      if (stone.hitTestElement(bugbow)) {
        stone.remove();
        bugbow.flare('stump');
      }
    };
  },
  // マップ作成
  setStage: function(stage) {
    var half = GRID_SIZE / 2;
    var self = this;
    // マップデータをループ
    stage.each(function(arr, j) {
      // 文字列を配列に変換
      arr.toArray().each(function(id, i) {
        var x = self.gx.span(i) + half;
        var y = self.gx.span(j) + half;
        // 空白以外の場合
        if (id > 0) {
          // タイルセットからスプライト作成
          var elem = Sprite('tiles', GRID_SIZE, GRID_SIZE).addChildTo(self.objectGroup);
          elem.setPosition(x, y);
          // フレームインデックス指定
          elem.frameIndex = id - 1;
          return;
        }
      });
    });
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  
  app.run();
});
```
:::

## runstantプロジェクト
https://runstant.com/alkn203/projects/b748b651
