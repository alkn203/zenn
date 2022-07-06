---
title: "[phina.js]イベントドリブンな当たり判定を作ってみた"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
**phina.js**で当たり判定を行う時は、**update**関数の中で当たり判定用の関数を使うことがほとんどだと思います。当たり判定に関連して、[[phina.js]Colliderクラスを作ってみた](https://zenn.dev/alkn203/articles/phina-collider)では、**Accessory**を使ってUnityなどの当たり判定で使用される**Collider**と同様な自作クラスについて書きました。
今回はそれを発展させて、イベントドリブンな当たり判定ができるようにしてみます。

![collisionlayer](/images/collisionlayer.gif)

[[runstantで確認]](https://runstant.com/alkn203/projects/e70df5a9)

# 目標
オブジェクトを追加しただけで、バックグラウンドで当たり判定が行われるようにします。

# 準備
scriptタグで**phina.js**のほかに**Collider**クラスを読み込んでおきます。

```js
<script src="https://cdn.jsdelivr.net/gh/phi-jp/phina.js@v0.2.3/build/phina.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alkn203/phina-extensions@master/build/phina-extensions.min.js"></script>
```

# CollisionLayerクラス
今回作成したクラスで、このクラスの子要素として追加されたオブジェクトを当たり判定の対象にします。

```js
phina.define("CollisionLayer", {
  // 継承
  superClass: 'DisplayElement',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
  },
  // 毎フレーム更新処理
  update: function() {
    var self = this;
    
    this.children.each(function(c1, i) {
      self.children.each(function(c2, j) {
        // 子要素にColliderがあれば
        if (i !== j && c1.collider && c2.collider) {
          // 当たり判定
          if (c1.collider.hitTest(c2.collider)) {
            // ヒットイベント発火
            c1.flare('hit', {obj: c2});
            c2.flare('hit', {obj: c1});
          }
        }
      });
    });
  },
});
```

# 当たり判定
**update**関数内で行います。

```js
 update: function() {
    var self = this;
    
    this.children.each(function(c1, i) {
      self.children.each(function(c2, j) {
        // 子要素にColliderがあれば
        if (i !== j && c1.collider && c2.collider) {
          // 当たり判定
          if (c1.collider.hitTest(c2.collider)) {
            // ヒットイベント発火
            c1.flare('hit', {obj: c2});
            c2.flare('hit', {obj: c1});
          }
        }
      });
    });
  },
```

* 追加された子オブジェクトは**children**配列に格納されてますので、ループ処理でお互いの判定を行います。
* まず、そのオブジェクトに**collider**があるか調べてから、**hitTest**関数で判定を行います。
* 単純にループで比べると同じオブジェクト同士を判定して常に当たっている状態になります。これを避けるために、配列中のインデックス値が同じ場合は判定をパスしています。
* ヒットした場合は、**flare**関数で自身のイベントを発火させます。第一引数はイベント名で、第二引数にイベントに引き渡したいパラメータで、ヒットしたオブジェクトを渡しています。

# 使い方
当たり判定を行いたいオブジェクトに**collider**を設定します。

```js
// コライダー設定
    player.collider.setSize(42, 64).show();
// コライダー設定
    wall.collider.show();
```

オブジェクトのイベント実行関数に処理を書きます。

```js
// イベント関数定義
    player.onhit = function(e) {
      // ヒットしたオブジェクト名
      console.log(e.obj.name);  
    };
```

* 関数は、**on + イベント名**とする必要があります。今回の場合は、**onhit**となります。
* 関数の引数**e**からヒットしたオブジェクトの情報が得られます。

# まとめ
このようにイベントを上手く活用すれば、イベントドリブンな当たり判定処理を実現することができます。大量のオブジェクトの場合、処理が重たくなることも予想されますが、一つの拡張手段と捉えて頂ければと思います。