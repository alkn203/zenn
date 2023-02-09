---
title: "[phina.js]型定義ファイルで補完して緩く型チェックする"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","typescript","vscode"]
published: false
---

# はじめに
これまで、**phina.js**のコーディングは、主に[Runstant](https://runstant.com/)を使ってきました。
コード補完はできるのですが、基本ソース上にあるキーワードのみです。
**phina.js**用の非公式の型定義ファイルがあることを知っていましたが、なかなか試す機会がなかったため、今回トライしてみることにしました。

https://github.com/negiwine/phina.js.d.ts

# 検証環境
* Manjaro Linux
* Nodejs v19.5.0
* npm 8.19.2
* Visual Studio Code 1.74.1

# phina.js型定義ファイルのインストール
リポジトリ記載の方法に従い、インストールします。

```bash
npm install negiwine/phina.js.d.ts
```

# ソースファイルから型定義ファイルを読み込む
通常は、TypeScriptファイルから読み込むことになるかと思いますが、今回は後述するVisual Studio Codeの**ts-check**機能を使いたかったので、javascriptファイルから読み込みます。
ソースの先頭に以下のように記載します。

```js
/// <reference path="./node_modules/phina.js.d.ts/globalized/index.d.ts" />
```

# 型定義ファイルの種類
**phina.globalize**宣言をしている場合と、そうでない場合で読み込むファイルが異なります。

| 種類 | 読み込み先 |
|:-----------------|:------------------|
| phina.globalize() | ./node_modules/phina.js.d.ts/globalized/index.d.ts |
| 通常 | ./node_modules/phina.js.d.ts/index.d.ts |

# コード補完の確認
ソースファイルでコード補完がされることを確認します。

![code_hint.gif](/images/code_hint.gif)

