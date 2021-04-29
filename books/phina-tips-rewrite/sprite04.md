---
title: "Sprite　フレームアニメーション"
---

![](https://storage.googleapis.com/zenn-user-upload/v5krk7yxjbpxlethssxq4knmf2dn)

**Sprite**画像でフレームアニメーションを設定します。

## スプライトシート画像を用意する
スプライトシート画像とは、以下のようにアニメーション用のコマをシートの様に並べた画像です。

![](https://storage.googleapis.com/zenn-user-upload/0apyqr2u79w8e2n0yuim6uqlwgp2)

* 上の画像だと、横6x縦3の計18コマから成り立っています。
* ゲーム作成ではコマのことを**フレーム**と呼ぶことが多いです。

## スプライトシート画像を読み込む
通常の画像と同じように**ASSETS**として読み込みます。

```js
// 透明度変化アニメーション
sp = Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
sp.update = function() {
  // 徐々に透明にする
  sp.alpha -= 0.01;
};
```

## フレームアニメーション情報を読み込む
次に、フレームアニメーション情報が定義された**json**形式のファイルを読み込みます。
今回はファイルからではなく**ASEETS**として、コード内に定義しています。

```js

```

* **frames**にアニメーションに使いたいフレーム番号の範囲を書きます。 **0**から始まることに注意してください。
* **next**に次のアニメーションを指定します。同じ名前にするとループします。
* **frequency**でアニメーションの間隔を指定します。小さくすれば速くなり、大きくすれば遅くなります。

## スプライトシート画像の作成
スプライトシート画像は、通常の画像と同じく**Sprite**クラスを使って作成しますが、アセット名の次の引数で**１フレームの画像サイズ**を指定します。

```js

```

## フレームアニメーションの設定

```js

```

* **FrameAnimation**クラスのコンストラクタにスプライトシートのアセット名を指定して、スプライトにアタッチします。
* **gotoAndPlay**関数にアニメーション名を指定すると、そのアニメーションが自動で再生されます。


## サンプルコード
::: details コードを見る
```js
// グローバルに展開
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phinajs/phina.js@develop/assets/images/tomapiko.png',
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
    // 透明度変化アニメーション
    sp = Sprite('tomapiko').addChildTo(this).setPosition(320, 480);
    sp.update = function() {
      // 徐々に透明にする
      sp.alpha -= 0.01;
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
https://runstant.com/alkn203/projects/e55bc28d
