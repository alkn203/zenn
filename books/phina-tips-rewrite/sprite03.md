---
title: "Sprite　透明度"
---

![](https://storage.googleapis.com/zenn-user-upload/w6j72no9nandzi7r82cqamwwtd85)

**Sprite** は、**Shape** と同じ基底クラスを継承していますので、共通のプロパティやメソッドを使用することができます。

## Spriteの透明度
**Shape**と同じく**alpha**プロパティに指定します。

```js
// 透明度変化アニメーション
sp = Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
sp.update = function() {
  // 徐々に透明にする
  sp.alpha -= 0.01;
};
```

## サンプルコード
::: details コードを見る
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko.png',
  },
};
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'skyblue';
    // サイズ指定
    var sp1 = Sprite('tomapiko').addChildTo(this).setPosition(320, 340);
    sp1.width = 128;
    Sprite('tomapiko').addChildTo(this).setPosition(320, 480).setSize(128, 128);
    // 回転
    var sp2 = Sprite('tomapiko').addChildTo(this).setPosition(320, 600);
    sp2.rotation = 45;
    Sprite('tomapiko').addChildTo(this).setPosition(320, 700).setRotation(15);
    // 回転アニメーション
    sp3 = Sprite('tomapiko').addChildTo(this).setPosition(320, 800);
    sp3.update = function() {
      sp3.rotation++;
    };
    // 拡大・縮小
    sp4 = Sprite('tomapiko').addChildTo(this).setPosition(200, 800);
    sp4.scaleY = 1.5;
    Sprite('tomapiko').addChildTo(this).setPosition(440, 800).setScale(0.5, 0.5);
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
https://runstant.com/alkn203/projects/a1919f6a
