---
title: "[phina.js]コード内にフレームアニメーション定義を記載する"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# 今回のTips
今回は、画像を使ったゲーム作りには欠かせないフレームアニメーションの小ネタを紹介します。phina.jsのフレームアニメーションについては、以下のエントリーがとても参考になります。

[[phina.js]SpriteSheetを使ってみよう！](http://qiita.com/minimo/items/0aad94d94478845d3ce2) by @minimo 氏

上の記事に書いてあるように、フレームアニメーションで使用するスプライトシートは、基本的には外部のファイルに**JSON**形式で定義して、**phina.js**から**ASSET**として読み込みます。

一方、**phina.js**のソースを読むと仕組みが分かりますが、スプライトシートの定義は**js**のコード内に直接書くこともできます。
方法は、以下のように**ASSET**のスプライトシートの該当箇所に**JSON**ファイルの中身を書くだけです。
この手法は、手っ取り早くフレームアニメーションの確認や調整をしたい場合に便利かと思います。また、**js**のコード内なので、コメントを付すこともできます。

```js
var ASSETS = {
  image: {
    'explosion': 'https://rawgit.com/alkn203/radar_touch/gh-pages/assets/explosion.png',
  },
  spritesheet: {
    'explosion_ss':
    {
      "frame": {
        "width": 80,
        "height": 80,
        "cols": 8,
        "rows": 1,
      },
      // アニメーション
      "animations" : {
        "start": {
          "frames": [0,1,2,3,4,5,6,7],
          "next": "end",
          "frequency": 2,
        },
        "end": {
          "frames": [5,6,7],
          "next": "end",
          "frequency": 5,
        }
      }
    },
  }
};
```
# サンプル
![frameanimation.png](https://qiita-image-store.s3.amazonaws.com/0/67114/0bee79cb-f320-7682-f20a-7f7a207d8515.png)

[[runstantで確認]](https://runstant.com/alkn203/projects/3a8e13fb)

# まとめ
今回は、フレームアニメーションの小ネタについて書かせて頂きました。使い方としては、まずはコード内で書いて動作を確認しつつ、固まったらソースコード肥大化防止のために外部JSONファイルに移すという流れが良いでしょう。
