---
title: "【phina.js】Shapeの背景色を指定する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![color-shape](/images/color-shape.png)

## Shapeの背景色指定

**Shape**の背景色は**backgroundColor**プロパティで指定します。 **CSS**と同じ感覚で指定できます。

### 文字列で指定
```js
shape.backgroundColor = 'red';
```

### 16進数で指定
```js
shape.backgroundColor = '#ffff00';
```

### RGB値で指定
```js
shape.backgroundColor = `rgb(0, 255, 255)`;
```
### hsl値で指定
```js
shape.backgroundColor = `hsl(300, 75%, 50%)`;
```

## サンプルコード
:::details コードを見る

```js
/// グローバルに展開
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
https://runstant.com/alkn203/projects/017932f4