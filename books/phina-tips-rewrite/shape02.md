---
title: "Shape　=位置・サイズ・背景色指定="
---

![](https://storage.googleapis.com/zenn-user-upload/v85h9ckw9wvrw4jn9g40ekjf4yp0)


## Shapeの位置指定
**Shape**の位置指定方法はいくつかあります。

####  x,yプロパティに直接指定
```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this);
// 位置指定
shape.x = 320;
shape.y = 480;
```

#### setPosition関数で一括指定
**setPosition** 関数を使えば、 **x, y** の値を一括で指定することができ、生成から一気にチェインメソッドで繋げて書くこともできるので便利です。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
```

#### **Shape**のコンストラクタで指定
```js
var shape = Shape({
  x: 320,
  y: 480
}).addChildTo(this)
```

#### 移動量で指定
* **moveBy**関数を使えば、**x, y**の移動量で位置を変更することができます。

```js
shape.setPosition(320, 480).moveBy(100, 200);
```

#### ベクトル値の加算で指定
* **Vector2**クラスを使ってベクトル値の加算で位置指定する方法もあります。

```js
var v = Vector2(100, 200);
shape.position.add(v);
```

## Shapeのサイズ指定
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

## 背景色指定

背景色は**backgroundColor**プロパティで指定します。**CSS**と同じ感覚で指定できます。

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
    // Shapeを作成してシーンに追加
    var shape = Shape().addChildTo(this);
    // 位置指定
    shape.x = 320;
    shape.y = 480;
    // setPosition
    var shape2 = Shape().addChildTo(this).setPosition(320, 600);
    // コンストラクタ
    var shape3 = Shape({
      x: 320,
      y: 720,
    }).addChildTo(this);
    // moveBy
    var shape4 = Shape().addChildTo(this).setPosition(320, 480).moveBy(200, 200);
    // ベクトル
    var shape5 = Shape().addChildTo(this).setPosition(320, 480);
    var v = Vector2(-200, 200);  
    shape5.position.add(v);
    // 幅
    var shape6 = Shape().addChildTo(this).setPosition(320, 100);
    shape6.width = 128;
    // 高さ
    var shape7 = Shape().addChildTo(this).setPosition(320, 300);
    shape7.height = 128;
    // setSize
    var shape8 = Shape().addChildTo(this).setPosition(150, 300);
    shape8.setSize(128, 128);
    // コンストラクタ
    var shape9 = Shape({
      // 位置・幅・高さ指定
      x: 480,
      y: 300,
      width: 128,
      height: 256,
    }).addChildTo(this);
    // 背景色（文字列）
    var shape10 = Shape().addChildTo(this).setPosition(320, 850);
    shape10.backgroundColor = 'red';
    // 背景色（RGB値）
    var shape11 = Shape().addChildTo(this).setPosition(150, 850);
    shape11.backgroundColor = '#ffff00';
    // 背景色（hsl値）
    var shape12 = Shape().addChildTo(this).setPosition(490, 850);
    shape12.backgroundColor = 'hsl(300, 75%, 50%)';
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
https://runstant.com/alkn203/projects/0b1aea5e
