---
title: "Spriteのサイズを指定する"
---

![](https://storage.googleapis.com/zenn-user-upload/nxuuoq5228trljswymy2x38cqlbg)


**Sprite** は、**Shape** と同じ基底クラスを継承していますので、共通のプロパティやメソッドを使用することができます。

## Spriteのサイズ指定

```js
// サイズ指定
var sp1 = Sprite('tomapiko').addChildTo(this).setPosition(320, 340);
sp1.width = 128;
// 一括指定
Sprite('tomapiko').addChildTo(this).setPosition(320, 480).setSize(128, 128);
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
    // 一括指定
    Sprite('tomapiko').addChildTo(this).setPosition(320, 480).setSize(128, 128);
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
https://runstant.com/alkn203/projects/5e7ac43a
