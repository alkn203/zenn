---
title: "phina.jsのプログラミング環境を構築する"
---

## プログラミング環境を準備する
ゲーム作成を始める前に、**phina.js** のプログラミング環境を構築します。
このチュートリアルでは、Webブラウザ上でコーディング・実行が可能な[runstant](https://runstant.com)を使用しますので、基本的に必要なものはWebブラウザだけです。
もちろん、好きなエディタを使ってローカルで開発することも可能ですが、クライアントサイドで実行されるjavascriptは、セキュリティ上の理由からローカルファイルへのアクセスが制限されていますので、回避策として、エディタの拡張機能で用意されているローカルサーバーを通しての実行をおすすめします。

## phina.jsを読み込む
**runstant**の **View(html)** タブのコードは以下のようになっています。

```js
<!doctype html>

<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, user-scalable=no" />
    <meta name="apple-mobile-web-app-capable" content="yes" />

    <title>${title}</title>
    <meta name="description" content="${description}" />

    <style>${style}</style>
  </head>
  <body>
    <script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
    <script>${script}</script>
  </body>
</html>
```
