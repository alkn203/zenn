---
title: "【phina.js】上方向だけすり抜ける床を作る"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# 上方向だけすり抜ける床
* ジャンプゲームなどでは、上方向にすり抜け下方向にはすり抜けない床がよくあります。
* 今回は基本的なジャンプ処理とともに、**phina.js** を使って自分なりに実装してみました。

# 動作サンプル
まずは以下のサンプルを確認して下さい。 画面タッチでキャラがジャンプしますが、上方向にはブロックをすり抜けて、その後に下のブロックに着地します。

![only-up](/images/only-up.gif)

[runstantで確認](https://runstant.com/alkn203/projects/61d44965)

# プレイヤーとブロックの作成
プレイヤーとブロックは、それぞれ**Sprite**クラスを継承して作成しました。

```javascript
// プレイヤークラス
phina.define('Player', {
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tomapiko', SPRITE_SIZE, SPRITE_SIZE);
    // フレームアニメーションをアタッチ
    this.anim = FrameAnimation('tomapiko_ss').attachTo(this);
    // 縦移動速度
    this.vy = 0;
  },
  // 縦方向移動
  moveY: function() {
    this.vy += GRAVITY;
    this.y += this.vy;
  },
});
```

```javascript
// ブロッククラス
phina.define('Block', {
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tiles', SPRITE_SIZE, SPRITE_SIZE);
    // タイルセットの指定フレームを表示   
    this.setFrameIndex(4);
  },
});
```

アセットで読み込んだタイルセットからブロックの画像のインデックスをsetFrameIndexで指定しています。

# プレイヤーとブロックの配置

```javascript
     // グループ
    var blockGroup = DisplayElement().addChildTo(this);
    // グリッド
    var gx = this.gridX;
    var gy = this.gridY;
    // プレイヤー
    var player = Player().addChildTo(this);
    player.setPosition(gx.center(), gy.span(13.5));
    player.state = 'FALLING';
    // ブロック
    [6, 9, 12, 15].each(function(i) {
      var block = Block().addChildTo(blockGroup);
      block.setPosition(gx.center(), gy.span(i));
```

- プレイヤーとブロックの配置には、**Grid**を利用しています。
- 今回はプレイヤーの状態毎に処理を分けるようにしており、管理用の変数**state**を用意しました。

# プレイヤーの状態毎に処理を分ける
毎フレーム更新される**update**関数内でプレイヤーの状態毎に処置を分けるアプローチをとり、プレイヤーの状態を以下の３つに分類しました。

- ブロックの上にいる**ON_BLOCK**
- ジャンプ中である**JUMPING**
- 落下中である**FALLING**

当たり判定の場合、上方向にはすり抜けるので**JUMPING**の時は当たり判定を行わず、残りの状態で当たり判定を行えばよいということになります。

```javascript
 // 毎フレーム更新処理
  update: function(app) {
    var player = this.player;
    var state = this.player.state;
    var p = app.pointer;
    // プレイヤーの状態で分ける
    switch (state) {
      // ブロックの上
      case 'ON_BLOCK':
        // タッチ開始
        if (p.getPointingStart()) {
          player.vy = -JUMP_POWER;
          player.state = 'JUMPING';
          // アニメーション変更
          player.anim.gotoAndPlay('fly');
        }
        // 縦あたり判定
        this.collisionY();
        break;
      // ジャンプ中
      case 'JUMPING':
        player.moveY();
        // 下に落下開始
        if (player.vy > 0) {
          player.state = 'FALLING';
        }
        break;
      // 落下中  
      case 'FALLING':
        player.moveY();
        this.collisionY();
        break;
    }
  },
```

- **update**関数の引数**app**のプロパティ**pointer**から、タッチの状態を取得します。
- ブロックに乗っている時はジャンプできるので、タッチされた場合、上方向へ移動量を設定してプレイヤーの状態を**JUMPING**に変更します。併せて、当たり判定を行います。
- ジャンプ中の時は、縦方向移動処理を行い、縦方向の移動の向きが下になった場合プレイヤーの状態を**FALLING**に変更します。
- 落下中の時は、縦方向移動処理と当たり判定を行っています。

# 当たり判定
当たり判定を行う**collisionY**という関数を用意しました。

```javascript
// 縦方向の当たり判定
  collisionY: function() {
    var player = this.player;
    // 床に乗っている場合は強引に当た(り判定を作る
    var vy = player.vy === 0 ? 4: player.vy;
    //var vy = player.vy;
    // 当たり判定用の矩形
    var rect = Rect(player.left, player.top + vy, player.width, player.height);
    var result = false;
    // ブロックグループをループ
    this.blockGroup.children.some(function(block) {
      // ブロックとのあたり判定
      if (Collision.testRectRect(rect, block)) {
        // 移動量
        player.vy = 0;
        // 位置調整
        player.bottom = block.top;
        //
        player.state = 'ON_BLOCK';
        // アニメーション変更
        player.anim.gotoAndPlay('stand');
        
        result = true;
        return true;
      }
    });
    // 当たり判定なし
    if (!true) {
      player.state = 'FALLING';
    }
  },
```

- 最初にプレイヤーの縦方向の速度を調べます。0の時にも無理やり速度を与えているのは、ブロックの上に乗っている時でも当たり判定を生じさせることで、位置調整した時にキャラが宙に浮いて上下にブレるのを防ぐためです。
- 次に、当たり判定用の矩形を作成しています。プレイヤーの位置から速度分ずらした位置で当たり判定を行うことで、着地時にプレイヤーがブロックにめり込むのを防ぐことができます。
- ブロックをループして当たり判定を行い、ヒットしたらプレイヤーの状態を**ON_BLOCK**に変更してループを抜けます。ループには**some**を使っていますが、もちろん通常の**for**ループでも構いません。
- 当たり判定がなければ落下中ということになるので、プレイヤーの状態を**FALLING**とします。

# まとめ
今回のアプローチはあくまでも自己流です。他に効率的な処理があるかもしれませんが、今回のコードをベースにすれば簡単なジャンプゲームが作れるのではないかと思います。

# サンプルコード
::: details コードを見る
```js
phina.globalize();
// アセット
var ASSETS = {
  // 画像
  image: {
    'tomapiko': 'https://cdn.jsdelivr.net/gh/phi-jp/phina.js@develop/assets/images/tomapiko_ss.png',
    'tiles': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc/tiles.png',
  },
  // フレームアニメーション情報
  spritesheet: {
    'tomapiko_ss':
    {
      "frame": {
        "width": 64,
        "height": 64,
        "rows": 3,
        "cols": 6
      },
      "animations": {
        "stand": {
          "frames": [0],
          "next": "stand",
          "frequency": 4
        },
        "fly": {
          "frames": [1, 2, 3],
          "next": "fly",
          "frequency": 4
        },
      }
    }
  }
};
// 定数
var SCREEN_WIDTH  = 640; // 画面横サイズ
var SCREEN_HEIGHT = 960; // 画面縦サイズ
var SPRITE_SIZE   = 64;
var GRAVITY       = 9.8 / 18; // 重力
var JUMP_POWER    = 15; // ジャンプ力
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'skyblue';
    // グループ
    var blockGroup = DisplayElement().addChildTo(this);
    // グリッド
    var gx = this.gridX;
    var gy = this.gridY;
    // プレイヤー
    var player = Player().addChildTo(this);
    player.setPosition(gx.center(), gy.span(13.5));
    player.state = 'FALLING';
    // ブロック
    [6, 9, 12, 15].each(function(i) {
      var block = Block().addChildTo(blockGroup);
      block.setPosition(gx.center(), gy.span(i));
    });
    // クラス全体で参照できるようにする
    this.player = player;
    this.blockGroup = blockGroup;
  },
  // 毎フレーム更新処理
  update: function(app) {
    var player = this.player;
    var state = this.player.state;
    var p = app.pointer;
    // プレイヤーの状態で分ける
    switch (state) {
      // ブロックの上
      case 'ON_BLOCK':
        // タッチ開始
        if (p.getPointingStart()) {
          player.vy = -JUMP_POWER;
          player.state = 'JUMPING';
          // アニメーション変更
          player.anim.gotoAndPlay('fly');
        }
        // 縦あたり判定
        this.collisionY();
        break;
      // ジャンプ中
      case 'JUMPING':
        player.moveY();
        // 下に落下開始
        if (player.vy > 0) {
          player.state = 'FALLING';
        }
        break;
      // 落下中  
      case 'FALLING':
        player.moveY();
        this.collisionY();
        break;
    }
  },
  // 縦方向の当たり判定
  collisionY: function() {
    var player = this.player;
    // 床に乗っている場合は強引に当た(り判定を作る
    var vy = player.vy === 0 ? 4: player.vy;
    //var vy = player.vy;
    // 当たり判定用の矩形
    var rect = Rect(player.left, player.top + vy, player.width, player.height);
    var result = false;
    // ブロックグループをループ
    this.blockGroup.children.some(function(block) {
      // ブロックとのあたり判定
      if (Collision.testRectRect(rect, block)) {
        // 移動量
        player.vy = 0;
        // 位置調整
        player.bottom = block.top;
        //
        player.state = 'ON_BLOCK';
        // アニメーション変更
        player.anim.gotoAndPlay('stand');
        
        result = true;
        return true;
      }
    });
    // 当たり判定なし
    if (!true) {
      player.state = 'FALLING';
    }
  },
});
// プレイヤークラス
phina.define('Player', {
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tomapiko', SPRITE_SIZE, SPRITE_SIZE);
    // フレームアニメーションをアタッチ
    this.anim = FrameAnimation('tomapiko_ss').attachTo(this);
    // 縦移動速度
    this.vy = 0;
  },
  // 縦方向移動
  moveY: function() {
    this.vy += GRAVITY;
    this.y += this.vy;
  },
});
// ブロッククラス
phina.define('Block', {
  superClass: 'Sprite',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit('tiles', SPRITE_SIZE, SPRITE_SIZE);
    // タイルセットの指定フレームを表示   
    this.setFrameIndex(4);
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    // アセット読み込み
    assets: ASSETS,
  });
  
  app.run();
});
```
:::
