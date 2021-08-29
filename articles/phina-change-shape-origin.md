---
title: "【phina.js】Shapeの原点を変更する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![change-shape-origin](/images/change-shape-origin.gif)

## Shapeの原点

**phina.js**では、**Shape**を始めとしたオブジェクトの原点はオブジェクトの中心となっており、配置や回転処理などではこの原点が基準となります。
この原点は変更することができます。

## 原点の位置と変更
オブジェクトの原点は**setOrigin**で指定でき、位置関係は以下のようになっています。

| 指定値 | 位置 |
| ---- | ---- |
| (0.5, 0.5) | 中心（デフォルト）|
| (0, 0) | 左上 |
| (0, 1) | 左下 |
| (1, 0) | 右上 |
| (1, 1) | 右下 |

例えば**Shape**の原点を左上にしたい場合、以下のようにします。

```js
shape.setOrigin(0, 0);
```

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
    // Shapeを作成してシーンに追加
    var shape = Shape().addChildTo(this).setPosition(320, 480);
    // 回転
    shape.update = function() {
      shape.rotation += 2;  
    };
    // 左上を原点に
    var shape2 = Shape().addChildTo(this).setPosition(320, 240).setOrigin(0, 0);
    // 回転
    shape2.update = function() {
      shape2.rotation += -2;  
    };
    // 右下を原点に
    var shape3 = Shape().addChildTo(this).setPosition(320, 720).setOrigin(1, 1);
    // 回転
    shape3.update = function() {
      shape3.rotation += -2;  
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
https://runstant.com/alkn203/projects/20a758db