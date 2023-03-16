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

htmlファイル
```html
todo
```

オーソドックスなセレクトボックスですので、特に説明はいらないかと思います。

### cssファイル

```css

#mycanvas {
  margin: 0 auto;
  width: 30%;
  display: block;
}
```

jsファイル
```js
todo
```

phina.js側の処理としては、選択された値をShapeのプロパティに
設定するだけです。

## 【サンプル2】ファイル選択ダイアログで選択した画像をSpriteの画像にする

htmlファイル
```html
todo
```

* オーソドックスなファイル選択ダイアログです。
* 複数選択は不可とし、画像ファイルのみ受け付けるようにしました。

jsファイル
```js
todo
```

phina.js側の処理としては、以下のとおりです。
* 選択された画像ファイルと同じサイズの**Canvas**を作成する。
* Canvasにイメージを描画する。
* Spriteのコンストラクタ引数に作成したCanvasを指定する。

ード補完の確

ソースファイルでコード補完がされることを確認します。

![code-hint.gif](/images/code-hint.gif)
jsconfig.jsonに "checkJs": true を追記すれば、配下のjsファイル全部にチェックをかけることもできます。

## さいごに

今回は、素のhtmlを例として使いましたが、基本的にその他のJSライブラリとも連携可能と考えられますので、普段使っているライブラリで試してみてはいかがでしょうか。
