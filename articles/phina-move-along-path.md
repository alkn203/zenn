---
title: "【phina.js】パスに沿ったオブジェクト移動"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# パスに沿ったオブジェクトの移動
ゲームを作成していると、動く床などのように一定のルートに従ってオブジェクトを移動させたい時があると思います。 **phina.js**を使って、自分なりにその処理を実装してみました。

## 実行サンプル画面

![](https://storage.googleapis.com/zenn-user-upload/0b33acb0b158d48826c46f3e.gif)

- オブジェクトの移動には、非同期処理が行える**tweener**を利用します。
- **tweener** の内部処理で使われている**add** 関数で処理をスタックさせています。
- **tweener**は、基本的にスタックされた順に非同期処理されるので、次の移動先である各頂点の位置を順番に与えることで、結果としてパスに沿った移動が可能になります。

## 課題
- 今回のサンプルでは、移動速度が一定になるように各頂点の距離が同一になるように配置しています。
- 一定の時間で移動させるのが**tweener**の処理ですので、距離が変わると移動速度も変わることになります。
- 各頂点の距離に応じて**duration**を変えると速度を一定にすることが可能になると思われますが、その辺は次回の課題にしたいと思います。

## サンプルコード
::: details コードを見る
```javascript
// グローバルに展開
phina.globalize();
//
var SCREEN_WIDTH = 506;
var SCREEN_HEIGHT = 253;
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: SCREEN_WIDTH,
      height: SCREEN_HEIGHT,

    });
    // 背景色
    this.backgroundColor = 'black';
    // グリッド
    var gx = this.gridX;
    var gy = this.gridY;
    var vec = Vector2;
    // 頂点データ
    var verts = [vec(gx.span(1), gy.center()),
                 vec(gx.span(3), gy.center(-2)),
                 vec(gx.span(5), gy.center()),
                 vec(gx.span(7), gy.center(-2)),
                 vec(gx.span(9), gy.center()),
                 vec(gx.span(11), gy.center(-2)),
                 vec(gx.span(13), gy.center()),
                 vec(gx.span(15), gy.center(-2))];
    // パス描画用
    var path = PathShape({
      width: SCREEN_WIDTH,
      height: SCREEN_HEIGHT,
      paths: verts,
    }).addChildTo(this);
    
    var ball = CircleShape({
      radius: 8,
    }).addChildTo(this);
    
    this.moveAlongPath(ball, verts);
  },
  // ターゲットを与えられたパスに沿って移動させる
  moveAlongPath: function(target, path) {
    // 最初の頂点へ移動
    target.position = path.first;
    // 頂点群をループ
    path.each(function(vert, i) {
      if (i > 0) {
        // tweenをスタック
        target.tweener._add({
          type: 'tween',
          mode: 'to',
          props: {x: vert.x, y: vert.y},
          duration: 1000,
        });
      }
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    width: SCREEN_WIDTH,
    height: SCREEN_HEIGHT,
    // MainScene から開始
    startLabel: 'main',
  });
  // 実行
  app.run();
});

```
:::

## runstantプロジェクト
https://runstant.com/alkn203/projects/10bfc984
