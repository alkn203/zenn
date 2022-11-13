---
title:"【phina.js】ゲーム作成チュートリアル（15パズル）（その１）【ピースの配置】`
---
※これまでのチュートリアルは[phina.js チュートリアル集](https://qiita.com/alkn203/items/2150f739d383b4fd5a92)にまとめています。

→[（その２）【数字の表示】](https://qiita.com/alkn203/items/c293dcac4a7046233d5e)

## はじめに
このチュートリアルでは、有名なスライドパズルである**15パズル**を**phina.js**で作っていきたいと思います。

## 今回の目標
以下のように、15パズルのピースを配置します。

![15puzzle-tut-1.png](https://qiita-image-store.s3.amazonaws.com/0/67114/25a75b28-ec5e-3af0-ddd5-b9a759432cf5.png)

コードは以下のとおりです。

```js
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
/*
 * メインシーン
 */
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
    // ピースグループ
    var pieceGroup = DisplayElement().addChildTo(this);
    // ピース配置
    PIECE_NUM_XY.times(function(spanX) {
      PIECE_NUM_XY.times(function(spanY) {
        // ピース作成
        var piece = Piece().addChildTo(pieceGroup);
        // Gridを利用して配置
        piece.x = grid.span(spanX) + PIECE_OFFSET;
        piece.y = grid.span(spanY) + PIECE_OFFSET;
      });
    });
  },
});
/*
 * ピースクラス
 */
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
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

<a href="http://runstant.com/alkn203/projects/bf291944" target="_blank">[runstantで確認]</a>

## コード説明
### 定数の定義
メインシーンの前にピースサイズなどを定数として定義します。

```js
// 定数
var SCREEN_WIDTH = 640;            // 画面横サイズ
var SCREEN_HEIGHT = 960;           // 画面縦サイズ
var GRID_SIZE = SCREEN_WIDTH / 4;  // グリッドのサイズ
var PIECE_SIZE = GRID_SIZE * 0.95; // ピースの大きさ
var PIECE_NUM_XY = 4;              // 縦横のピース数
var PIECE_OFFSET = GRID_SIZE / 2;  // オフセット値
```

### ピースクラスの定義
後々の利便性を考え、**phina.define**で以下のようにクラス化します。

```js
// ピースクラス
phina.define('Piece', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: PIECE_SIZE,
        height: PIECE_SIZE,
        cornerRadius: 10,
        fill: 'silver',
        stroke: 'white',
      });
    },
});
```

* **Piece**クラスは**phina.js**で最初から用意されている**RectangleShape**（矩形）クラスを継承し、**init**関数内で親クラス(RectangleShape）にパラメータを渡して初期化しています。
* ピースは正方形ですので、**width**と**height**には同じ**PIECE_SIZE**を与えています。
* **cornerRadius**プロパティを設定することで、角丸の四角形にすることができます。
* **fill**は塗りつぶしの色、**stroke**は枠の色です。

**phina.js**には他にも円や三角形などの**Shape**（基本図形）が用意されており、どれも最小限のパラメータで描画することができるので、簡単な動作確認の際に重宝します。
**Shape**の使い方については、作者である[phi](https://twitter.com/phi_jp)さんの記事[phina.js を使って様々な図形を表示してみよう](http://phiary.me/phinajs-shape-collection/)が参考になります。

### ピースの配置
今回の目的であるピースの配置について説明します。

```js
// グリッド
var grid = Grid(SCREEN_WIDTH, PIECE_NUM_XY);
// ピースグループ
var pieceGroup = DisplayElement().addChildTo(this);
// ピース配置
PIECE_NUM_XY.times(function(spanX) {
  PIECE_NUM_XY.times(function(spanY) {
    // ピース作成
    var piece = Piece().addChildTo(pieceGroup);
    // Gridを利用して配置
    piece.x = grid.span(spanX) + PIECE_OFFSET;
    piece.y = grid.span(spanY) + PIECE_OFFSET;
  });
});
```

* 画面幅をピースの並び数で区割りした**Grid**を作成しています。**Grid**クラスの使い方については、[【phina.js】Gridクラスを使いこなそう](http://qiita.com/alkn203/items/d176a10d4e38d15e4062)を参考にして下さい。

* **DisplayElement**クラスを使って、**pieceGroup**という名前のグループを作っています。グループ管理については、[【phina.js】グループ管理の基本テクニック](http://qiita.com/alkn203/items/8ad0b80175d23d03bd49)を参考にして下さい。

* ピースの配置は2重ループ処理で行いますが、今回は、**phina.js**独自の**times**メソッドを使用しています。**PIECE_NUM_XY**には**4**という数値が入っていますので、引数で与えられた**function**内の処理を**4回繰り返す**という意味になります。**spanX**と**spanY**には、インデックス値である**0～3**の数値がループ処理で入ってきますので、これを上手く利用します。
* 次にピースを作成して、先に作った**pieceGroup**に追加しています。

* 最後に、グリッドを利用してピースを配置しています。定数の定義部分でグリッドのサイズよりピースのサイズを少し小さくしていますが、これにより**padding**に似た効果を得ることができます。なお、**phina.js**でのオブジェクトの座標値はオブジェクトの原点（デフォルトではオブジェクトの中心）となりますので、**PIECE_OFFSET**で位置を調整しています。**PIECE_OFFSET**の箇所を削除して実行してみると、結果の違いが分かるかと思います。

* 作成した**pieceGroup**を**MainScene**に**addChildTo**することで、ピースが画面に表示されます。

## 今回はここまで
ピースは配置できましたが、現時点ではただのタイルの集まりですね。
次回は、ピースに数字を表示させたいと思います。

→[（その２）【数字の表示】](https://qiita.com/alkn203/items/c293dcac4a7046233d5e)
