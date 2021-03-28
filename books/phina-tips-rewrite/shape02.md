---
title: "Shape　サイズ指定・回転・拡大・縮小"
---

![](https://storage.googleapis.com/zenn-user-upload/bfambczlmqdkqx1mmebcql37uwt3)

## Shapeのサイズ指定
**Shape**のサイズ指定には複数の方法があります。
#### 幅指定

幅は**width**プロパティで指定します。

```js
shape.width = 128;
```

#### 高さ指定

高さは**height**プロパティで指定します。

```js
shape.height = 128;
```

#### 幅・高さを一括指定
**setSize**関数を使えば、幅と高さを一括で指定できます。

```js
shape.setSize(128, 256);
```

#### コンストラクタ内で指定
位置指定と同じくコンストラクタ内で幅・高さを指定することも可能です。

```js
    var shape = Shape({
      // 位置・幅・高さ指定
      x: 320,
      y: 480,
      width: 128,
      height: 256,
    }).addChildTo(this);
```

## Shapeの回転
**Shape**の回転角度指定には複数の方法があります。どちらの場合も **度=degree**で指定します。

####  rotaitionプロパティに直接指定
```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this).setPosition(320, 480);
// 回転指定
shape.rotation = 45;
```

#### setRotation関数で指定
**setRotation**関数を使うと、生成から一気にチェインメソッドで繋げて書くことができます。

```js
var shape = Shape().addChildTo(this).setPosition(320, 600).setRotation(15);
```

#### Shapeのコンストラクタで指定
位置などと一緒にコンストラクタでも指定できます。

```js
var shape = Shape({
  x: 320,
  y: 720,
  rotation: 60
}).addChildTo(this);
```

#### 回転アニメーション
**Shape**の**update**関数でプロパティ**rotation**の値を変更することで、回転アニメーションをさせることができます。

```js
// スプライト回転
shape.update = function() {
  shape.rotation++;
};
```

## Shapeの拡大縮小
**Shape**の拡大縮小には複数の方法があります。どちらの場合も **1.0**を基準として、小さければ縮小、大きければ拡大になります。

#### scaleX scaleY プロパティに直接指定
```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
// 横方向に拡大
shape.scaleX = 1.5;
```

#### setScale関数で指定
**setScale**関数を使うと、縦横の拡大縮小をまとめて指定できます。また、生成から一気にチェインメソッドで繋げて書くことができます。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480).setScale(0.5, 0.5);
```

:::message
サイズ変更と拡大・縮小の違いは、実際のサイズが変更されるかどうかです。拡大・縮小は見た目は変わっても実際のサイズは変更されませんので、当たり判定の時などに注意が必要です。
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
    // 幅
    var shape = Shape().addChildTo(this).setPosition(320, 100);
    shape.width = 128;
    // 高さ
    var shape2 = Shape().addChildTo(this).setPosition(320, 300);
    shape2.height = 128;
    // setSize
    var shape3 = Shape().addChildTo(this).setPosition(150, 300);
    shape3.setSize(128, 128);
    // コンストラクタ
    var shape4 = Shape({
      // 位置・幅・高さ指定
      x: 480,
      y: 300,
      width: 128,
      height: 256,
    }).addChildTo(this);
    // 回転
    var shape5 = Shape().addChildTo(this).setPosition(320, 480);
    // 回転指定
    shape5.rotation = 45;
    // setRotation
    var shape6 = Shape().addChildTo(this).setPosition(320, 600).setRotation(15);
    // コンストラクタ
    var shape = Shape({
      x: 320,
      y: 720,
      rotation: 60
    }).addChildTo(this);

    var shape7 = Shape().addChildTo(this).setPosition(320, 860);
    // 回転アニメーション
    shape7.update = function() {
      shape7.rotation++;
    };
    // 拡大
    var shape8 = Shape().addChildTo(this).setPosition(150, 860);
    shape8.scaleX = 1.5;
    // 縮小
    var shape9 = Shape().addChildTo(this).setPosition(480, 860).setScale(0.5, 0.5);
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
https://runstant.com/alkn203/projects/c8b3a756
