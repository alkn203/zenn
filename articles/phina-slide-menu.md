---
title: "phina.jsでスライドメニュー作成"
emoji: "📋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5"]
published: true
---

## はじめに
以前に[Runstant](https://runstant.com)でコーディングしていたのですが、[phina.js](https://phinajs.com/)でスライドメニューのようなものを作成しましたので、作り方を説明したいと思います。

## 実行サンプル
![](https://storage.googleapis.com/zenn-user-upload/06xa4k972cthr68e6918hhoebdip)

以下の**runstant**プロジェクトを実行すると、画面左側から時間差でメニューがスライド表示されるのが確認できるかと思います。
https://runstant.com/alkn203/projects/7a4c6a1c

## サンプルコード
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
    // グループ
    var menuGroup = DisplayElement().addChildTo(this);
    // メニューを並べる
    Array.range(2, 10, 2).each(function(i) {
      var menu = Button({text: 'menu' + i}).addChildTo(menuGroup);
      menu.setPosition(this.gridX.span(-3), this.gridY.span(i));
      // クリック時処理
      menu.onpush = function() {
        console.log(menu.text);
      };
    }, this);
    // 時間差で表示
    menuGroup.children.each(function(menu, i) {
      menu.tweener.wait(i * 100).by({x: 250}, 400, 'easeOutCubic').play();
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

## コード説明
ポイントを順に説明します。

### メニューグループの作成
```js
// グループ
var menuGroup = DisplayElement().addChildTo(this);
```

* **DisplayElement** クラスを利用してメニューグループを作成し、**MainScene** に追加しています。

### メニューを並べる
```js
// メニューを並べる
Array.range(2, 10, 2).each(function(i) {
  var menu = Button({text: 'menu' + i}).addChildTo(menuGroup);
  menu.setPosition(this.gridX.span(-3), this.gridY.span(i));
  // クリック時処理
  menu.onpush = function() {
    console.log(menu.text);
  };
}, this);
```

* メニューとして**Button**を使用しています。
* 拡張**Array**クラスのメソッド**range**と繰り返し用の**each**メソッドを使って、インデックス2つ飛ばしで5個のボタンを作成しています。
* **Scene**にあらかじめ設定されている**Grid**を使って、ボタンを並べて配置しています。一旦画面の左外側で見えない場所に配置しています。
* ボタンのクリック時に**onpush**メソッドが呼ばれるので、例としてボタンのテキストをログに表示しています。

### メニューのスライド表示
```js
// 時間差で表示
menuGroup.children.each(function(menu, i) {
  menu.tweener.wait(i * 100).by({x: 250}, 400, 'easeOutCubic').play();
});
```

* メニューグループの子要素に対して、**tweener** で移動アニメーションを設定しています。
* **wait** で、その後の処理の実行までの待ち時間を一定時間ずらして設定し、**by** で一定時間（400ミリ秒）かけて、**x** の正方向に一定距離移動させています。
* **easeOutCubic**は、イージングと呼ばれるものの一種で、動きに緩急の効果をつけることができます。

## 使い方
htmlファイルで **phina.js** を以下のように読み込みます。

```html
<script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
```

## さいごに
* 今回は、ゲームのUIに使えるかもしれない、ちょっとしたテクニックの紹介でした。
* **phina.js**には、ゲーム画面用のUIはあらかじめ用意されていませんが、工夫次第で自分好みのUIを作ることもできると思います。
