---
title: "[phina.js]型定義ファイルを使った入力補完とts-checkを使った簡易型チェック"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","typescript","vscode"]
published: false
---

# はじめに
これまで、phina.jsのコーディングは、主にrunstantを使ってきました。
コード補完はできるのですが、基本ソース上にあるキーワードのみです。
phina.js用の非公式の型定義ファイルがあることを知っていましたが、なかなか試す機会がなかったため
今回、トライしてみることにしました。

リポジトリへのリンク

# 検証環境
Manjaro Linux
Nodejs
npm
Visual Studio Code

# phina.js型定義ファイルのインストール
リポジトリ記載の方法に従い、インストールします。

```bash
npm install
```

# ソースファイルから型定義ファイルを読み込む
通常は、TypeScriptファイルから読み込むことになるかと思いますが
今回は後述するVisual Studio Codeのts-check機能を使いたかったので、javascriptファイルから読み込みます。




有名な**Box2d**ではありますが、いざ使おうとすると、結構面倒な前処理を自前で書く必要があります。
**phina.js**には、このような処理を書かずに**Box2dWeb**をより簡単に使うことができるクラスが用意されています。

| クラス名          | 役割               |
|:-----------------|:------------------|
| phina.box2d.Box2dLayer | 主にBox2dを使用するための前処理を行います。|
| phina.box2d.Box2dBody  | 物体の定義やBox2dとphina.jsとの間の座標及びスケールの調整などを行います。 |

ライブラリの読み
**Box2dWeb**をhtmlで読み込みます。[本家](https://github.com/hecht-software/box2dweb)のものだと何故か上手く読み込まれなかったので、今回は@phiさんが[forkしてbugfixしたバージョン](https://github.com/phi-jp/box2dweb)を使用します。

```html
    <script src="https://rawgit.com/phi-jp/box2dweb/master/Box2D.js"></script>
    <script src="http://cdn.rawgit.com/phi-jp/phina.js/v0.2.1/build/phina.js"></script>
```

# レイヤーの作成
最初に**Box2d**表示用のレイヤーを作って**Scene**に追加します。

```js
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
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
    // Box2d用レイヤー作成
    var layer = Box2dLayer({
      width: SCREEN_WIDTH,
      height: SCREEN_HEIGHT,
    }).addChildTo(this);
  }
});
```

レイヤーは、画面サイズと同じサイズにしないと位置がずれるので注意しましょう。

# 物体を追加する（落下するボール）
簡単な例として、落下するボールを追加してみます。

![falling_ball.gif](/images/falling_ball.gif)

[[runstantで動作確認]](https://runstant.com/alkn203/projects/011d43c4)

```js
    // Box2d用レイヤー作成
    var layer = Box2dLayer({
      width: SCREEN_WIDTH,
      height: SCREEN_HEIGHT,
    }).addChildTo(this);
    // ボール
    var ball = CircleShape().addChildTo(this);
    ball.setPosition(this.gridX.center(), this.gridY.center());
    // Box2dのデバッグ表示が見えるようにする
    ball.alpha = 0.5;
    // Box2dオブジェクトを作成してballにアタッチ
    layer.createBody({
      type: 'dynamic', 
      shape: 'circle',
    }).attachTo(ball);
```
* 先に**phina.js**のオブジェクトである**CircleShape**を作成して、 **Scene**に追加しています。
* その後、 **Box2dLayer**のメソッドである**createBody**で**Box2d**オブジェクトを作成して、 **CircleShape**にアタッチしています。
* **createBody**のパラメータのうち、 **type**には重力などが適用される**dynamic**を指定し、形状を表す**shape**には**circle**を指定しています。
* アタッチすると、以後**Box2d**側の動きと**phina.js**側の動きが同期するようになります。

# 固定の床
このままではボールが落ちるだけなので、障害物となる床を配置します。

![falling_bounce_ball.gif](/images/falling_bounce_ball.gif)

[[runstantで動作確認]](https://runstant.com/alkn203/projects/dc8f3bf1)

```js
    // 床
    var floor = RectangleShape({
      width: SCREEN_WIDTH,
      height: 32,
    }).addChildTo(this);
    floor.setPosition(this.gridX.center(), this.gridY.span(15));
    // Box2dのデバッグ表示が見えるようにする
    ball.alpha = 0.5;
    // Box2dオブジェクトを作成してfloorにアタッチ
    layer.createBody({
      type: 'static', 
      shape: 'box',
    }).attachTo(floor);
```

* 床は**phina.js**側では**RectangleShape**で表示します。
* 床は固定で動かないので**type**を**static**にします。
* 形状を表す**shape**は**box**にします。

これで、ボールが床に当たり跳ねるようになりました。

# 床に角度をつける
ここまでだと余り**Box2d**の恩恵に預かっていないので、より物理的な動きを確認するために床を傾けてみます。

![falling_rolling_ball.gif](/images/falling_rolling_ball.gif)

[[runstantで動作確認]](http://runstant.com/alkn203/projects/6f06df88)

```js
    // Box2dオブジェクトを作成してfloorにアタッチ
    layer.createBody({
      type: 'static', 
      shape: 'box',
    }).attachTo(floor).body.SetAngle(Math.degToRad(10));
```

* **phina.js**に慣れていると**shape**に**rotation**を設定したくなりますが、　**phina.js**側は**Box2d**の動きを追従しているだけなので、　**Box2d**のメソッドを使う必要があります。
* **Box2dBody**クラスのプロパティ**body**で**Box2d**のオブジェクトを参照できるので、　**SetAngle**メソッドで角度(radian)を設定しています。

これで、ボールが床に落ちて転がっていきます。

# おわりに
以上が**phina.js**から**Box2d**を使う簡単な方法です。まだ紹介出来ていない部分も多くあると思いますし、私自身もまだまだ勉強中てす。

* Shapeではなくスプライトを使う
* Box2dのデバック描画をしないようにする

などが次にトライできるテーマでしょう。
結構簡単に連携できることがお分かり頂けたと思いますので、皆さんの方でも色々と試して遊んでみて下さい。
