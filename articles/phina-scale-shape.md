---
title: "【phina.js】Shapeを拡大・縮小させる"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![scale-shape](/images/scale-shape.png)

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

## runstantプロジェクト]
http://runstant.com/alkn203/projects/c8b3a756