---
title: "Shape　=位置・サイズ・背景色指定="
---

## Shapeの位置指定
**Shape**の位置指定方法はいくつかあります。

####  x,yプロパティに直接指定
```js
// Shapeを作成してシーンに追加
var shape = Shape().addChildTo(this);
// 位置指定
shape.x = 320;
shape.y = 480;
```

#### setPosition関数で一括指定
**setPosition** 関数を使えば、 **x, y** の値を一括で指定することができ、生成から一気にチェインメソッドで繋げて書くこともできるので便利です。

```js
var shape = Shape().addChildTo(this).setPosition(320, 480);
```

#### **Shape**のコンストラクタで指定
```js
var shape = Shape({
  x: 320,
  y: 480
}).addChildTo(this)
```

#### 移動量で指定
* **moveBy**関数を使えば、**x, y**の移動量で位置を変更することができます。

```js
shape.setPosition(320, 480).moveBy(100, 200);
```

#### ベクトル値の加算で指定
* **Vector2**クラスを使ってベクトル値の加算で位置指定する方法もあります。

```js
var v = Vector2(100, 200);
shape.position.add(v);
```

## Shapeのサイズ指定
#### 幅指定

幅は**width**プロパティで指定します。

```js
shape.width = 128;
```

#### 高さ指定

高さは**height**プロパティで指定します。

```js
shape.height = 128;
```

#### 幅・高さを一括指定
**setSize**関数を使えば、幅と高さを一括で指定できます。

```js
shape.setSize(128, 256);
```

#### コンストラクタ内で指定
位置指定と同じくコンストラクタ内で幅・高さを指定することも可能です。

```js
    var shape = Shape({
      // 位置・幅・高さ指定
      x: 320,
      y: 480,
      width: 128,
      height: 256,
    }).addChildTo(this);
```

## 背景色指定

背景色は**backgroundColor**プロパティで指定します。**CSS**と同じ感覚で指定できます。

#### 文字列で指定
```js
shape.backgroundColor = 'red';
```

#### 16進数で指定
```js
shape.backgroundColor = '#ffff00';
```

#### RGB値で指定
```js
shape.backgroundColor = `rgb(0, 255, 255)`;
```
#### hsl値で指定
```js
shape.backgroundColor = `hsl(300, 75%, 50%)`;
```
