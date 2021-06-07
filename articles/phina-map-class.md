---
title: "【phina.js】Mapクラスを作ってみた"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

## はじめに

**RPG** などで固定マップで単純な当たり判定を行いたい時は、**Map** データと当たり判定データを読み込んで行う方法が便利です。
今回は、**enchant.js** にあった**Map**クラスを参考に**phina.js**版を作ってみました。

![](https://storage.googleapis.com/zenn-user-upload/e410bdb5fed39ae657ead2bf.gif)

## コンストラクタ

```javascript
var map = phina.util.Map({
  tileWidth: 64,
  tileHeight: 64,
  imageName: 'tile',
  mapData: data,
  collisionData: collision,
}).addChildTo(this);
```

## プロパティ

| プロパティ         | 説明             |
| :------------ | :------------- |
| tileWidth     | タイルの幅          |
| tileHeight    | タイルの高さ         |
| imageName     | タイルセット画像の名前    |
| mapData       | マップデータの2次元配列   |
| collisionData | タイル衝突判定用の2次元配列 |

## メンバ関数

| 関数               | 説明                     |
| :--------------- | :--------------------- |
| hitTest          | マップとの衝突判定を行う(座標から)     |
| hitTestByIndex   | マップとの衝突判定を行う(インデックスから) |
| checkTile        | タイルが何か調べる(座標から)        |
| checkTileByIndex | タイルが何kか調べる(インデックスから)    |
| setTile          | タイルを更新する（座標から）               |
| setTileByIndex   | タイルを更新する（インデックスから）               |
| getChild   |  子要素を得る（座標から） |
| getChildByIndex   |  子要素を得る（インデックスから） |

## おわりに

本格的なマップ作成には、タイルマップエディタが必要になってくると思いますが、簡単なゲームであれば、この程度の機能でも使えるのではないでしょうか。   

## サンプルコード
::: details コードを見る
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tile': 'https://cdn.jsdelivr.net/gh/alkn203/tomapiko_run@master/assets/tile.png',
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
  // スプライトシート
  spritesheet: {
    "tomapiko_ss": {
      // フレーム情報
      "frame": {
        "width": 64, // 1フレームの画像サイズ（横）
        "height": 64, // 1フレームの画像サイズ（縦）
        "cols": 6, // フレーム数（横）
        "rows": 3, // フレーム数（縦）
      },
      // アニメーション情報
      "animations": {
        "left": { // アニメーション名
          "frames": [12,13,14], // フレーム番号範囲
          "next": "left", // 次のアニメーション
          "frequency": 4, // アニメーション間隔
        },
        "right": { 
          "frames": [15,16,17], 
          "next": "right", // 次のアニメーション
          "frequency": 4, // アニメーション間隔
        },
        "up": { 
          "frames": [9,10,11], 
          "next": "up", // 次のアニメーション
          "frequency": 4, // アニメーション間隔
        },
        "down": { 
          "frames": [6,7,8], 
          "next": "down", // 次のアニメーション
          "frequency": 4, // アニメーション間隔
        },
      }
    },
  }
};
// 定数
var UNIT = 64;
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
    
    var collision = [
      [1,1,1,1,1,1,1,1,1,1],
      [1,0,0,0,0,0,0,0,0,1],
      [1,1,1,1,1,1,1,0,0,1],
      [1,0,0,0,0,0,1,0,0,1],
      [1,0,0,0,0,0,1,0,0,1],
      [1,0,0,1,0,0,1,0,0,1],
      [1,0,0,1,0,0,0,0,0,1],
      [1,0,0,1,0,0,0,0,0,1],
      [1,0,0,1,0,0,0,0,0,1],
      [1,0,0,1,0,0,0,1,1,1],
      [1,0,0,0,0,0,0,0,0,1],
      [1,0,0,0,0,1,0,0,0,1],
      [1,0,1,1,1,1,1,0,0,1],
      [1,0,0,0,0,0,0,0,0,1],
      [1,1,1,1,1,1,1,1,1,1]
    ];
    
    var map = phina.util.Map({
      tileWidth: 64,
      tileHeight: 64,
      imageName: 'tile',
      mapData: data,
      collisionData: collision,
    }).addChildTo(this);
    // スプライト画像作成
    var player = Sprite('tomapiko', 64, 64).addChildTo(this);
    // 初期位置
    player.setPosition(UNIT, UNIT).setOrigin(0, 0);
    player.moving = false;
    // スプライトにフレームアニメーションをアタッチ
    player.anim = FrameAnimation('tomapiko_ss').attachTo(player);
    // アニメーションを指定
    player.anim.gotoAndPlay('right');
    // 毎フレーム処理
    player.update = function(app) {
      if (player.moving) return;
      
      var key = app.keyboard;
      var array = [['left',-1,0],['right',1,0],['up',0,-1],['down',0,1]];
      // 上下左右移動
      array.each(function(elem) {
        var e0 = elem[0];
        var e1 = elem[1];
        var e2 = elem[2];
        if (key.getKey(e0)) {
          // マップとの当たり判定
          if (map.hitTest(player.x + e1 * UNIT, player.y + e2 * UNIT)) {
            return;
          }
          else {
            player.moving = true;
            player.anim.gotoAndPlay(e0);
            // 移動処理
            player.tweener
                  .clear()
                  .to({x: player.x + e1 * UNIT, y: player.y + e2 * UNIT}, 200)
                  .call(function() {
                    player.moving = false;  
                  });
          }
        }
      });
    };
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    //startLabel: 'main',
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
https://runstant.com/alkn203/projects/05d8a818

## ソースコード
https://github.com/alkn203/phina-extensions/blob/master/util/map.js
