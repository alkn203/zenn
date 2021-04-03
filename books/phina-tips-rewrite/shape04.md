---
title: "Shape　原点変更"
---

![](https://storage.googleapis.com/zenn-user-upload/2medybekhzw1qszm851k3tfnncyd)

## Shapeの原点変更

**phina.js**では、**Shape**を始めとしたオブジェクトの原点は中心となっており、配置や回転処理などではこの原点が基準となります。
この原点は変更することができます。

## originプロパティと原点の位置
オブジェクトの原点は**origin**で指定でき、位置関係は以下のようになっています。

| origin | 位置 |
| ---- | ---- |
| (0.5, 0.5) | 中心（デフォルト）|
| (0, 0) | 左上 |
| (0, 1) | 左下 |
| (1, 0) | 右上 |
| (1, 1) | 右下 |

例えばshapeの原点を左上にしたい場合、以下のようにします。

```js
shape.origin.set(0, 0);
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
    this.backgroundColor = 'black';
    // 背景色（文字列）
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    shape.backgroundColor = 'red';
    // 背景色（RGB値）
    var shape1 = Shape().addChildTo(this).setPosition(150, 480);
    shape1.backgroundColor = '#ffff00';
    // 背景色（hsl値）
    var shape2 = Shape().addChildTo(this).setPosition(490, 480);
    shape2.backgroundColor = 'hsl(300, 75%, 50%)';
    // 透明度
    var shape3 = Shape().addChildTo(this).setPosition(320, 600);
    shape3.alpha = 0.25;
    // 非表示
    var shape4 = Shape().addChildTo(this).setPosition(320, 800);
    shape4.hide();
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
https://runstant.com/alkn203/projects/ee8654fb
