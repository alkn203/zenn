---
title: "Shape　=回転・拡大縮小・透明化="
---

![](https://storage.googleapis.com/zenn-user-upload/v85h9ckw9wvrw4jn9g40ekjf4yp0)


## Shapeの回転
**Shape**の回転角度指定には複数の方法があります。どちらの場合も **度(degree)**で指定します。

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
var shape = Shape().addChildTo(this).setPosition(320, 480).setRotation(45);
```

#### Shapeのコンストラクタで指定
位置などと一緒にコンストラクタでも指定できます。

```js
var shape = Shape({
  x: 320,
  y: 480,
  rotation: 45
}).addChildTo(this)
```

#### 回転アニメーション
**Shape**の**update**関数でプロパティ**rotation**の値を変更することで、回転アニメーションをさせることができます。

```js
// スプライト回転
sprite.update = function() {
  sprite.rotation++;
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
var shape = Shape().addChildTo(this).setPosition(320, 480).setScale(1.2, 1.2);
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
