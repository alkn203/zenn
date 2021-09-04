---
title: "【phina.js】Labelの文字色を変更する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![change-label-color](/images/change-label-color.png)

## ラベルの文字色を変更する
ラベルの文字色は、**fill** プロパティで後から変更可能です。

```js
var label2 = Label('Time is money').addChildTo(this);
label2.setPosition(320, 600);
label2.fontSize = 48;
label2.fill = 'red';
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
    this.backgroundColor = 'silver';
    // ラベル表示
    var label = Label('Time is money').addChildTo(this);
    label.setPosition(320, 480);
    var label2 = Label('Time is money').addChildTo(this);
    label2.setPosition(320, 600);
    label2.fontSize = 48;
    label2.fill = 'red';
    var label3 = Label('Time is money').addChildTo(this);
    label3.setPosition(320, 720);
    label3.fontSize = 60;
    label3.fill = 'yellow';
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
https://runstant.com/alkn203/projects/bc9aac6a