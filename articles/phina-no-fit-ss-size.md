---
title: "【phina.js】スプライトのサイズがスプライトシートに定義されたサイズにフィットしないようにする"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![no-fit-ss-size](/images/no-fit-ss-size.gif)

## スプライトのサイズがスプライトシートに定義されたサイズにフィットしないようにする
フレームアニメーションを使うためには、**SpriteSheet** を定義する必要がありますが、デフォルトだと実際のコード中でスプライトのサイズを変えてもスプライトシート(json)に書かれた**width**と**height**に戻ってしまい、スプライトを拡大して表示したい時に意図した結果になりません。

```js
// スプライトシート
spritesheet: {
  'explosion_ss':
  {
    "frame": {
      "width": 20, // ←この値にフィットされる
      "height": 20,// ←この値にフィットされる
      "cols": 16,
      "rows": 1,
    },
  }
}
```

* 回避するためには、**FrameAnimation** クラスのプロパティ**fit**を**false**にします。
* これで実際に変更したサイズで正しく表示されるようになります。
* サンプルでは**20X20**で切り出した画像を**5倍**で表示しています。

```js
// 親クラスの初期化
this.superInit('explosion', 20, 20); // ←実際の画像の切り取りサイズ
// SpriteSheetをスプライトにアタッチ
var anim = FrameAnimation('explosion_ss').attachTo(this);
// スプライトシートのサイズにフィットさせない
anim.fit = false; // ←ここ
//アニメーションを再生する
anim.gotoAndPlay('start');
// サイズ変更
this.setSize(20*5, 20*5); // ←サイズ変更
```

## サンプルコード
:::details コードを見る
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'explosion': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/explosion.png',
  },
  // スプライトシート
  spritesheet: {
    'explosion_ss':
    {
      "frame": {
        "width": 20,
        "height": 20,
        "cols": 16,
        "rows": 1,
      },
      // アニメーション
      "animations" : {
        "start": {
          "frames": Array.range(16),
          "next": "",
          "frequency": 1,
        },
      }
    },
  },
};
// メインシーン
phina.define(`MainScene`, {
  // DisplaySceneを継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // グループ
    this.explosionGroup = DisplayElement().addChildTo(this);
  },
  // 毎フレーム処理
  update: function(app) {
    if (app.frame % 20 === 0) {
      // 爆発
      var explosion = Explosion().addChildTo(this.explosionGroup);
      explosion.x = Random.randint(0, 640);
      explosion.y = Random.randint(0, 960);
      explosion.blendMode = 'lighter';
    }
  },
});
// 爆発クラス
phina.define(`Explosion`, {
  // Spriteを継承
  superClass: 'Sprite',
  // 初期化
  init: function() {
    // 親クラスの初期化
    this.superInit('explosion', 20, 20);
    // SpriteSheetをスプライトにアタッチ
    var anim = FrameAnimation('explosion_ss').attachTo(this);
    // スプライトシートのサイズにフィットさせない
    anim.fit = false;
    //アニメーションを再生する
    anim.gotoAndPlay('start');
    // サイズ変更
    this.setSize(20*5, 20*5);
    // 参照用
    this.anim = anim;
  },
});
// メイン処理
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    assets: ASSETS,
    startLabel: 'main',
  });
  // 実行
  app.run();
});
```
:::

## runstantプロジェクト
https://runstant.com/alkn203/projects/6c71743a