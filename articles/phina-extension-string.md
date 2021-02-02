---
title: "phina.jsの拡張クラスを使ってみよう　ーString編ー"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5"]
published: false
---

## はじめに
[phina.jsの拡張クラスを使ってみよう　ーNumber編ー](https://zenn.dev/alkn203/articles/phina-extension-number)では、[phina.js](https://phinajs.com/)の数字の拡張クラスを紹介しました。今回は、**String** の拡張クラスを紹介したいと思います。

## 拡張Stringクラス
**phina.js** では、元々の **javascript** の**String**クラスに様々な拡張メソッドが追加されています。
以下に、使う機会が多いと思われるメソッドを紹介します。

### format
定義したフォーマットに引数を適用した文字列を返します。

```js
// 引数がオブジェクトの場合
var obj = {r: 128, g: 0, b: 255};
"color: rgb({r}, {g}, {b});".format(obj); // => "color: rgb(128, 0, 255);"
// 引数がオブジェクトでない場合
var count = 100;
"time: {0} / {1}".format(count, 1000); // => "time: 100 / 1000"
```

* 文字列の連結（+）を使用しなくても良いので、より直感的に書くことができます。
* 引数がオブジェクトでない場合、引数の数に応じて、{0}、{1}、{2}・・・のように定義した文字列内で参照することができます。

### trim
文字列両サイドの空白文字を全て取り除いた文字列を返します。

```js
"  Hello, world!  ".trim(); // => "Hello, world!"
```

### capitalize
すべての単語の先頭を大文字にした文字列を返します。単語の先頭以外は小文字化されます。

```js
"i aM a pen.".capitalize(); // => "I Am A Pen."
```

### capitalizeFirstLetter
先頭の文字を大文字にして、それ以外を小文字にした文字列を返します。

```js
"i aM a pen.".capitalizeFirstLetter(); // "I am a pen."
```
  /**
   * @method toDash
   * 文字列内の大文字を「"-" + 小文字」に変換します。
   *
   * css2properties（element.style）の各プロパティ名を CSS のプロパティ名に変換する場合に便利です。
   *
   * ### Example
   *     "borderTopColor".toDash(); // => "border-top-color"
   *
   */
  String.prototype.$method("toDash", function() {
    return this.replace(/([A-Z])/g, function(m){ return '-'+m.toLowerCase(); });
  });


  /**
   * @method toHash
   * ハッシュ値を生成して返します。
   *
   * ### Example
   *     "phina.js".toHash(); // => 2676327394
   *
   * @return {Number} CRC32ハッシュ値
   */
  String.prototype.$method("toHash", function() {
    return this.toCRC32();
  });

  /**
   * @method padding
   * 左に文字を埋めて指定した桁にします。this の文字列は右寄せされます。
   *
   * ### Example
   *     "1234".padding(10);      // => "      1234"
   *     "1234".padding(10, '0'); // => "0000001234"
   *
   * @param {Number} figure 桁数
   * @param {String} [ch=" "] 埋める文字
   * @return {String} 指定した桁の文字列
   */
  String.prototype.$method("padding", function(n, ch) {
    var str = this.toString();
    n  = n-str.length;
    ch = (ch || ' ')[0];

    while(n-- > 0) { str = ch + str; }

    return str;
  });

  /**
   * @method paddingLeft
   * 左に文字を埋めて指定した桁にします。this の文字列を右寄せされます。
   *
   * {@link #padding} と同じです。
   * @inheritdoc #padding
   */
  String.prototype.$method("paddingLeft", function(n, ch) {
    var str = this.toString();
    n  = n-str.length;
    ch = (ch || ' ')[0];

    while(n-- > 0) { str = ch + str; }

    return str;
  });

  /**
   * @method paddingRight
   * 右に文字を埋めて指定した桁にします。this の文字列は左寄せされます。
   *
   * ### Example
   *     "1234".paddingRight(10);      // => "1234      "
   *     "1234".paddingRight(10, '0'); // => "1234000000"
   *
   * @param {Number} figure 桁数
   * @param {String} [ch=" "] 埋める文字
   * @return {String} 指定した桁の文字列
   */
  String.prototype.$method("paddingRight", function(n, ch) {
    var str = this.toString();
    n  = n-str.length;
    ch = (ch || ' ')[0];

    while(n-- > 0) { str = str + ch; }

    return str;
  });

  /**
   * @method quotemeta
   * 正規表現のメタ文字をクォートします。
   *
   * ### Example
   *     "Hello world. (can you hear me?)".quotemeta(); // => "Hello\\ world\\.\\ \\(can\\ you\\ hear\\ me\\?\\)"
   *
   *  @return {String} クォートされた文字列
   */
  String.prototype.$method("quotemeta", function(n) {
    return this.replace(/([^0-9A-Za-z_])/g, '\\$1');
  });

  /**
   * @method repeat
   * 自分自身を指定した回数だけ繰り返した文字列を返します。
   *
   * ### Example
   *     "Abc".repeat(4); // => "AbcAbcAbcAbc"
   *
   * @param {Number} n 繰り返し回数
   * @return {String} 文字列
   */
  String.prototype.$method("repeat", function(n) {
    // TODO: 確認する
    var arr = Array(n);
    for (var i=0; i<n; ++i) arr[i] = this;
    return arr.join('');
  });

  /**
   * @method count
   * 指定した文字列が何個入っているかをカウントして返します。
   *
   * 大文字・小文字は区別されます。
   *
   * ### Example
   *     "This is a string. Isn't it?".count("is"); // => 2
   *
   * @param {String} str 調べる文字列
   * @return {Number} this に str が入っている個数
   */
  String.prototype.$method("count", function(str) {
    var re = new RegExp(str, 'gm');
    return this.match(re).length;
  });

  /**
   * @method include
   * 指定した文字列が含まれているかどうかを返します。
   *
   * 大文字・小文字は区別されます。
   *
   * ### Example
   *     "This is a string.".include("is"); // => true
   *     "This is a string.".include("was"); // => false
   *
   * @param {String} str 調べる文字列
   * @return {Boolean} 含まれているかどうか
   */
  String.prototype.$method("include", function(str) {
    return this.indexOf(str) != -1;
  });

  /**
   * @method each
   * 各文字を順番に渡しながら関数を繰り返し実行します。
   *
   * ### Example
   *     str = 'abc';
   *     str.each(function(ch) {
   *       console.log(ch);
   *     });
   *     // => 'a'
   *     //    'b'
   *     //    'c'
   *
   * @param {Function} callback 各要素に対して実行するコールバック関数
   * @param {Object} [self=this] callback 内で this として参照される値
   */
  String.prototype.$method("each", function() {
    Array.prototype.forEach.apply(this, arguments);
    return this;
  });

  /**
   * @method toArray
   * 1文字ずつ分解した配列を返します。
   *
   * ### Example
   *     "12345".toArray(); // => ["1", "2", "3", "4", "5"]
   *     "あいうえお".toArray(); // => "あ", "い", "う", "え", "お"]
   *
   * @return {String[]} 配列
   */
  String.prototype.$method("toArray", function() {
    var arr = [];
    for (var i=0,len=this.length; i<len; ++i) {
      arr.push(this[i]);
    }
    return arr;
  });

  /**
   * @method toObject
   * キーと値の組み合わせが連結された文字列からオブジェクトを生成します。
   *
   * 値は Number、Boolean、String のいずれかの型として評価されます。
   *
   * ### Example
   *     obj1 = "num=128.5&flag1=true&flag2=false&str=hoge";
   *     obj1.toObject(); // => {num: 128.5, flag1: true, flag2: false, str: "hoge" }
   *     
   *     obj2 = "num:-64.5|flag1:false|flag2:true|str:foo";
   *     obj2.toObject('|', ':'); // => {num: -64.5, flag1: false, flag2: true, str: "foo" }
   *
   * @param {String} [sep="&"] セパレータ文字
   * @param {String} [eq=""] キーと値の組み合わせを表す文字
   * @return {Object} オブジェクト
   */
  String.prototype.$method("toObject", function(sep, eq) {
    sep = sep || '&';
    eq  = eq || '=';

    var obj = {};
    var params = this.split(sep);
    params.each(function(str, i) {
      var pos = str.indexOf(eq);
      if (pos > 0) {
        var key = str.substring(0, pos);
        var val = str.substring(pos+1);
        var num = Number(val);

        if (!isNaN(num)) {
          val = num;
        }
        else if (val === 'true') {
          val = true;
        }
        else if (val === 'false') {
          val = false;
        }

        obj[key] = val;
      }
    });

    return obj;
  });

  var table = "00000000 77073096 EE0E612C 990951BA 076DC419 706AF48F E963A535 9E6495A3 0EDB8832 79DCB8A4 E0D5E91E 97D2D988 09B64C2B 7EB17CBD E7B82D07 90BF1D91 1DB71064 6AB020F2 F3B97148 84BE41DE 1ADAD47D 6DDDE4EB F4D4B551 83D385C7 136C9856 646BA8C0 FD62F97A 8A65C9EC 14015C4F 63066CD9 FA0F3D63 8D080DF5 3B6E20C8 4C69105E D56041E4 A2677172 3C03E4D1 4B04D447 D20D85FD A50AB56B 35B5A8FA 42B2986C DBBBC9D6 ACBCF940 32D86CE3 45DF5C75 DCD60DCF ABD13D59 26D930AC 51DE003A C8D75180 BFD06116 21B4F4B5 56B3C423 CFBA9599 B8BDA50F 2802B89E 5F058808 C60CD9B2 B10BE924 2F6F7C87 58684C11 C1611DAB B6662D3D 76DC4190 01DB7106 98D220BC EFD5102A 71B18589 06B6B51F 9FBFE4A5 E8B8D433 7807C9A2 0F00F934 9609A88E E10E9818 7F6A0DBB 086D3D2D 91646C97 E6635C01 6B6B51F4 1C6C6162 856530D8 F262004E 6C0695ED 1B01A57B 8208F4C1 F50FC457 65B0D9C6 12B7E950 8BBEB8EA FCB9887C 62DD1DDF 15DA2D49 8CD37CF3 FBD44C65 4DB26158 3AB551CE A3BC0074 D4BB30E2 4ADFA541 3DD895D7 A4D1C46D D3D6F4FB 4369E96A 346ED9FC AD678846 DA60B8D0 44042D73 33031DE5 AA0A4C5F DD0D7CC9 5005713C 270241AA BE0B1010 C90C2086 5768B525 206F85B3 B966D409 CE61E49F 5EDEF90E 29D9C998 B0D09822 C7D7A8B4 59B33D17 2EB40D81 B7BD5C3B C0BA6CAD EDB88320 9ABFB3B6 03B6E20C 74B1D29A EAD54739 9DD277AF 04DB2615 73DC1683 E3630B12 94643B84 0D6D6A3E 7A6A5AA8 E40ECF0B 9309FF9D 0A00AE27 7D079EB1 F00F9344 8708A3D2 1E01F268 6906C2FE F762575D 806567CB 196C3671 6E6B06E7 FED41B76 89D32BE0 10DA7A5A 67DD4ACC F9B9DF6F 8EBEEFF9 17B7BE43 60B08ED5 D6D6A3E8 A1D1937E 38D8C2C4 4FDFF252 D1BB67F1 A6BC5767 3FB506DD 48B2364B D80D2BDA AF0A1B4C 36034AF6 41047A60 DF60EFC3 A867DF55 316E8EEF 4669BE79 CB61B38C BC66831A 256FD2A0 5268E236 CC0C7795 BB0B4703 220216B9 5505262F C5BA3BBE B2BD0B28 2BB45A92 5CB36A04 C2D7FFA7 B5D0CF31 2CD99E8B 5BDEAE1D 9B64C2B0 EC63F226 756AA39C 026D930A 9C0906A9 EB0E363F 72076785 05005713 95BF4A82 E2B87A14 7BB12BAE 0CB61B38 92D28E9B E5D5BE0D 7CDCEFB7 0BDBDF21 86D3D2D4 F1D4E242 68DDB3F8 1FDA836E 81BE16CD F6B9265B 6FB077E1 18B74777 88085AE6 FF0F6A70 66063BCA 11010B5C 8F659EFF F862AE69 616BFFD3 166CCF45 A00AE278 D70DD2EE 4E048354 3903B3C2 A7672661 D06016F7 4969474D 3E6E77DB AED16A4A D9D65ADC 40DF0B66 37D83BF0 A9BCAE53 DEBB9EC5 47B2CF7F 30B5FFE9 BDBDF21C CABAC28A 53B39330 24B4A3A6 BAD03605 CDD70693 54DE5729 23D967BF B3667A2E C4614AB8 5D681B02 2A6F2B94 B40BBE37 C30C8EA1 5A05DF1B 2D02EF8D".split(' ');

  /**
   * @method toCRC32
   * 文字列の CRC32 を計算します。
   *
   * ### Example
   *     "phina.js".toCRC32(); // => 2676327394
   *
   * @return {Number} CRC32 ハッシュ値
   */
  String.prototype.$method("toCRC32", function() {
    var crc = 0, x=0, y=0;

    crc = crc ^ (-1);
    for (var i=0, iTop=this.length; i<iTop; ++i) {
      y = (crc ^ this.charCodeAt(i)) & 0xff;
      x = "0x" + table[y];
      crc = (crc >>> 8) ^ x;
    }

    return (crc ^ (-1)) >>> 0;
  });

})();

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
