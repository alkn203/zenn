---
title: "ハドルの配置"
---

## はじめに

この本では、ゲームプログラミングの定番とも言える**ブロック崩し**を**phina.js**で作った過程を解説します。

## 今回の目標

以下のようにハドルを配置します。

![breakout-tut-1.png](/images/breakout-tut-1.png)

## 完成コード

今回の完成コードは、以下のとおりです。

::: details コードを見る

```js
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
// アセット
var ASSETS = {
  // 画像
  image: {
    'paddle': 'https://cdn.jsdelivr.net/gh/alkn203/phina-game-prototypes@main/breakout/assets/paddle.png',
  },
};
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // グリッド
    this.gx = Grid(SCREEN_WIDTH, 10);
    this.gy = Grid(SCREEN_HEIGHT, 30);
    // パドル
    this.paddle = Paddle().addChildTo(this);
    this.paddle.x = this.gx.center();
    this.paddle.y = this.gy.span(26);
  },
});
/*
 * パドル
 */
phina.define('Paddle', {
  // Spriteクラスを継承
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('paddle');
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    // MainSceneから開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  app.run();
});


```

:::

## runstantプロジェクト

https://runstant.com/alkn203/projects/47e7ee64

## コード説明

### 定数の定義

```js
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
```

* ゲーム画面のサイズを定義しています。

### アセットの定義

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'paddle': 'https://cdn.jsdelivr.net/gh/alkn203/phina-game-prototypes@main/breakout/assets/paddle.png',
  },
};
```

* パドルの画像をアセットとして定義します。

```js
// メイン
phina.main(function() {
  var app = GameApp({
    // MainSceneから開始
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  app.run();
});
```

* アセットは、main関数のパラメータとして与えて読み込みます。

### Paddleクラス

メイン処理の前にパドルクラスについて説明します。

```js
/*
 * パドル
 */
phina.define('Paddle', {
  // Spriteクラスを継承
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('paddle');
  },
});

```

* **phina.define**でクラスを定義します。
* 画像を扱う**Sprite**クラスを継承します。
* **this.superInit**で親クラスにパラメーターとしてアセット名を与えて初期化します。

### メインシーン

```js
  // 背景色
  this.backgroundColor = 'black';
```

**Scene**の背景色を指定しています。

```js
  // グリッド
  this.gx = Grid(SCREEN_WIDTH, 10);
  this.gy = Grid(SCREEN_HEIGHT, 30);
```

**Grid**クラスを使って、配置用の情報を生成しています。ここで1グリッドの大きさが決まります。

```js
  // パドル
  this.paddle = Paddle().addChildTo(this);
  this.paddle.x = this.gx.center();
  this.paddle.y = this.gy.span(26);
```

* パドルのインスタンスを作成して、グリッドを利用して配置しています。
* **center**で中央に配置することができます。