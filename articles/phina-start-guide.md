---
title: "phina.js事始め"
emoji: "🐦"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

![](https://raw.githubusercontent.com/phinajs/phina.js/develop/logo.png)
*ロゴはphinajs.comから借用*

## はじめに
これまで[phina.js](https://phinajs.com)に関するいくつかの記事を投稿していますが、**phina.js** 自体について書いていなかったので、ここで簡単に紹介したいと思います。

## phina.jsについて
趣味でプログラミングをしている私が日々楽しんで使っている**国産**の**javascript**ゲームライブラリです。以前から**javascript**でゲーム開発をしている方は知っているかもしれませんが、**tmlib.js** の後継ライブラリになります。とりあえずどのようなものか知りたい方には、以下の紹介記事が参考になるかと思います。

* [本日 JavaScript ゲームライブラリ『phina.js』をリリースしました!](http://phiary.me/phinajs-release/) by [phi](https://twitter.com/phi_jp) さん
* [phina.js - JavaScriptで楽しく簡単にゲームが作れるライブラリ](http://qiita.com/simiraaaa/items/7431734994c9e94dacfd) by [simiraaaa](https://twitter.com/simiraaaa) さん
* [はじめてのphina.js – JavaScriptゲームライブラリを使ってみた！](https://liginc.co.jp/306739) by [株式会社LIG](https://liginc.co.jp/) さん

## phina.jsで作ることができるゲーム
端的に言うと、現時点では**2Dゲーム** ならジャンル問わず（やる気さえあれば）大抵のものを作ることが出来ると思います。実際に作られたゲームを見てみたいという方は、以下のサイトがおすすめです。

* [かちゃコム](https://cachacacha.com/) by [utyo](https://twitter.com/utyo) さん
**phina.js**で作られた様々なミニゲームがあります。どれも良く出来ていて熱中してしまいます。

* [Quest for the Tanelorn](https://minimo.github.io/QuestForTanelorn/) by [minimo](https://twitter.com/minimo) さん
スーパーファミコン時代を彷彿させる2Dアクションゲームです。作り込みのレベルが高いです。

## 気に入っているところ
以下は個人の主観ですが、他のユーザーも少なからず感じているのではないかと思います。

#### 同じ処理でも少ないコード量でコーディングできる
全てのゲームライブラリと比較したわけではありませんが、**phina.js** で書き慣れると、個人的には同様の処理を行う他のライブラリのコードが冗長に感じるようになりました。

#### ソースが読みやすい
**phina.js**のソースの書き方を真似ることで、結果的に自分のコードも読みやすくなりました。

#### 実行確認までのステップが短い
Web上でコーディングできる[runstant](https://runstant.com/)を使うことで、簡単に自分のプログラムの実行結果が確認できます。

#### 配列やベクトルなどのコーディングする上でベースとなるクラスの機能拡張が充実している
コアクラスの拡張など、コーディングする上で細かな配慮がされた仕様になっています。

## とにかく使ってみたい方へ
* [Githubのリポジトリ](https://github.com/phinajs/phina.js)に導入方法が記載されていますが、開発者向けの内容に近いので、その辺り不慣れな方には少し敷居が高く感じられるかもしれません。

* 一番簡単な方法は、**runstant** へのユーザー登録が必要ですが、[runstantに用意されたテンプレート](https://runstant.com/phi/projects/phinajs_template)をForkすることです。

* **javascript**で作られていますので、Webブラウザと自分の好きなエディタさえあれば、複雑な環境構築の必要もなくゲーム開発を始めることができます。

## オープンソースという魅力
* **phina.js**はオープンソースプロジェクトであり、これまで有志により改良が重ねられてきています。個人的には、ゲーム開発に必要な基本機能は現バージョンでもひと通り揃っていると思っています。
* その開発については広く門戸が開かれており、誰でもオープンソースプロジェクトに貢献できる可能性があるというのが大きな魅力の１つと言えます。

## 質問など
* 個人的に**phina.js**がオススメな理由の一つに、親切なサポート体制があります。
* **Twitter**で **#phina_js** タグをつけて質問すれば、ユーザーが優しく真剣に答えてくれます。

## Tipsなど
まずどんなものか少しでも知ってもらうために、手前味噌ながら[phina.js Tips集](https://zenn.dev/alkn203/books/phina-js-tips?utm_source=pocket_mylist)というものを書いていますので、こちらもよろしければご覧ください。

## おわりに
* ゲームライブラリは実際に使ってみないとその良し悪しはわかりませんが、その中でも**phina.js**は、比較的簡単にトライすることができるライブラリです。
* **javascript**でのゲーム開発に興味がある方は、是非一度触ってみて色々と試して頂ければと思います。
