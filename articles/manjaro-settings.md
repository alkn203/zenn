---
title: "Manjaro Linux 設定備忘録"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Linux"]
published: true
---

# パッケージ更新
* 管理ソフト**pamac**の設定から、ダウンロードミラー先を**日本**に変更
* パッケージ更新コマンドを実行

```sh
sudo pacman -Syu
```

# AURヘルパーインストール

```sh
sudo pacman -S yay
```

# 開発環境インストール

## Google Chrome

```sh
yay -S google-chrome
```

### Visual Studio Code

```sh
yay -S visual-studio-code-bin
```

# CapsLockキーと左Ctrlキー入れ替え

GUIで設定可能な**Input Remapper**を導入

```sh
yay -S input-remapper-git
```

設定方法

https://arimasou16.com/blog/2022/09/11/00477/

:::details その他の方法

* Caps→Ctrl
* 左Ctrl←→Alt
* 「セッションと起動」→「自動開始アプリケーション」→「追加」→「コマンド」に以下を追加
```
/usr/bin/setxkbmap -option "ctrl:nocaps" -option "ctrl:swap_lalt_lctl"
```

:::

# 日本語入力
## パッケージインストール(fcitx-mozc)

```sh
sudo pacman -S fcitx-mozc fcitx-gtk2 fcitx-gtk3 fcitx-qt5
```

## .xprofileを作成して以下を入力

```sh
export LANG="ja_JP.UTF-8"
export XMODIFIERS="@im=fcitx"
export XMODIFIER="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export DefaultIMModule=fcitx
```

## .bashrcに以下を追記

```sh
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
```

# ターミナル設定

## gitコマンド補完


ブランチ名やコミット差分が表示されるようになる。
https://qiita.com/varmil/items/9b0aeafa85975474e9b6

#### 現在のプロンプト表示設定
```sh
export PS1='\[\033[1;32m\]\u\[\033[00m\]:[\[\033[1;34m\]\w\[\033[1;31m\]$(__git_ps1)\[\033[00m\] ]\$ '
```

### Github ログイン関係
* 2021年8月13日からパスワードでの認証が不可になった。
* gitコマンドラインでpushでエラーが出る。
* pushがhttp認証の場合、Personal Access Tokenを取得してパスワードの代わりに入力する必要がある。
* パスワード記録用ツールは、gnome-keyringが廃止予定のため、**libsecret**を使用する。
* パスワード管理は、パッケージから**seahorse**をインストールして行う。

```bash
git config --global credential.helper /usr/lib/git-core/git-credential-libsecret
```

https://zenn.dev/oratake/articles/linux-git-https-token

## runstant カーソルずれ問題対応
デフォルトのままだと入力とカーソルがずれる。
### 対処方法
* AURリポジトリから**monaco**フォントをインストール
* パッケージ名：**ttf-monaco**

https://qiita.com/kawadumax/items/8bbbc042c6f17407847e

## Visual Studio Code設定
### emacsキーバインド
https://qiita.com/bbapexx/items/e1812407ed0ee948dd9c
https://developers.gmo.jp/11041/

### fcitxのCTRL + SPACEの日本語変換を無効にする
* メニュー　→　設定　でコンフィグファイルがエディタで開かれる
* 以下のように変更

```
# 入力メソッドのオンオフ
#TriggerKey=CTRL_SPACE ZENKAKUHANKAKU
TriggerKey=ZENKAKUHANKAKU
```

## homeにある日本語ディレクトリを英語にする
https://qiita.com/apu4se/items/7e36586e0ba1bfe5dd48
