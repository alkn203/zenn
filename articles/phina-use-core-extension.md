---
title: "phina.jsのコアライブラリを使ってみよう　ーArray編ー"
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
}; // => arr = [1,3]
```

### eraseIfAll
引数で与えられたコールバック関数の条件に一致する要素を全て削除します。

```js
var arr = [1,2,3];
arr.eraseIf(function(elem) {
  if (elem > 1) {
    return true;
  }
}; // => arr = [1]
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

   * @method equals
   * 渡された配列と等しいかどうかをチェックします。
   * @method deepEquals
   * ネストされている配列を含め、渡された配列と等しいかどうかをチェックします。
   * @method at
   * 指定したインデックスの要素を返します（ループ・負数の指定可）。
   * @method find
   * 各要素を引数にして関数を実行し、その値が真となる（＝条件にマッチする）最初の要素を返します。
   * @method findIndex
   * 各要素を引数にして関数を実行し、その値が真となる（＝条件にマッチする）最初のインデックスを返します。
   * @method erase
   * 指定したオブジェクトと一致した最初の要素を削除します。
   * @method eraseAll
   * 指定したオブジェクトと一致したすべての要素を削除します。
   * @method clone
   * 自身のコピーを生成して返します。
  
  /**
   * @method fill
   * @chainable
   * 自身の一部の要素を特定の値で埋めます。
   *
   * ### Example
   *     arr = [1, 2, 3, 4, 5];
   *     arr.fill("x");       // => ["x", "x", "x", "x", "x"]
   *     arr.fill("x", 2, 4); // => [1, 2, "x", "x", 5]
   *
   * @param {Object} value 埋める値
   * @param {Number} [start=0] 値を埋める最初のインデックス
   * @param {Number} [end=自身の配列の長さ] 値を埋める最後のインデックス+1
   */
  Array.prototype.$method("fill", function(value, start, end) {
    start = start || 0;
    end   = end   || (this.length);
    
    for (var i=start; i<end; ++i) {
      this[i] = value;
    }
    
    return this;
  });
  

  /**
   * @method range
   * @chainable
   * 自身を等差数列（一定間隔の整数値の列）とします。
   *
   * - 引数が1つの場合、0～end（end含まず）の整数の配列です。  
   * - 引数が2つの場合、start～end（end含まず）の整数の配列です。  
   * - 引数が3つの場合、start～end（end含まず）かつ start + n * step (nは整数)を満たす整数の配列です。
   *
   * ### Example
   *     arr = [];
   *     arr.range(4);        // => [0, 1, 2, 3]
   *     arr.range(2, 5);     // => [2, 3, 4]
   *     arr.range(2, 14, 5); // => [2, 7, 12]
   *     arr.range(2, -3);    // => [2, 1, 0, -1, -2]
   *
   * @param {Number} start 最初の値（デフォルトは 0）
   * @param {Number} end 最後の値（省略不可）
   * @param {Number} [step=1または-1] 間隔
   */
  Array.prototype.$method("range", function(start, end, step) {
    this.clear();
    
    if (arguments.length == 1) {
      for (var i=0; i<start; ++i) this[i] = i;
    }
    else if (start < end) {
      step = step || 1;
      if (step > 0) {
        for (var i=start, index=0; i<end; i+=step, ++index) {
          this[index] = i;
        }
      }
    }
    else {
      step = step || -1;
      if (step < 0) {
        for (var i=start, index=0; i>end; i+=step, ++index) {
          this[index] = i;
        }
      }
    }
    
    return this;
  });
  
  /**
   * @method shuffle
   * @chainable
   * 自身の要素をランダムにシャッフルします。
   *
   * ### Example
   *     arr = [1, 2, 3, 4, 5];
   *     arr.shuffle(); // => [5, 1, 4, 2, 3] など
   */
  Array.prototype.$method("shuffle", function() {
    for (var i=0,len=this.length; i<len; ++i) {
      var j = Math.randint(0, len-1);
      
      if (i != j) {
        this.swap(i, j);
      }
    }
    
    return this;
  });

  /**
   * @method sum
   * 要素の合計値を返します。
   * @method average
   * 要素の平均値を返します。
   * @method each
   * 要素を順番に渡しながら関数を繰り返し実行します。
   * @method most
   * 指定した関数の返り値が最小となる要素と最大となる要素をまとめて返します。