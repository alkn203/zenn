---
title: "Shape　移動"
---

![](https://storage.googleapis.com/zenn-user-upload/7224zy06lr9c5xiecwvervsbfxmu)

## Shapeの移動

**Shape** を始めとしたオブジェクトの移動は、オブジェクトの**update**関数で位置を変更させるのが一般的です。

## update関数で位置を変更する
**update** 関数は毎フレーム呼ばれるので、関数内で以下のように記述することで **Shape** を移動させることができます。

```js

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
