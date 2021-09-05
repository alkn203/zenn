---
title: "【phina.js】更新イベントを設定する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![event-update](/images/event-update.gif)

## 更新イベント処理（update）
* 更新処理を行うためには、オブジェクトに**update**関数を実装します。
* **update**関数が実装されていれば、その内容が毎フレーム実行されます。

```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this).setPosition(320, 480);
// 更新処理
shape.update = function() {
  shape.moveBy(2, 2);
};
```

## 更新イベント処理（on + enterframe）
**update** では1つの処理しか登録できません。一方、**on** と**enterframe**を組み合わせると複数のメソッドを登録可能で、その内容が並行して実行されます。

```js
// 毎フレーム更新処理
shape2.on('enterframe', function() {
  // 移動
  shape2.moveBy(-2, -2);  
});
// 毎フレーム更新処理
shape2.on('enterframe', function() {
  // 回転
  shape2.rotation += 2;  
});
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
    // 更新処理
    shape.update = function() {
      shape.moveBy(2, 2);
    };
    
    var shape2 = Shape().addChildTo(this).setPosition(320, 480);
    // 毎フレーム更新処理
    shape2.on('enterframe', function() {
      // 移動
      shape2.moveBy(-2, -2);  
    });
    // 毎フレーム更新処理
    shape2.on('enterframe', function() {
      // 回転
      shape2.rotation += 2;  
    });
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
https://runstant.com/alkn203/projects/4a760b10