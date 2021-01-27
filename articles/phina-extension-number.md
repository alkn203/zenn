---
title: "phina.jsの拡張クラスを使ってみよう　ーNumber編ー"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5"]
published: false
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

**for** を途中からカウントアップするバーションです。

### downto
自分自身の数から指定した数まで、カウンタをデクリメントしながら関数を繰り返し実行します。

```js
var arr = [];
(5).downto(1, function(i) {
  arr.push(i);
}); // => arr = [5,4,3,2,1]
```

  /**
   * @method round
   * 指定した小数の位を四捨五入した値を返します。
   *
   * 負の値を指定すると整数部の位を四捨五入できます。
   *
   * ### Example
   *     (13.87).round(); // => 14
   *     (-1.87).round(); // => -2
   *     (-1.27).round(); // => -1
   *     
   *     (2.345).round(); // => 2
   *     (2.345).round(1); // => 2.3
   *     (2.345).round(2); // => 2.35
   *
   *     (12345.67).round(-3); // => 12000
   *
   * @param {Number} [figure=0] 四捨五入する位
   * @return {Number} 小数第 figure 位で四捨五入した値
   */
  Number.prototype.$method("round", function(figure) {
    figure = figure || 0;
    var base = Math.pow(10, figure);
    var temp = this * base;
    temp = Math.round(temp);
    return temp/base;
  });

  /**
   * @method ceil
   * 指定した小数の位を切り上げた値を返します。
   *
   * 負の値を指定すると整数部の位を切り上げられます。
   *
   * ### Example
   *     (-1.27).ceil(); // => -1
   *     (-1.87).ceil(); // => -1
   *     
   *     (2.345).ceil(); // => 3
   *     (2.345).ceil(1); // => 2.4
   *     (2.345).ceil(2); // => 2.35
   *
   *     (12345.67).ceil(-3); // => 13000
   *
   * @param {Number} [figure=0] 切り上げる位
   * @return {Number} 小数第 figure 位で切り上げた値
   */
  Number.prototype.$method("ceil",  function(figure) {
    figure = figure || 0;
    var base = Math.pow(10, figure);
    var temp = this * base;
    temp = Math.ceil(temp);
    return temp/base;
  });

  /**
   * @method floor
   * 指定した小数の位を切り下げた値を返します。
   *
   * 負の値を指定すると整数部の位を切り下げられます。
   *
   * ### Example
   *     (-1.27).floor(); // => -2
   *     (-1.87).floor(); // => -2
   *     
   *     (2.345).floor(); // => 2
   *     (2.345).floor(1); // => 2.3
   *     (2.345).floor(2); // => 2.34
   *
   *     (12345.67).floor(-3); // => 12000
   *
   * @param {Number} [figure=0] 切り下げる位
   * @return {Number} 小数第 figure 位で切り下げた値
   */
  Number.prototype.$method("floor",  function(figure) {
    figure = figure || 0;
    var base = Math.pow(10, figure);
    var temp = this * base;
    temp = Math.floor(temp);

    // ~~this
    // this|0

    return temp/base;
  });

  /**
   * @method toInt
   * 数値を整数に変換します。
   *
   * ### Example
   *     (42.195).toInt(); // => 42
   *
   * @return {Number} 整数値
   */
  Number.prototype.$method("toInt",  function() {
    return (this | 0);
  });

  /**
   * @method toHex
   * 数値を16進数表記にした文字列を返します。
   *
   * ### Example
   *     (26).toHex(); // => "1a"
   *     (-26).toHex(); // => "-1a"
   *     (26.25).toHex(); // => "1a.4"
   *
   * @return {String} 16進数表記の文字列
   */
  Number.prototype.$method("toHex",  function() {
    return this.toString(16);
  });

  /**
   * @method toBin
   * 数値を2進数表記にした文字列を返します。
   *
   * ### Example
   *     (6).toBin(); // => "110"
   *     (-6).toBin(); // => "-110"
   *     (0xA3).toBin(); // => "10100011"
   *     (6.25).toHex(); // => "110.01"
   *
   * @return {String} 2進数表記の文字列
   */
  Number.prototype.$method("toBin",  function() {
    return this.toString(2);
  });


  /**
   * @method toUnsigned
   * 数値を unsigned int 型に変換します。
   *
   * 数値を符号無し整数として評価した値を返します。  
   * Javascriptのビット演算では数値を符号付きの32bit整数として扱うため、RGBA を
   * 整数値で表現して演算する場合、期待通りの結果が得られない場合があります。
   * そこで本関数で unsigned int 型に変換することで期待通りの値を得ることができます。
   *
   * ### Example
   *     rgba = 0xfeffffff & 0xff000000; // => -33554432
   *     rgba.toHex(); // => "-2000000"
   *     rgba.toUnsigned().toHex(); // => "fe000000"
   *
   * @return {Number} unsigned int 型に変換した値
   */
  Number.prototype.$method("toUnsigned",  function() {
    return this >>> 0;
  });


   * @method downto
   * 自分自身の数から指定した数まで、カウンタをデクリメントしながら関数を繰り返し実行します。
   *
   * 指定した数が自分自身の数より大きい場合は関数は実行されません。
   *
   * ### Example
   *     arr = [];
   *     (7).downto(4, function(i){
   *       arr.push(i);
   *     }); // => [7, 6, 5, 4]
   *
   * @param {Function} fn コールバック関数。引数にカウンタが渡される。
   * @param {Object} [self=this] 関数内で this として参照される値。デフォルトは自分自身。
   */
  Number.prototype.$method("downto",  function(t, fn, self) {
    self = self || this;
    for (var i=+this; i>=t; --i) {
      fn.call(self, i, this);
    }
    return this;
  });

  /**
   * @method step
   * 自分自身の値から指定した数まで、カウンタを増分させながら関数を繰り返し実行します。
   *
   * 上限値や増分値は float 型を指定することができます。
   *
   * ### Example
   *     var arr = [];
   *     (2.4).step(5.3, 0.8, function(n) {
   *       arr.push(n);
   *      }); // => [2.4, 3.2, 4.0, 4.8]
   *
   * @param {Number} limit カウンタの上限値
   * @param {Number} step カウンタを増分する量
   * @param {Function} fn コールバック関数。引数にカウンタが渡される。
   * @param {Object} [self=this] 関数内で this として参照される値。デフォルトは自分自身。
   */
  Number.prototype.$method("step",  function(limit, step, fn, self) {
    self = self || this;
    for (var i=+this; i<=limit; i+=step) {
      fn.call(self, i, this);
    }
    return this;
  });

  /**
   * @method map
   * 0から自分自身の値-1までカウンタをインクリメントさせながらコールバック関数を繰り返し実行し、
   * その返り値を要素とする配列を生成します。
   *
   * ### Example
   *     (5).map(function(i) {
   *       return i*i;
   *     }); // => [0, 1, 4, 9, 16]
   *
   * @param {Function} fn コールバック関数。引数にカウンタが渡される。
   * @param {Object} [self=this] 関数内で this として参照される値。デフォルトは自分自身。
   * @return {Array} 生成した配列
   */
  Number.prototype.$method("map",  function(fn, self) {
    self = self || this;

    var results = [];
    for (var i=0; i<this; ++i) {
      var r = fn.call(self, i);
      results.push(r);
    }
    return results;
  });

  /**
   * @method abs
   * 絶対値を返します。
   *
   * ### Example
   *     (-5).abs(); // => 5
   *     (+5).abs(); // => 5
   *
   * @return {Number} 絶対値
   */
  Number.prototype.$method("abs", function() { return Math.abs(this) });

  /**
   * @method acos
   * アークコサイン（ラジアン単位）を返します。
   *
   * ### Example
   *     (0).asin(); // => 0
   *     (1).asin(); // => 1.5707963267948966
   *
   * @return {Number} アークコサイン
   */
  Number.prototype.$method("acos", function() { return Math.acos(this) });

  /**
   * @method asin
   * アークサイン（ラジアン単位）を返します。
   *
   * ### Example
   *     (1).acos(); // => 0
   *     (-1).acos(); // => 3.141592653589793
   *
   * @return {Number} アークサイン
   */
  Number.prototype.$method("asin", function() { return Math.asin(this) });

  /**
   * @method atan
   * アークタンジェント（ラジアン単位）を返します。
   *
   * ### Example
   *     (0).atan(); // => 0
   *     (1).atan(); // => 0.7853981633974483
   *
   * @return {Number} アークタンジェント
   */
  Number.prototype.$method("atan", function() { return Math.atan(this) });

  /**
   * @method cos
   * コサイン（ラジアン単位）を返します。
   *
   * ### Example
   *     (Math.PI/3).cos(); // => 0.5
   *
   * @return {Number} コサイン
   */
  Number.prototype.$method("cos", function() { return Math.cos(this) });

  /**
   * @method exp
   * e<sup>this</sup> を返します。ここで e は自然対数の底であるネイピア数（オイラー数）です。
   *
   * ### Example
   *     (2).exp(); // => e<sup>2</sup>
   *     (0).exp(); // => 1
   *
   * @return {Number} e<sup>x</sup>
   */
  Number.prototype.$method("exp", function() { return Math.exp(this) });

  /**
   * @method log
   * 自然対数を返します。
   *
   * ### Example
   *     (Math.E * Math.E * Math.E).log(); // => 3
   *     (1).log(); // => 0
   *     (0).log(); // => -Infinity
   *
   * @return {Number} 自然対数
   */
  Number.prototype.$method("log", function() { return Math.log(this) });

  /**
   * @method max
   * 自分自身と引数の値を比べ、大きい方の値を返します。
   *
   * ### Example
   *     (15).max(10); // => 15
   *     (15).max(90); // => 90
   *
   * @param {Number} value 比較する値
   * @return {Number} 最大値
   */
  Number.prototype.$method("max", function(value) { return Math.max(this, value) });

  /**
   * @method min
   * 自分自身と引数の値を比べ、小さい方の値を返します。
   *
   * ### Example
   *     (15).min(10); // => 10
   *     (15).min(90); // => 15
   *
   * @param {Number} value 比較する値
   * @return {Number} 最小値
   */
  Number.prototype.$method("min", function(value) { return Math.min(this, value) });

  /**
   * @method clamp
   * 指定した範囲に収めた値を返します。
   *
   * ### Example
   *     (200).clamp(0, 640); // => 200
   *     (-15).clamp(0, 640); // => 0
   *     (999).clamp(0, 640); // => 640
   *
   * @param {Number} min 範囲の下限
   * @param {Number} max 範囲の上限
   * @return {Number} 範囲内に収めた値
   */
  Number.prototype.$method("clamp", function(min, max) { return Math.clamp(this, min, max) });

  /**
   * @method pow
   * 自分自身を exponent 乗した値、つまり this<sup>exponent</sup> の値を返します。
   *
   * ### Example
   *     (3).pow(2); // => 9
   *
   * @param {Number} exponent 累乗する指数
   * @return {Number} 累乗した結果の値
   */
  Number.prototype.$method("pow", function(exponent) { return Math.pow(this, exponent) });

  /**
   * @method sin
   * サイン（ラジアン単位）を返します。
   *
   * ### Example
   *     (Math.PI/4).sin(); // => 0.7071067811865476
   *
   * @return {Number} サイン
   */
  Number.prototype.$method("sin", function() { return Math.sin(this) });

  /**
   * @method sqrt
   * 平方根を返します。
   *
   * ### Example
   *     (49).sqrt(); // => 7
   *
   * @return {Number} 平方根
   */
  Number.prototype.$method("sqrt", function() { return Math.sqrt(this) });

  /**
   * @method tan
   * タンジェント（ラジアン単位）を返します。
   *
   * ### Example
   *     (Math.PI/4).tan(); // => 1.0
   *
   * @return {Number} タンジェント
   */
  Number.prototype.$method("tan", function() { return Math.tan(this) });

  /**
   * @method toDegree
   * ラジアンを度に変換します。
   *
   * ### Example
   *     Math.radToDeg(Math.PI/4); // => 45
   *
   * @return {Number} 度
   */
  Number.prototype.$method("toDegree", function() { return (this*Math.RAD_TO_DEG); });

  /**
   * @method toRadian
   * 度をラジアンに変換します。
   *
   * ### Example
   *     (180).toRadian(); // => 3.141592653589793
   *
   * @return {Number} ラジアン
   */
  Number.prototype.$method("toRadian", function() { return this*Math.DEG_TO_RAD; });

})();
