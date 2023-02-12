---
title: "[phina.js]型定義ファイルで補完して緩く型チェックする"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","typescript","vscode"]
published: false
---

## はじめに

これまで、**phina.js**のコーディングは、主に[Runstant](https://runstant.com/)を使ってきました。
コード補完はできるのですが、基本ソース上にあるキーワードのみです。
**phina.js**用の非公式の型定義ファイルがあることを知っていましたが、なかなか試す機会がなかったため、今回トライしてみることにしました。

https://github.com/negiwine/phina.js.d.ts

## 検証環境

* Node.js v19.5.0
* npm 8.19.2
* Visual Studio Code 1.74.1

## phina.js型定義ファイルのインストール

リポジトリ記載の方法に従い、インストールします。

```bash
npm install negiwine/phina.js.d.ts
```

## ソースファイルから型定義ファイルを読み込む

* 通常は、TypeScriptファイルから読み込むことになるかと思いますが、今回は後述するVisual Studio Codeの**ts-check**機能を使いたかったので、javascriptファイルのまま使用できるようにします。
* 今回は、jsconfig.jsonを作成し、phina.globalize()を使用する場合の設定を使用しました。

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

## 補完範囲について

* 型ファイルは、主に単一のクラスのプロパティやメソッドを補完することを目的としているため、継承クラスにおける継承元のメソッドなどの補完まではカバーしていないと思われます。
* JsDoc形式で独自クラスの型やプロパティを定義すれば、定義したものについては補完を行うことができます。

## ts-checkを使った型チェック

Visual Studio Codeでは、ソースコードの先頭に```//@ts-check```と記載することで**JavaScriptファイルのままTypeScript相当の型チェックをさせる**ことができます。

```js
//@ts-check

phina.globalize();
```

* **ts-check**の特徴は、TypeScriptの型エラーが出ていてもJavaScript的に正しければ実行できるという点です。
* TypeScriptでガッツリ組むまでもないプログラムで使うのが良いかもしれません。

## さいごに

* 今回試してみて、ts-checkについては、現在のphina.jsの仕様では、厳密に型チェックを行うことが逆効果となる可能性もあるので、使いどころが難しいと感じました。
* 型定義ファイルの方は、入力補完やメソッドが簡単に確認できるというメリットが大きいので、これからも積極的に使っていこうと思いました。
