---
title: "Shape　種類"
---

![](https://storage.googleapis.com/zenn-user-upload/oij9s1rvmhg8jyj9tttxj6s18v1c)

## Shapeの種類

**phina.js**には、以下の種類の**Shape**が予め用意されています。

## RectangleShape
矩形の**Shape**です。

| プロパティ | 説明 |
| ---- | ---- |
| (0.5, 0.5) | 中心（デフォルト）|
| (0, 0) | 左上 |
| (0, 1) | 左下 |
| (1, 0) | 右上 |
| (1, 1) | 右下 |

```js
// 移動
shape.update = function() {
  shape.x += 2;  
  shape.y += 2;
};
```

## moveBy関数を使った移動
オブジェクトに用意されている**moveBy**関数を使って移動させることもできます。

```js
// 移動
shape.update = function() {
  shape.moveBy(-2, -2);
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
