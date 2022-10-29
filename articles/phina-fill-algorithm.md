---
title: "【phina.js】塗りつぶしアルゴリズムとtweenerを組み合わせた表現"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

## はじめに
ふと思い立って、単純な塗りつぶしアルゴリズムと**tweener**を組み合わせて、塗りつぶしが進む様子を可視化してみました。
工夫次第では、ゲームで室内が水で埋まっていく表現とかに使えそうです。

### Flood Fill アルゴリズム
* 塗りつぶしにおける一番単純なアルゴリズムで、塗りつぶし開始地点から上下左右のタイルで塗りつぶし可能なものを調べて、それを再帰的に調べて処理していくというものです。
* マス目が多いほど調べる対象が増えますので、処理が遅くなる可能性があります。

![](https://storage.googleapis.com/zenn-user-upload/b9d99f6a920e2fb422ca3b07.gif)

### Scanline Seed Fill アルゴリズム
* 開始点から左右横方向に塗りつぶしを行い、上下ラインに対象を広げながら繰り返し塗りつぶしていきます。
* Flood Fillに比べて調べる対象（シード）が少なくなるため、処理が比較的速いという特徴があります。

![](https://storage.googleapis.com/zenn-user-upload/811e88349d03c070b11ee436.gif)

### 塗りつぶしアニメーション
* 探査の途中で時間差で水を表示するとタイムラグが生じるため、一旦水を配置したら非表示にしています。
* **tweener**の**wait**を使って、時間差で水を再表示していくようにしています。

## ソースコード(Flood Fill)
::: details コードを見る
```javascript

// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tile': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/tiles2.png',
  },
};
// 定数
var UNIT = 64;
var HALF = UNIT / 2;
var DIR_ARRAY = [Vector2.UP, Vector2.DOWN, Vector2.LEFT, Vector2.RIGHT];
var WALL = 0;
var FLOOR = 1;
var WATER = 2;
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
    
    var data = [
      [0,0,0,0,0,0,0,0,0,0],
      [0,1,1,1,1,1,1,1,1,0],
      [0,0,0,0,0,0,0,1,1,0],
      [0,1,1,1,1,1,0,1,1,0],
      [0,1,1,1,1,1,0,1,1,0],
      [0,1,1,0,1,1,0,1,1,0],
      [0,1,1,0,1,1,1,1,1,0],
      [0,1,1,0,1,1,1,1,1,0],
      [0,1,1,0,1,1,1,1,1,0],
      [0,1,1,0,1,1,1,0,0,0],
      [0,1,1,1,1,1,1,1,1,0],
      [0,1,1,1,1,0,1,1,1,0],
      [0,1,0,0,0,0,0,1,1,0],
      [0,1,1,1,1,1,1,1,1,0],
      [0,0,0,0,0,0,0,0,0,0]
    ];

    this.map = phina.util.Map({
      tileWidth: UNIT,
      tileHeight: UNIT,
      imageName: 'tile',
      mapData: data,
    }).addChildTo(this);
    //
    this.waterGroup = DisplayElement().addChildTo(this);
    // タッチ時
    this.onpointend = function(e) {
      // タッチ位置からインデックス計算
      var i = (e.pointer.x / UNIT) | 0;
      var j = (e.pointer.y / UNIT) | 0;
      // タッチした位置が床なら
      if (this.map.checkTileByIndex(i, j) === FLOOR) {
        this.setInteractive(false);
        // 塗りつぶし開始
        this.fill(i, j);
        //
        this.showWater();
      }
    };    
  },
  // 塗りつぶし処理
  fill: function(i, j) {
    var map = this.map;
    var self = this;
    // タイル情報更新
    map.setTileByIndex(i, j, -1);
    // 水スプライト作成
    var water = Sprite('tile', UNIT, UNIT).addChildTo(this.waterGroup);
    water.frameIndex = WATER;
    water.setPosition(i * UNIT + HALF, j * UNIT + HALF);
    // 一旦非表示
    water.hide();
    // 上下左右隣のタイルを調べる
    DIR_ARRAY.each(function(dir) {
      var di = i + dir.x;
      var dj = j + dir.y;
      // 塗りつぶせる場所があれば
      if (map.checkTileByIndex(di, dj) === FLOOR) {
        // 再起呼び出し
        self.fill(di, dj);
      }
    });
  },
  // 水を表示
  showWater:function() {
    var self = this;

    this.waterGroup.children.each(function(water, i) {
      // 時間差で表示
      self.tweener.wait(50)
                  .call(function() {
                     water.show();
                  });
    });
    self.tweener.play();
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```
:::

## ソースコード(ScanLine Seed Fill)
::: details コードを見る
```javascript
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tile': 'https://cdn.jsdelivr.net/gh/alkn203/tomapiko_run@master/assets/tile.png',
    'tile_sea': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/pipo-map001_at-umi.png',
  },
};
// 定数
var UNIT = 64;
var TARGET_COLOR = 1;
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
    
    var data = [
      [2,2,2,2,2,2,2,2,2,2],
      [2,1,1,1,1,1,1,1,1,2],
      [2,2,2,2,2,2,2,1,1,2],
      [2,1,1,1,1,1,2,1,1,2],
      [2,1,1,1,1,1,2,1,1,2],
      [2,1,1,2,1,1,2,1,1,2],
      [2,1,1,2,1,1,1,1,1,2],
      [2,1,1,2,1,1,1,1,1,2],
      [2,1,1,2,1,1,1,1,1,2],
      [2,1,1,2,1,1,1,2,2,2],
      [2,1,1,1,1,1,1,1,1,2],
      [2,1,1,1,1,2,1,1,1,2],
      [2,1,2,2,2,2,2,1,1,2],
      [2,1,1,1,1,1,1,1,1,2],
      [2,2,2,2,2,2,2,2,2,2]
    ];

    this.map = phina.util.Map({
      tileWidth: UNIT,
      tileHeight: UNIT,
      imageName: 'tile',
      mapData: data,
    }).addChildTo(this);
    
    this.waterGroup = DisplayElement().addChildTo(this);
    
    var self = this;
    // タッチ時
    this.onpointend = function(e) {
      // タッチ位置からインデックス計算
      var i = (e.pointer.x / UNIT) | 0;
      var j = (e.pointer.y / UNIT) | 0;
      // 
      if (this.map.checkTileByIndex(i, j) === TARGET_COLOR) {
        this.setInteractive(false);
        // タッチした位置から塗りつぶし開始
        this.fill(i, j);
        this.showWater();
      }
    };
  },
  // 水表示
  showWater: function() {
    this.waterGroup.children.each(function(water, i) {
      // 時間差で表示
      water.tweener.wait(100 * i)
                   .call(function() {
                     water.show();
                   }).play();
    });
  },
  // 塗りつぶし
  fill: function(i, j) {
    var map = this.map;
    var self = this;
    // シードバッファ
    this.buffer = [];
    // バッファにスタック
    this.buffer.push({ i : i, j : j });
    // バッファにシードがある限り
    while (this.buffer.length > 0) {
      // シードを１つ取り出す
      var point = this.buffer.pop();
      // 塗れる左端
      var leftI = point.i;
      // 塗れる右端
      var rightI = point.i;
      // 既に塗られていたらスキップ
      if (map.checkTileByIndex(point.i, point.j) === -1) {
        continue;
      }
      // 左端を特定
      for (; 0 < leftI; leftI--) {
        if (map.checkTileByIndex(leftI - 1, point.j) !== TARGET_COLOR) {
          break;
        }
      }
      // 右端をよ特定
      for (; rightI < 9; rightI++) {
        if (map.checkTileByIndex(rightI + 1, point.j) !== TARGET_COLOR) {
          break;
        }
      }
      // 横方向を塗る
      this.paintHorizontal(leftI, rightI, point.j);
      // 上下のラインをサーチ
      if (point.j + 1 < 14) {
        this.scanLine(leftI, rightI, point.j + 1);
      }
      if (point.j - 1 >= 0) {
        this.scanLine(leftI, rightI, point.j - 1);
      }
    }
  },
  // 指定された直線範囲を塗りつぶす
  paintHorizontal: function(leftI, rightI, j) {
    var map = this.map;
    
    for (var i = leftI; i <= rightI; i++) {
       // タイル情報更新
      map.setTileByIndex(i, j, -1);
      // 指定したインデックスの子要素を得る
      var target = map.getChildByIndex(i, j);
      // 水スプライト・一旦非表示
      var sea = Sprite('tile_sea', 32, 32).addChildTo(this.waterGroup);
      sea.setPosition(target.x, target.y);
      sea.setFrameIndex(4).hide();
      sea.setSize(64, 64);
    }
  },
  // ライン上でシードをスキャンする
  scanLine: function(leftI, rightI, j) {
    var map = this.map;
    //
    while (leftI <= rightI) {
      // 塗れる最初の場所
      for (; leftI <= rightI; leftI++) {
        if (map.checkTileByIndex(leftI, j) === TARGET_COLOR) {
            break;
        }
      }
      // 右端に到達したら終わり
      if (rightI < leftI) {
        break;
      }
      // 塗れない右端を特定
      for (; leftI <= rightI; leftI++) {
        if (map.checkTileByIndex(leftI, j) !== TARGET_COLOR) {
          break;
        }
      }
      // 塗れる右端をｓシードに登録
      this.buffer.push({ i : leftI - 1, j : j});
      // シード表示
      Label({
        text: 'S',
        fontSize: UNIT * 0.8,
      }).addChildTo(this).setPosition((leftI - 1) * UNIT + 32, j * UNIT + 32);
    }
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```
:::


## runstantプロジェクト
https://runstant.com/alkn203/projects/d28dc8e2

https://runstant.com/alkn203/projects/28a1c109

## 参考にしたサイト
https://fussy.web.fc2.com/algo/algo3-1.htm

https://www.serendip.ws/archives/4797

