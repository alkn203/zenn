---
title: "phina.jsの拡張クラスを使ってみよう　ーArray編ー"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5"]
published: false
---

## はじめに
[phina.js](https://phinajs.com/)は、国産の **javascript** ゲームライブラリです。
ゲーム製作向けのライブラリですので、ゲーム製作を前提とした機能が充実していますが、加えて**javascript** でのコーディングをより快適にするための細かな配慮がされているライブラリでもあります。その例として、今回は、**javascript** の基本クラスである **Array** の拡張メソッド群を紹介したいと思います。

## 拡張Arrayクラス
**phina.js** では、元々の **javascript** の**Array**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いだろうと思われるメソッドを紹介します。

### first
配列の最初の要素を返します。

### last
配列の最後の要素を返します。

```js
var arr = [1,2,3];
arr.first; // => 1
arr.last; // => 3
```
これらは、メソッドというよりもプロパティです。

### contains
指定された要素が配列に含まれているかどうかを調べます。

```js
var arr = [1,2,3];
arr.contains(1); // => true
```

### swap
指定されたインデックスの要素を入れ替えます。

```js
var arr = ['a','b','c'];
arr.swap(0,1); // => arr = ['b','a','c']
```

### eraseIf
引数で与えられたコールバック関数の条件に一致する最初の要素を削除します。

```js
var arr = [1,2,3];
arr.eraseIf(function(elem) {
  if (elem > 1) {
    return true;
  } 
}); //=> arr = [1,3]
aaa
```

### eraseIfAll
引数で与えられたコールバック関数の条件に一致する要素を全て削除します。

```js
var arr = [1,2,3];
arr.eraseIf(function(elem) {
  if (elem > 1) {
    return true;
  }
}); // => arr = [1]
```

### random
配列からランダムな要素を１つ返します。

```js
var arr [1,2,3];
arr.random(); // => random value in 1,2,3
```

### uniq
配列から重複した値を除いた新しい配列を返します。

```js
var arr = [1,2,2,3,4,4,5];
arr.uniq() => [1,2,3,4,5]
```

### clear
配列の要素を全て削除します。

```js
var arr = [1,2,3];
arr.clear(); //=> []
```

### fill
要素を指定した値で埋めます。

```js
var arr = [1,2,3];
arr.fill(0); // => arr = [0,0,0,]
```

### range
一定間隔の整数の列としての配列を返します。

```js
var arr = [];
arr.range(4); // => arr = [0,1,2,3]
arr.range(2,5) // => arr = [2,3,4]
arr.range(2,10,2) // => arr = [2,4,6,8]
```

### shuffle
配列の要素をシャッフルします。

```js
var arr = [1,2,3,4,5];
arr.shuffle(); // => [2,3,5,4,1] など
```

### その他
他にも以下のようなメソッドが用意されています。

#### equals
渡された配列と等しいかどうかをチェックします。

#### at
指定したインデックスの要素を返します。（ループ・負数の指定可）

#### erase
指定したオブジェクトと一致した最初の要素を削除します。

#### clone
自身のコピーを生成して返します。

#### sum
要素の合計値を返します。

#### average
要素の平均値を返します。