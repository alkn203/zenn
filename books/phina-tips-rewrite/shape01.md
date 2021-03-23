---
title: "Shape　=表示="
---

オブジェクトの基本形である**Shape**を以下のように画面に表示します。位置を指定しない場合は、画面左上に表示されます。

![](https://storage.googleapis.com/zenn-user-upload/7d2h3e44vsseakjl8bsmxj0fhmz4)

[[runstantで実行]](https://runstant.com/alkn203/projects/c5ac89af)

## コード

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
## コード説明

* **Shape**クラスのコンストラクタで生成します。コンストラクタの前に**new**をつける必要はありません。
* **addChildTo(this)** で現在の **Scene** に追加します。**this** は **MainScene** を指しています。

```js
this.addChild(shape)
```

としても同じですし

```js
var shape = Shape().addChildTo(this)
```

として1行にまとめることもできます。

* 位置が指定されていない時は、画面左上(0,0)に表示されます。
* 変数に代入しなくても表示されますが、後にプロパティを操作することが多いので、とりあえずは変数に代入しておいた方が良いでしょう。
