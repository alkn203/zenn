---
title: "Webブラウザだけでhtml5ゲームを作成して公開する"
emoji: "🎮"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","runstant","github"]
published: true
---

## はじめに
* ゲームを作っている人なら、自分の作ったゲームを誰かに遊んでもらいたい気持ちは皆持っていると思います。
* 今では、スマホアプリを作ってプラットフォームで公開する方法がメジャーですが、初心者にとってはそう簡単にはいかないものです。    
* そういう中でも、**アプリ化まではいかないけど何かゲームを作って公開してみたい** と思っている人もいるでしょう。その場合は、**javascriptでhtml5ゲーム** を作って、Webで公開することをおすすめします。（もちろんhtml5ゲームをアプリ化する方法もあります）

## Webブラウザだけでゲームを作成して公開する
今回は、特別な環境構築をせずに、Webブラウザだけを使って**html5ゲーム** を公開する方法を紹介します。大まかな流れは以下のとおりです。

1. **runstant**でコーティングする
2. **runstant**プロジェクトを**html**ファイルとしてダウンロードする
3. **GitHub**にゲーム公開用のリポジトリを作る
4. **html**ファイルを**GitHub**のリポジトリにアップロードする
5. **GitHub**のリポジトリの設定で**GitHub Pages**を有効にする

それでは、順番に説明していきます。

## runstantでコーティングする
* [runstant](https://runstant.com/)は、**phina.js**の生みの親である[phi](https://twitter.com/phi_jp)さんが開発したWebブラウザ上で動くオンラインエディタで、ユーザー登録すれば誰でも利用することができます。
* **phina.js**の公式エディタでもあり、実際に様々なプログラムが作られて公開されています。

## phina.jsのひな形からゲームを作成
普段私が使っている[phina.jsのひな形](https://runstant.com/alkn203/projects/8f0388a4)から作ることで、簡単に始められます。

## 今回作ったゲーム
サンプルとして、制限時間内にどれだけ円をタッチできるかを競う単純なゲームを作りました。
![touchthecircle](https://storage.googleapis.com/zenn-user-upload/210ynv5dsh82z023ccx1j7ejus3h)

[Touch The Circleをプレイする](https://alkn203.github.io/touchthecircle/)

## runstantプロジェクトをhtmlファイルとしてダウンロードする
* ゲームを作ったら、Webに公開するファイルの準備をする必要がありますが、**runstant** の便利な機能として、プロジェクトのダウンロードがあります。
* ダウンロードされたファイルは**html**形式で**javascript**のコード部分もパッケージ化されているので、このファイル１つあればゲームとして動作します。

## プロジェクトのダウンロード方法
![runstantdownload.gif](https://storage.googleapis.com/zenn-user-upload/j4vv2kwh35cn5frxob4n465orq33)

* **runstant**の画面下部にあるボタンをクリックすると、プロジェクトのダウンロードができます。
* ダウンロード先は任意の場所にして、ファイル名を**index.html**に変更して下さい。

## GitHubにゲーム公開用のリポジトリを作る
* [GitHub](https://github.co.jp/)は、今や有名になった、主にソースコードのバージョン管理を目的としたサービスです。
* 今回は、この**GitHub**をゲームの公開用サーバとして利用します。
* ユーザー登録を行ったら、ゲーム公開用のリポジトリを作成します。
* ユーザーホーム画面で「New」ボタンをクリックします。

![newrepository.gif](https://storage.googleapis.com/zenn-user-upload/q1gkshcr8rhs5yaqq40e4a8vcc83)

1. 作成画面でリポジトリ名を入力します。
1.  公開範囲が「Public」になっているのを確認します。
1. 「リポジトリをREAD ME で初期化」にチェックを入れます。
1. 「Create repository」ボタンをクリックします。

## htmlファイルをGitHubのリポジトリにアップロードする
リポジトリの用意が出来たら、**runstant** からダウンロードした**html**ファイルをリポジトリにアップロードします。**GitHub** は、ローカル環境から**git**のコマンドを駆使して使うイメージがありますが、**GitHub** 上のGUI操作でもファイルのアップロード程度はできます。

![uploadfile.gif](https://storage.googleapis.com/zenn-user-upload/xi5kve8jup0jas2q6n2bw5q9iw2j)

* 「Upload files」ボタンをクリックします。

![uploadfile2.gif](https://storage.googleapis.com/zenn-user-upload/tcgdtlgrhgone6x79kzofcqm0ihq)

1. 上の領域にダウンロードした**html**ファイルをドラッグするか、ファイル選択ダイアログでファイルを選択します。
2.  「Commit Changes」ボタンをクリックします。

![uploaded.gif](https://storage.googleapis.com/zenn-user-upload/t90xfu8jjjo3vf91656x8ixob0b1)

* ファイルがアップロードされたのを確認します。

## GitHubのリポジトリの設定でGitHub Pagesを有効にする

![setting.gif](https://storage.googleapis.com/zenn-user-upload/xy3m2megbtujv4ox7nqho1k6rhca)

*  上部メニューから「Setting」をクリックします。

![ghpages.gif](https://storage.googleapis.com/zenn-user-upload/jqn8gby5xk08iikeqhgavlx8ya51)

* 「github pages」を有効にします。以前は、**git** で**gh-pages**名でブランチを切って、**push**する必要がありましたが、現在は不要になっています。

![ghpagescheck.gif](https://storage.googleapis.com/zenn-user-upload/zr98mbgoa53sgstzpls4k1slaf7x)

* 上部に表示される アドレスが公開先のアドレスになります。
* 反映されるまでには、少し時間がかかる場合もあるようです。

## runstantプロジェクト
[Touch The Circle(runstant)](https://runstant.com/alkn203/projects/21370e4b)
