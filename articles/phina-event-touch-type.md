---
title: "【phina.js】タッチイベントの種類を理解する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![touch-type](/images/touch-type.gif)

## タッチイベントの種類
**phina.js**には、以下のタッチイベントが用意されています。

## onpointstart
タッチされたタイミングで発動します。

## onpointend
タッチが離れたタイミングで発動します。

## onpointmove
タッチされたまま動いている時に発動します。

## onpointstay
* タッチされたまま動かない時に発動します。
* 現在の仕様では、タッチ移動時にも発動します。

## サンプルコード
:::details コードを見る
```js
// グローバルに展開
phina.globalize();

var LABEL_COLOR = 'yellow';
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

    var label1 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 480);

    var label2 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 600);

    var label3 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 720);
    
    var label4 = Label({
      text: '',
      fill: LABEL_COLOR,
    }).addChildTo(this).setPosition(320, 840);
    
    var cnt1 = 0;
    var cnt2 = 0;
    var cnt3 = 0;
    var cnt4 = 0;
    // タッチ開始
    this.onpointstart = function() {
      cnt1++;
      label1.text = 'pointstart: ' + cnt1; 
    };
    // タッチ終了
    this.onpointend = function() {
      cnt2++;
      label2.text = 'pointend: ' + cnt2; 
    };
    // タッチしながら移動
    this.onpointmove = function() {
      cnt3++;
      label3.text = 'pointmove: ' + cnt3; 
    };
    // タッチしながら停止
    this.onpointstay = function() {
      cnt4++;
      label4.text = 'pointstay: ' + cnt4; 
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
https://runstant.com/alkn203/projects/eac67790