---
title: "【phina.js】フレームアニメーションの速度を動的に変更する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

その他のTipsは[こちら](https://zenn.dev/alkn203/articles/phina-tips-rewrite)

![change-animation-speed](/images/change-animation-speed.gif)

## フレームアニメーション設定の変更
外部ファイルとして読み込まれたフレームアニメーション設定を後から変更する方法について説明します。例として、フレームアニメーションの速度を変更します。

## フレームアニメーション設定を外部ファイルに定義
フレームアニメーション設定は外部ファイルとして定義して、アセットとして読み込むことができます。

```js
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
   // フレームアニメーション情報
  spritesheet: {
    'tomapiko_ss': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/tmss/tomapiko.tmss',
  },
};
```

## アニメーションのfreaquencyプロパティ
* **anim** を **FrameAnimation** クラスのインスタンスとした場合、**ss** で **SpriteSheet** を参照することができます。
* **getAnimation**で指定したアニメーションを参照することができるので、そのプロパティ**freaquency**の値を変更します。

```js
// アニメーション速度変更
anim.ss.getAnimation('left').frequency += 1;
```

以下のサンプルでは、画面をタッチするとフレームアニメーションの速度が遅くなっていきます。

## サンプルコード
:::details コードを見る
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko_ss.png',
  },
   // フレームアニメーション情報
  spritesheet: {
    'tomapiko_ss': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/tmss/tomapiko.tmss',
  },
};
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
    this.backgroundColor = 'skyblue';
    // スプライト画像作成
    var sprite = Sprite('tomapiko', 64, 64).addChildTo(this);
    sprite.setPosition(320, 480);
    // スプライトにフレームアニメーションをアタッチ
    var anim = FrameAnimation('tomapiko_ss').attachTo(sprite);
    // アニメーションを指定する
    anim.gotoAndPlay('left');
    // 画面タッチ処理
    this.onpointend = function() {
      // アニメーション速度変更
      anim.ss.getAnimation('left').frequency += 1;
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
    // アセット読み込み
    assets: ASSETS,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```
:::

## runstantプロジェクト
https://runstant.com/alkn203/projects/ca5546df