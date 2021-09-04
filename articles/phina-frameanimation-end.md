---
title: "【phina.js】フレームアニメーションの終了を検知する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![frameanimation-end](/images/frameanimation-end.gif)

## フレームアニメーションの終了を検知する
* ゲーム作成において、フレームアニメーションが終了した後に何か処理をしたい時があるかと思いまます。
* この場合、**FrameAnimation** クラスの**finished**プロパティの値が**true**かどうかを調べると便利です。
* サンプルでは爆発アニメーションが終了したら、自身を消去するようにしています。（consoleに結果表示）

```js
// 毎フレーム処理
update: function() {
  // アニメーションが終わったら自身を消去
  if (this.anim.finished) { // ←ここで判定
    this.remove();
    console.log('removed');
  }
},
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
  // 毎フレーム処理
  update: function() {
    // アニメーションが終わったら自身を消去
    if (this.anim.finished) {
      this.remove();
      console.log('removed');
    }
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
https://runstant.com/alkn203/projects/4256f208