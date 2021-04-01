---
title: "Shape　背景色・透明度"
---

![](https://storage.googleapis.com/zenn-user-upload/2medybekhzw1qszm851k3tfnncyd)

## Shapeの背景色指定

**Shape**の背景色は**backgroundColor**プロパティで指定します。 **CSS**と同じ感覚で指定できます。

#### 文字列で指定
```js
shape.backgroundColor = 'red';
```

#### 16進数で指定
```js
shape.backgroundColor = '#ffff00';
```

#### RGB値で指定
```js
shape.backgroundColor = `rgb(0, 255, 255)`;
```
#### hsl値で指定
```js
shape.backgroundColor = `hsl(300, 75%, 50%)`;
```

## Shapeの透明度指定
#### alphaプロパティ
**Shape**の透明度は**alpha**プロパティで指定します。デフォルトが**1.0**で、小さくなるほど透明度が増し、**0**で完全に透明になります。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
shape.alpha = 0.25;
```

## Shapeの非表示
#### hideメソッド
**hide**メソッドで**Shape**を非表示にすることができます。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
shape.hide();
```

:::message
透明の場合は、処理上も描画の対象で当たり判定があるのに対し、非表示の場合は、そもそも描画の対象とはならないという違いがあります。
:::

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
