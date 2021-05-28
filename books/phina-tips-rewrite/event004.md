---
title: "タッチイベントの種類を理解する"
---

![](https://storage.googleapis.com/zenn-user-upload/sg6qoblnqv522qpsfozka6w6sett)

## onpointstart
* タッチされたタイミングで発動します。

```js
// シーンにタッチイベント登録
this.onpointstart = function(e) {
  // タッチされた座標を調べる
  var tx = e.pointer.x;
  var ty = e.pointer.y;
  // タッチされた座標にShapeを移動
  shape.setPosition(tx, ty);
};
```

## onpointend
* タッチが離れたタイミングで発動します。

```js
// シーンにタッチイベント登録
this.onpointstart = function(e) {
  // タッチされた座標を調べる
  var tx = e.pointer.x;
  var ty = e.pointer.y;
  // タッチされた座標にShapeを移動
  shape.setPosition(tx, ty);
};
```

## onpointmove
* タッチされたまま動いている時に発動します。

```js
// シーンにタッチイベント登録
this.onpointstart = function(e) {
  // タッチされた座標を調べる
  var tx = e.pointer.x;
  var ty = e.pointer.y;
  // タッチされた座標にShapeを移動
  shape.setPosition(tx, ty);
};
```

## onpointstay
* タッチされたまま動かない時に発動します。

```js
// シーンにタッチイベント登録
this.onpointstart = function(e) {
  // タッチされた座標を調べる
  var tx = e.pointer.x;
  var ty = e.pointer.y;
  // タッチされた座標にShapeを移動
  shape.setPosition(tx, ty);
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
    // シーンにタッチイベント登録
    this.onpointstart = function(e) {
      // タッチされた座標を調べる
      var tx = e.pointer.x;
      var ty = e.pointer.y;
      // タッチされた座標にShapeを移動
      shape.setPosition(tx, ty);
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
https://runstant.com/alkn203/projects/72f02607
