---
title: "【phina.js】Gridクラスを使いこなそう"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
[phina.js](http://phinajs.com/)には**Grid**というクラスが用意されています。この**Grid**を活用することで、直接的な座標値をあまり意識せずにオブジェクトをシーンに配置することができます。このエントリーでは、 **Grid**の仕組みや機能について、使用例を織り交ぜながら説明したいと思います。

# SceneにはデフォルトでGridが設定されている
**Grid**は文字通り**格子**を意味しており、 **phina.js**では、 **Scene**に対してデフォルトで**Grid**が設定されています。可視化すると下図のようになります。

![grid.gif](/images/grid.gif)

[[runstantで開く]](http://runstant.com/alkn203/projects/ffdf49bd)

上の図のように、デフォルトでは**縦・横がそれぞれ16のグリッド**に分けられていることが分かります。

# Gridの使用方法
**Scene**の**Grid**は初期設定されているので、すぐに使うことができます。
例として**Scene**の**横4グリッド・縦6グリッドの位置**に **Rectangle**を配置してみます。以下が該当コード部分です。

```js
    var rect = RectangleShape().addChildTo(this);
    rect.setPosition(this.gridX.span(4), this.gridY.span(6));
```

* **this**は現在の**Scene**を指しています。
* **gridX**は横の**Grid**、**gridY**は縦の **Grid**です。
* **span**で位置を指定します。（0から16まで指定可能）

実際に配置した画像が以下のとおりです。
**Grid**との関係がわかり易いように、半透明にしています。

![grid-span.gif](/images/grid-span.gif)

[[runstantで開く]](http://runstant.com/alkn203/projects/5f2e7803)

# centerメソッドが便利
**Grid**を使った配置では、上述したように個別に**span**を使ってもいいのですが、 **center**を使うと画面を中心に対照的に上手く配置することができます。

![grid-center.gif](/images/grid-center.gif)

[[runstantで開く]](http://runstant.com/alkn203/projects/73380d8f)

以下が該当コード部分です。

```js
    var rect = RectangleShape().addChildTo(this);
    rect.setPosition(this.gridX.center(), this.gridY.center());
    rect.alpha = 0.5;

    var rect2 = RectangleShape().addChildTo(this);
    rect2.setPosition(this.gridX.center(-3), this.gridY.center(2));
    rect2.alpha = 0.5;

    var rect3 = RectangleShape().addChildTo(this);
    rect3.setPosition(this.gridX.center(3), this.gridY.center(2));
    rect3.alpha = 0.5;
```

* **gridX.center**と **gridY.center**で画面の中央に配置することが出来ます。
* **center**の引数にオフセット値を与えることで、中心を基準として配置を行うことが出来ます。対称にしたければ、3と-3のように正負対の数値を指定します。
* **span**や**center**の引数には小数値も指定できますので、位置の微調整が可能です。

# Gridを自作する
**Scene**の**Grid**で事足りない場合は、任意の幅とグリッド数で独自に**Grid**を作成することができます。 **Grid**クラスのコンストラクタは以下のとおりです。

```js
Grid({
  width: グリッド全体の幅,
  columns: グリッドの数,
  loop: spanにマイナス値を指定できるかどうか（任意）,
  offset: オフセット値（任意）
});
```

**Grid**を自作すれば、以下のように異なる幅やグリッド数の**Grid**を混在させることもでき、レイアウトの幅が広がります。

![various-grid.gif](/images/various-grid.gif)

[[runstantで開く]](http://runstant.com/alkn203/projects/386838f8)

# まとめ
**Grid**は一見地味に見えますが、ゲーム作りの土台となる重要な役割を担っているとも言えるかと思います。 **Grid**を使いこなして、**phina.js**でのゲーム作りに役立てましょう！