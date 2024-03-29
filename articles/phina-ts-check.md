---
title: "[phina.js]型定義ファイルで補完してts-checkで緩く型チェックする"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","typescript","vscode"]
published: true
---

## はじめに

これまで、**phina.js**のコーディングは、主に[Runstant](https://runstant.com/)を使ってきました。コード補完はできるのですが、基本的にjavascriptのビルトイン関数やソース上にあるキーワードのみです。
以前から**phina.js**用の非公式の型定義ファイルがあることを知っていましたが、なかなか試す機会がなかったため、今回トライしてみました。

https://github.com/negiwine/phina.js.d.ts

## 検証環境
* Manjaro Linux
* Node.js v19.5.0
* npm 8.19.2
* Visual Studio Code 1.74.1

## phina.js型定義ファイルのインストール

リポジトリ記載の方法に従い、インストールします。

```sh
npm install negiwine/phina.js.d.ts
```

## ソースファイルから型定義ファイルを読み込む

* 通常は、TypeScriptファイルから読み込むことになるかと思いますが、今回は後述するVisual Studio Codeの**ts-check**機能を使いたかったので、javascriptファイルから読み込みます。
* 導入方法に従い、jsconfig.jsonを作成して以下の内容を記載します。以下は、phina.globalize()を使う場合の設定です。

```json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "phina.js": [
                "node_modules/phina.js.d.ts/globalized"
            ]
        }
    }
}
```

## コード補完の確認

ソースファイルでコード補完がされることを確認します。

![code-hint.gif](/images/code-hint.gif)

## 補完機能利用のコツ

型ファイルは、主に単一のクラスのプロパティやメソッドを補完することを目的としているため、継承クラスにおける継承元のメソッドなどの補完まではカバーしていないと思われます。

## ts-checkを使った型チェック

Visual Studio Codeでは、ソースコードの先頭に```//@ts-check```と記載することで**JavaScriptファイルのままTypeScript相当の型チェックをさせる**ことができます。

```js
//@ts-check

phina.globalize();
```

* ts-checkの特徴は、TypeScriptの型エラーが出ていても、JavaScriptの構文として正しければ実行できるという点です。
* TypeScriptでガッツリ組むまでもないプログラムで使うのが良いかもしれません。
* 上で作ったjsconfig.jsonに "checkJs": true を追記すれば、配下のjsファイル全部にチェックをかけることもできます。

## さいごに

* 今回試してみて、ts-checkについては、現在のphina.jsの仕様では、厳密に型チェックを行うことが逆効果となる可能性もあるので、使いどころが難しいと感じました。
* 型定義ファイルの方は、入力補完やメソッドが簡単に確認できるというメリットが大きいので、これからも積極的に使っていこうと思いました。