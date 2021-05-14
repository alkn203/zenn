---
title: "Labelの文字を変更する"
---

![](https://storage.googleapis.com/zenn-user-upload/6mgrscdl0bxdief1yv638k45x8wx)

## ラベルの文字を変更する
ラベルの文字は、**text** プロパティで後から変更可能です。
以下のサンプルでは、画面がタッチされたらラベルの文字を変更しています。



```js
// ラベル表示
var label = Label('Time is money').addChildTo(this);
label.setPosition(this.gridX.center(), this.gridY.center());
// ラベル文字変更
this.onpointstart = function() {
  label.text = 'Money is time';
};
```

## サンプルコード
::: details コードを見る
```js
// グローバルに展開
phina.globalize();
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
    this.backgroundColor = 'silver';
    // ラベル表示
    var label = Label('Time is money').addChildTo(this);
    label.setPosition(this.gridX.center(), this.gridY.center());
    // ラベル文字変更
    this.onpointstart = function() {
      label.text = 'Money is time';
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
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```
:::

## runstantプロジェクト
https://runstant.com/alkn203/projects/093751a0
