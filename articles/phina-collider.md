---
title: "[phina.js]Colliderクラスを作ってみた"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# きっかけ
**phina.js**では、当たり判定メソッドがあらかじめ用意されていて便利ですが、少し凝った当たり判定をしたい時は当たり判定用のダミーオブジェクトを使うなど、自分で工夫する必要があります。
例えば**Unity**では、オブジェクトに**Collider**というコンポーネントを追加すると、視覚的に衝突の判定が確認できます。この**Collider**のようなものが**phina.js**で実装できないかと思ったのがきっかけです。

# ColliderをAccessoryとして設計
**phina.js**には**Unity**のコンポーネント方式のような**Accessory**というクラスがあります。その名前のとおり、アクセサリーのように様々な機能などの着脱ができるようにしたものです。**phina.js**のサンプルに良く見られる**Tweener**や**Physical**も**Accessory**を継承しています。
作り方などは、[phina.jsでアクセサリを使ってみよう＆作ってみよう！](https://qiita.com/minimo/items/25304f0834ecff853077)が参考になると思います。

# サンプル
![collider-class.gif](/images/collider-class.gif)


[[runstantで確認]](https://runstant.com/alkn203/projects/0611497c)

* サンプルでは、2つのキャラクターが壁に向かって移動し、壁にぶつかると反転移動します。
* キャラの周りに表示されているのが今回作った**Collider**です。
* **Collider**を単純にアタッチすると、下のキャラのようにスプライトのサイズに合わせた大きさになります。
* 上のキャラは、**Collider**を実際の画像ギリギリの幅にサイズ調整しているので、衝突の仕方に違いが出ているのが分かるかと思います。

# 使い方
自作の拡張クラスの一部として個人のリポジトリにアップしていますので、htmlで以下のように読み込めば使用できます。

```javascript
<script src="https://cdn.jsdelivr.net/gh/phi-jp/phina.js@v0.2.3/build/phina.js"></script>
<script src="https://cdn.jsdelivr.net/gh/alkn203/phina-extensions@master/build/phina-extensions.min.js"></script>
```

# 対象オブジェクトにアタッチする
```javascript
obj.collider.show();
```
* 一番簡単な方法は、上のようにオブジェクトのプロパティとしてアクセスしながらアタッチする方法です。
* デフォルトでは非表示になっているので、**show**で表示させます。

# サイズを変える
```javascript
obj.collider.setSize(width, height);
```
* デフォルトでは対象オブジェクトのサイズになりますが、サイズを指定して変更することができます。
* 意図的に当たり判定を小さくしたい時などに使えます。

# 位置を変える
```javascript
obj.collider.offset(x, y);
```
* デフォルトでは対象オブジェクトの中心に表示されますが、**オフセット値**（相対座標）を指定して位置を変更することができます。
* 対象オブジェクトより少し前にずらして、先読み判定でめり込みを防止する時などに使えます。

# 当たり判定を行う
```javascript
obj.collider.hitTest(target.collider);
```
* このメソッドの引数は、同じ**Collider**です。つまり、対象オブジェクトにも**Collider**がアタッチされている必要があります。
* ヒットすれば**true**、そうでない場合は**false**を返します。

# 課題
* パフォーマンスは二の次に考えて設計しましたので、大量のオブジェクトの場合どうなるかは未検証です。
* 今の仕様では1つの**Collider**で矩形のみですので、**Unity**のように複数の**Collider**かつ、矩形だけでなく円もということになると、**type**や**id**で区別するなど抜本的な仕様変更が必要かと思います。

# まとめ
今回は**Collider**をサンプルにしましたが、**phina.js**の**Accessory**の拡張性を少しは感じて頂けたかと思います。