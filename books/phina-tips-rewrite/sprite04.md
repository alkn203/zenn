---
title: "Sprite　フレームアニメーション"
---

![](https://storage.googleapis.com/zenn-user-upload/v5krk7yxjbpxlethssxq4knmf2dn)

**Sprite**画像でフレームアニメーションを設定します。

## スプライトシート画像を用意する
スプライトシート画像とは、以下のようにアニメーション用のコマをシートの様に並べた画像です。

* 上の画像だと、横6x縦3の計18コマから成り立っています。
* ゲーム作成ではコマのことを**フレーム**と呼ぶことが多いです。


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
    // 透明度変化アニメーション
    sp = Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
    sp.update = function() {
      // 徐々に透明にする
      sp.alpha -= 0.01;
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
https://runstant.com/alkn203/projects/e55bc28d
