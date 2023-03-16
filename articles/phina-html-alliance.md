---
title: "[phina.js]html要素との連携サンプル"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html"]
published: false
---

## はじめに

**phina.js**は、デフォルトでは実行時に**html5**の**canvas**を作成して、そこに描画を行います。
その仕様を踏まえて、工夫次第で通常の**html**要素と上手く連携することも可能です。
今回は、その簡単なサンプルを紹介します。

## phina.jsの実行結果を表示するcanvasを指定する

* **phina.js**では、実行結果を表示する**canvas**を別途指定することができます。
* **html**で**canvas**要素を作成し、**css**で**style**を指定します。

### htmlファイル

```html
<!doctype html>
 
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    
    <title>phina x selectbox</title>
    <meta name="description" content="phina x selectbox" />
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <script src="../phina.min.js"></script>
    <script src="main.js"></script>
    <!-- 別canvas -->
    <canvas id="mycanvas"></canvas>
  </body>
</html>
```

### cssファイル

```css
#mycanvas {
  margin: 0 auto;
  width: 30%;
  display: block;
}
```

### jsファイル

```js
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // 画面サイズ
    width: 300,
    height: 300,
    // 表示先のcanvasを指定
    query: '#mycanvas',
    // MainScene から開始
    startLabel: 'main',
    // 画面にフィットさせない
    fit: false,
  });
  // 実行
  app.run();
});
```

**phina.js**側では、**main**関数の**query**オプションに作成した**canvas**の**id**を渡します。
これで前準備はOKです。

## 【サンプル1】セレクトボックスでShapeの色を変更する

### htmlファイル

```html
<!doctype html>
 
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    
    <title>phina x selectbox</title>
    <meta name="description" content="phina x selectbox" />
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <script src="../phina.min.js"></script>
    <script src="main.js"></script>
    <!-- 別canvas -->
    <canvas id="mycanvas"></canvas>
    <!-- セレクトボックス -->
    <div class="selection">
      <select id="selector">
        <option value="red">赤</option>
        <option value="blue">青</option>
        <option value="yellow">黄</option>
      </select>
    </div>
  </body>
</html>
```

オーソドックスなセレクトボックスですので、特に説明はいらないかと思います。

### cssファイル

```css
#mycanvas {
  margin: 0 auto;
  width: 30%;
  display: block;
}

.selection {
  text-align: center;
  padding: 10px;
}

#selector {
  display: inline-block;
}
```

### jsファイル

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
  init: function(options) {
    // 親クラス初期化
    this.superInit(options);
    // 背景色
    this.backgroundColor = 'black';
    // Shape
    var shape = RectangleShape({
      width: 128,
      height: 128,
      fill: 'red',
      stroke: null,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
    // ドロップダウンリストを取得
    var selector = document.getElementById('selector');
    // イベント設定
    selector.addEventListener('change', function(event) {
      // 選択された値をShapeの色に設定
      shape.fill = event.target.value;
    });
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // 画面サイズ
    width: 300,
    height: 300,
    // 表示先のcanvasを指定
    query: '#mycanvas',
    // MainScene から開始
    startLabel: 'main',
    // 画面にフィットさせない
    fit: false,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

**phina.js**側の処理としては、選択された値を**Shape**のプロパティに設定するだけです。

## 【サンプル2】ファイル選択ダイアログで選択した画像をSpriteの画像にする

### htmlファイル

```html
<!doctype html>
 
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />
    
    <title>phina x selectbox</title>
    <meta name="description" content="phina x selectbox" />
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <script src="../phina.min.js"></script>
    <script src="main.js"></script>
    <!-- 別canvas -->
    <canvas id="mycanvas"></canvas>
    <!-- ファイルダイアログ -->
    <div class="selection">
      <input type="file" id="example" accept="image/*">
    </div>
  </body>
</html>
```

* オーソドックスなファイル選択ダイアログです。
* 複数選択は不可とし、画像ファイルのみ受け付けるようにしました。

### jsファイル

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
    // シーンを参照できるようにする
    const self = this;
    // ファイルダイアログ取得
    const fileInput = document.getElementById('example');
    //
    const handleFileSelect = () => {
      // 選択されたファイル
      const file = fileInput.files[0];
      // FileReaderオブジェクトを作成
      const reader = new FileReader();
      // ファイルが読み込まれたときに実行
      reader.onload = function (e) {
        // 画像のURL
        const imageUrl = e.target.result;
        // img要素を作成
        const image = document.createElement("img");
        // 画像のURLをimg要素にセット
        image.src = imageUrl; 
        
        image.onload = () => {
          // img要素からスプライト作成
          // 新規canvas作成
          var canvas = phina.graphics.Canvas().setSize(image.width, image.height);
          // canvasに描画
          canvas.context.drawImage(image, 0, 0);
          // スプライト作成
          const sprite = Sprite(canvas).addChildTo(self);
          sprite.setPosition(self.gridX.center(), self.gridY.center());
        };
      };
      // ファイルを読み込む
      reader.readAsDataURL(file);
    };
    //
    fileInput.addEventListener('change', handleFileSelect);
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // 表示先のcanvasを指定
    query: '#mycanvas',
    // MainScene から開始
    startLabel: 'main',
    // 画面にフィットさせない
    fit: false,
  });
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```

**phina.js**側の処理のポイントとしては、以下の部分です。

```js
var canvas = phina.graphics.Canvas().setSize(image.width, image.height);
// canvasに描画
canvas.context.drawImage(image, 0, 0);
// スプライト作成
const sprite = Sprite(canvas).addChildTo(self);
```

* 選択された画像ファイルと同じサイズの**phina.graphics.Canvas**を作成する。
* **Canvas**にイメージを描画する。
* **Sprite**のコンストラクタ引数に作成した**Canvas**を渡す。

## さいごに

今回は、素の**html**を例として使いましたが、基本的にその他のJSライブラリとも連携可能と考えられますので、普段使っているライブラリで試してみてはいかがでしょうか。
