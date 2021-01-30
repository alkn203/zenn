---
title: "phina.jsの拡張クラスを使ってみよう　ーNumber編ー"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5"]
published: true
---

## はじめに
[phina.jsの拡張クラスを使ってみよう　ーArray編ー](https://zenn.dev/alkn203/articles/phina-use-core-extension)では、[phina.js](https://phinajs.com/)の配列の拡張クラスを紹介しました。今回は、**Number** の拡張クラスを紹介したいと思います。

## 拡張Numberクラス
**phina.js** では、元々の **javascript** の**Number**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いと思われるメソッドを紹介します。

### padding
指定した桁になるように左側に文字を埋めます。

```js
(999).padding(5); // => "00999"
```

ゲームのスコア表示などに使えます。返り値は文字列です。

### times
カウンタをインクリメントしながら関数を繰り返し実行します。

```js
var arr = [];
(5).times(function(i) {
  arr.push(i);
}); // => arr = [0,1,2,3,4]
```

通常の **for** の代わりに使用できます。添字を意識しなくて良いのがポイントです。

### upto
自分自身の数から指定した数まで、カウンタをインクリメントしながら関数を繰り返し実行します。

```js
var arr = [];
(5).upto(10, function(i) {
  arr.push(i);
}); // => arr = [5,6,7,8,9,10]
```

**for** を途中からカウントアップするイメージです。

### downto
自分自身の数から指定した数まで、カウンタをデクリメントしながら関数を繰り返し実行します。

```js
var arr = [];
(5).downto(1, function(i) {
  arr.push(i);
}); // => arr = [5,4,3,2,1]
```

**for** を途中からカウントダウンするイメージです。

### step
自分自身の値から指定した数まで、指定された差分でカウントアップさせながら関数を繰り返し実行します。

```js
var arr = [];
(2).step(10, 2, function(n) {
  arr.push(n);
}); // => [2,4,6,8]
```

オブジェクトを並べる際に、一定間隔で間を空けたいときなどに使えます。

### map
カウンタをインクリメントさせながらコールバック関数を繰り返し実行し、その返り値を要素とする配列を生成します。

```js
(5).map(function (i) {
  return i * 10;  
}); // => [0,10,20,30,40]
```

**step** にも似ていますが、配列が返ってくるところが違いです。

### max
自分自身と引数の値を比べ、大きい方の値を返します。

```js
(10).max(5); // => 10
```

実際には、変数と比べることになると思います。

### clamp
指定した範囲に収めた値を返します。

```js
(-10).clamp(0, 640); // => 0
(320).clamp(0, 640); // => 320
(780).clamp(0, 640); // => 640
```

キャラクターの移動を画面の範囲内に制限したいときなどに使えます。

他にも、例えば以下のようなメソッドが用意されています。
詳しく見たいという方は、以下をご確認下さい。
> Numberクラスのソース
> https://github.com/phinajs/phina.js/blob/develop/src/core/number.js

### round
指定した小数の位を四捨五入した値を返します。

### toHex
数値を16進数表記にした文字列を返します。

### abs
絶対値を返します。

### cos
コサイン（ラジアン単位）を返します。

### sqrt
平方根を返します。

### toDegree
ラジアンを度に変換します。

### toRadian
度をラジアンに変換します。

## 使い方
htmlファイルで **phina.js** を以下のように読み込みます。

```html
<script src="https://cdn.jsdelivr.net/gh/phinajs/phina.js@v0.2.3/build/phina.js"></script>
```

## 実行サンプル
https://runstant.com/alkn203/projects/52837800

## さいごに
* 数字もオブジェクトとして扱えるという点で、直感的な操作ができるので便利です。
* 次回は、**String** クラスを紹介したいと思います。
