---
title: "Manjaro Linux 設定備忘録"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Linux"]
published: true
---

# Manjaro Linuxの自分用の設定備忘録

## キー入れ替えCaps　→　Ctrl
.bashrcに以下を追記
`/usr/bin/setxkbmap -option "ctrl:nocaps"`

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
https://qiita.com/varmil/items/9b0aeafa85975474e9b6

### keyring
https://wiki.archlinux.jp/index.php/GNOME/Keyring#GNOME_Keyring_.E3.81.A8_Git


## runstant カーソルずれ問題対応
AURリポジトリから**monaco**フォントをインストール
パッケージ名：**ttf-monaco**

https://qiita.com/kawadumax/items/8bbbc042c6f17407847e