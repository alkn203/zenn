---
title: "Manjaro Linux 設定備忘録"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Linux"]
published: true
---

# Manjaro Linuxの自分用の設定備忘録

# パッケージ更新
* pamacの設定から、ダウンロードミラー先を日本に変更

## キー入れ替え
* Caps→Ctrl
* 左Ctrl←→Alt
* 「セッションと起動」→「自動開始アプリケーション」→「追加」→「コマンド」に以下を追加
```
/usr/bin/setxkbmap -option "ctrl:nocaps" -option "ctrl:swap_lalt_lctl"
```

## 日本語入力
### パッケージインストール(fcitx-mozc)
```
$ sudo pacman -S fcitx-mozc fcitx-gtk2 fcitx-gtk3 fcitx-qt5
```

### .xprofileを作成して以下を入力
```
export LANG="ja_JP.UTF-8"
export XMODIFIERS="@im=fcitx"
export XMODIFIER="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export DefaultIMModule=fcitx
```

### .bashrcに以下を追記
```
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
```

## git設定
### プロンプト表示変更
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

## Atom設定
### emacsキーバインド
* atomic-emacsパッケージをインストール
* キーマップを設定（現在は、edit → keymap）
https://qiita.com/jirourashima/items/153aefdb471b561df4f6

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
