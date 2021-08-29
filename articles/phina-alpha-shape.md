---
title: "【phina.js】Shapeを透明・非表示にする"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![alpha-shape](/images/alpha-shape.png)

## Shapeの透明度指定
### alphaプロパティ
**Shape**の透明度は**alpha**プロパティで指定します。デフォルトが**1.0**で、小さくなるほど透明度が増し、 **0**で完全に透明になります。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
shape.alpha = 0.25;
```

## Shapeの非表示
### hideメソッド
**hide**メソッドで**Shape**を非表示にすることができます。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
shape.hide();
```

透明の場合は、処理上も描画の対象で当たり判定があるのに対し、非表示の場合は、そもそも描画の対象とはならないという違いがあります。

## サンプルコード
:::details コードを見る
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