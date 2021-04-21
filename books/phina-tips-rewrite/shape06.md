---
title: "Shape　種類"
---

## Shapeの種類

**phina.js**には、以下の種類の**Shape**が予め用意されています。

## RectangleShape
矩形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| cornerRadius | 角丸め値 |

```js
// RectangleShape
RectangleShape({
  width: 128,
  height: 128,
  fill: 'red',
  stroke: 'lime',
  strokeWidth: 16,
  cornerRadius: 16
}).addChildTo(this).setPosition(320, 200);
```

## CircleShape
円形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |

```js
// CircleShape
CircleShape({
  fill: 'green',
  stroke: 'white',
  strokeWidth: 16,
  radius: 64
}).addChildTo(this).setPosition(320, 400);
```

## TriangleShape
三角形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |

```js
// TriangleShape
TriangleShape({
  fill: 'purple',
  stroke: 'white',
  strokeWidth: 16,
  radius: 64
}).addChildTo(this).setPosition(320, 600);
```

## StarShape
星形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| sides | 外側の頂点数 |
| sideIndent | 凹み間隔 |

```js
// StarShape
StarShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
  sideIndent: 0.5,
}).addChildTo(this).setPosition(320, 800);
```

## PolygonShape
正多角形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| sides | 頂点数 |

```js
// PolygonShape
PolygonShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
  sides: 8,
}).addChildTo(this).setPosition(100, 480);
```

## HeartShape
ハート形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| fill | 塗りつぶし色 |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| cornerAngle | 両側の辺の角度 |

```js
// HeartShape
HeartShape({
  stroke: 'white',
  strokeWidth: 16,
  radius: 64,
}).addChildTo(this).setPosition(540, 480);    
```

## PathShape
頂点配列からパスを作成する**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| paths | 頂点配列（Vector2型） |

| メソッド | 説明 |
| ---- | ---- |
| stroke | 縁取り線色 |
| strokeWidth | 縁取り線の太さ |
| radius | 半径 |
| cornerAngle | 両側の辺の角度 |

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
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    // 移動
    shape.update = function() {
      shape.x += 2;  
      shape.y += 2;
    };
    // Shapeを作成してシーンに追加
    var shape2 = Shape().addChildTo(this).setPosition(320, 480);
    // 移動
    shape2.update = function() {
      shape2.moveBy(-2, -2);
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
https://runstant.com/alkn203/projects/024439ca
