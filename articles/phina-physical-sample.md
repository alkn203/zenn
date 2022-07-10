---
title: "[phina.js]Physicalクラスでオブジェクトを動かす"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
[phina.js](http://phinajs.com/)には**Physical**という物理運動に似せた動きをオブジェクトに簡単に適用できるクラスが用意されています。今回は、この**Physical**について簡単に説明したいと思います。

# Physicalクラスの使い方
**Physical**クラスは、**Accessory**というクラスを継承しています。
使用する際には、**Shape**などのオブジェクトに**アタッチ**（取り付け）する必要があります。

以下は、**CircleShape**にアタッチする例です。

```js
var circle = CircleShape().addChildTo(this);
var physical = Physical().attachTo(circle);
```

もしくは、以下のようにオブジェクトのプロパティとして明示的に**physical**を設定すると、バックグラウンドでアタッチされます。

```js
var circle = CircleShape().addChildTo(this);
circle.physical.gravity.set(0, 0.5);
```

上記の場合、**オブジェクト変数名.physical**でプロパティにアクセスできるようになります。特段の理由がない限りは、手軽なこちらのアタッチ方法をお薦めします。

# Physicalクラスのメンバ
**Physical**クラスのメンバは、以下のとおりとなっています。

# velocity
* velocity.x ・・・ x方向の速さ
* velocity.y ・・・ y方向の速さ

xとyそれぞれ個別、velocity.set(x, y)による一括指定、force(x, y)メソッドによる指定ができます。addForce(x, y)で加速させることもできます。

```js
physical.velocity.x = 4;
physical.velocity.set(4, 8);
physical.force(0, 4);
physical.addForce(0.5, 0);
```

# gravity
* gravity.x ・・・ x方向の重力
* gravity.y ・・・ y方向の重力

xとyそれぞれ個別、gravity.set(x, y)による一括指定ができます。

```js
physical.gravity.y = 4;
physical.gravity.set(4, 8);
```

# friction
* friction ・・・ 摩擦度

デフォルト値は**1.0**です。**0に近づくほど摩擦が強く**なりますので、設定の際は注意してください。xとy方向、双方に適用されます。

```js
physical.friction = 0.98;
```

# まとめ
最後に**Physial**クラスを使ったサンプルを紹介します。
本来の正確な物理運動ではありませんが、それらしい動きになっているのがお分かりになるかと思います。

![physical-sample.gif](/images/physical-sample.gif)

[サンプルコード(runstant)](https://runstant.com/alkn203/projects/f256ad63)