---
title: "【phina.js】グループ管理の基本テクニック"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
通常ゲームを作る時には、配置物や敵キャラなどをグループとして管理する必要性がいずれ出てきます。この場合、一般的には**配列**を使用することになると思いますが、[phina.js](http://phinajs.com/)では、 **DisplayElement**クラスがオブジェクトのグループ管理用に良く使われます。

# DisplayElementクラスについて
**DisplayElement** クラスは、**アルファ値** などを持った基本クラスで、 **Shape**などの継承元です。更に基底の**Element**クラスなどを使うこともできますが、最低限の要素しか持っていませんので避けた方が良いかと思います。

# グループの作成
以下が**DisplayElement**を使ってグループを追加する例です。

```js
var group = DisplayElement().addChidTo(this)
```

# オブジェクトをグループに追加する
オブジェクトは直接**Scene**に追加するのではなく、 **group**に追加していきます。
以下は**CircleShape**を追加する例です。

```js
CircleShape().addChildTo(group);
```

# childrenを常に意識する
追加されたオブジェクトは、グループの**children**という**子要素配列**に格納されており、上記の場合だと、 **group.children**で参照することができます。
この**children**を常に意識しておくことが、オブジェクトのグループ管理では大切になってきます。

# グループからオブジェクトを削除する方法
**children**自体は配列です。通常の配列に行う操作は一通り可能ですが、折角なので、今回は**phina.js**で拡張されている**Array**クラスのメソッドを使ってみたいと思います。
[API Documentation：Array](http://phinajs.com/docs/#!/api/global.Array)

# 最初の要素を削除する
**first**プロパティで最初の要素を参照できますので、以下のようにします。

```js
group.children.first.remove();
```

# 最後の要素を削除する
**last**プロパティで最後の要素を参照できますので、以下のようにします。

```js
group.children.last.remove();
```

# 条件にマッチした要素を削除する（単発）
**eraseIf**メソッドを使います。引数には、削除される条件を書いた無名関数を与えます。 **true**を返した場合、その要素が削除されてループ処理から抜けます。

```js
group.eraseIf(function(elem) {
  if (elem.fill === 'red') {
    return true;
  }  
});
```

# 条件にマッチした要素を削除する（一括）
**eraseIfAll**メソッドを使います。使い方は**eraseIf**と同じですが、1回のループで条件にマッチした要素を一括で削除することができます。

```js
group.eraseIfAll(function(elem) {
  if (elem.fill === 'red') {
    return true;
  }  
});
```

# 要素を全て削除する
**clear**メソッドを使います。

```js
group.children.clear();
```

# 要素のプロパティの一括変更
グループに追加された要素に対して、拡大、回転、アルファ値などのプロパティを一括で変更することもできます。ただし、拡大や回転については、 **group**の原点を変更するなどの工夫が必要です。

```js
// 拡大
group.scaleX = 1.2;
// 回転
group.rotation = 45;
// 透明度
group.alpha = 0.5;
```

# サンプルプログラム
これまで紹介した内容は、以下のサンプルで実際に動作確認できます。

![group-management.gif](/images/group-management.gif)

* [要素の追加・削除(runstant)](https://runstant.com/alkn203/projects/3267e666)
* [プロパティの一括変更(runstant)](https://runstant.com/alkn203/projects/fac02dd8)

# まとめ
グループ管理は、ゲーム制作において不可欠な要素と言えるかと思います。
管理の方法は個々で異なると思うので、今回はその一例とお考え下さい。
**phina.js**の機能を色々と試して、自分に合った効率的な管理方法を是非見つけて下さい！