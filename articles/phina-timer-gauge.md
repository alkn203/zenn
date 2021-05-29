---
title: "【phina.js】タイマーゲージを作ってみた"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

**鋭意執筆中**
https://zenn.dev/alkn203/books/phina-tips-rewrite

![](https://storage.googleapis.com/zenn-user-upload/6908fcc52cd6764f0cb1eb2e.gif)

## はじめに
**phina.js**には、体力ゲージの実装に使える**Gauge**という便利なクラスが用意されています。今回、これをベースにゲームの制限時間表示によく見られるタイマーゲージを作ってみました。

## 実行サンプル
https://runstant.com/alkn203/projects/7eab89e2/

* runボタンを押すとゲージが経過時間で減っていきます。
* pauseボタンで一時停止。recoverボタンで全回復します。

## TimerGaugeクラスの仕様
以下のように、シーンに追加します。

```javascript
    // タイマーゲージ
    var gauge = phina.ui.TimerGauge({
      limitTime: 30,
    }).addChildTo(this);
    gauge.setPosition(this.gridX.center(), this.gridY.center());
```

## プロパティ・メソッド・イベント

| プロパティ | 説明 |
| ---  | --- |
| limitTime  | 制限時間(秒） デフォルト値60|

| メソッド | 説明 |
| ---  | --- |
| run  | タイマー作動 |
| pause | 一時停止 |
| recover | ゲージを満タンにする |

| イベント | 説明 |
| ---  | --- |
| onfull | ゲージが満タンになった時 |
| onempty | ゲージが空になった時 |

>その他のプロパティは、継承元の[**phina.ui.Gauge**](https://github.com/phinajs/phina.js/blob/develop/src/ui/gauge.js)クラスを参照して下さい。

## 使用方法
**html**ファイルで以下のように読み込みます。

```javascript
<script src="https://cdn.jsdelivr.net/gh/phi-jp/phina.js@v0.2.3/build/phina.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alkn203/phina-extensions@master/ui/timergauge.js"></script>
```

## ソースコード
https://github.com/alkn203/phina-extensions/blob/master/ui/timergauge.js
