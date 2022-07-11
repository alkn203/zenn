---
title: "[phina.js]Collisionクラスを使った当たり判定"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# Collisionクラスについて
**phina.geom.Collision**は、当たり判定を行うためのクラスです。
**phina.js**の当たり判定で良く使うのは、**phina.app.Object2d**クラスの**hitTestElement**メソッドかと思いますが、このメソッドを使うためには以下のようにインスタンス経由となります。

```js
obj.hitTestElement(target);
```

対して、**Collision**クラスのメソッドは全て**static**なメソッドとなっています。

```js
Collision.メソッド名(引数1, 引数2);
```

# Collisionクラスのメソッド
## testCircleCircle
円同士が重なっているかを判定します。

## testRectRect
矩形同士が重なっているかを判定します。

## testCircleRect
円と矩形が重なってるかを判定します。

## testCircleLine
円と線分が重なってるかを判定します。

![collision-class.gif](/images/collision-class.gif)

# サンプルコード
:::details コードを見る
```js
// グローバルに展開
phina.globalize();
/*
 * メインシーン
 */
phina.define("MainScene", {
  // 継承
  superClass: 'DisplayScene',
  // 初期化
  init: function() {
    // 親クラス初期化
    this.superInit();
    // 背景色
    this.backgroundColor = 'black';
    // ラベル1
    this.label1 = Label({
      text: '図形をドラッグして重ねよう',
      fill: 'white',
      fontSize: 48,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center(-6));
    // ラベル2
    this.label2 = Label({
      fill: 'white',
      fontSize: 48,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center(6));
    // 円1
    var circle1 = CircleShape({
      fill: null,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center(-2));
    // ドラッグ可能にする
    Draggable().attachTo(circle1);
    // 円2
    var circle2 = CircleShape({
      fill: null,
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center(2));
    // ドラッグ可能にする
    Draggable().attachTo(circle2); 
    // 矩形1
    var rect1 = RectangleShape({
      fill: null,
    }).addChildTo(this).setPosition(this.gridX.center(-3), this.gridY.center());
    Draggable().attachTo(rect1);
    // 矩形2
    var rect2 = RectangleShape({
      fill: null,
    }).addChildTo(this).setPosition(this.gridX.center(3), this.gridY.center());
    Draggable().attachTo(rect2);
    // canvas要素描画用
    var elem  = PlainElement({
      width: this.gridX.width,
      height: this.gridY.width, 
    }).addChildTo(this).setPosition(this.gridX.center(), this.gridY.center());
    // canvasパラメータ指定
    elem.canvas.context.strokeStyle = 'white';
    elem.canvas.context.lineWidth = 4;
    // 線分の両端の座標
    this.points = [Vector2(this.gridX.center(-3), this.gridY.center(-3)),
                   Vector2(this.gridX.center(3), this.gridY.center(3))];
    
    var p1 = this.points[0];
    var p2 = this.points[1];
    // 線を引く
    elem.canvas.drawLine(p1.x, p1.y, p2.x, p2.y);
    // 参照用
    this.circle1 = circle1;
    this.circle2 = circle2;
    this.rect1 = rect1;
    this.rect2 = rect2;
  },

  update: function() {
    var c1 = this.circle1;
    var c2 = this.circle2;
    var r1 = this.rect1;
    var r2 = this.rect2;
    // 円同士の当たり判定
    if (Collision.testCircleCircle(c1, c2)) {
      this.label2.text = 'Circle And Circle';
      return;
    }
    // 矩形同士の当たり判定
    if (Collision.testRectRect(r1, r2)) {
      this.label2.text = 'Rect And Rect';
      return;  
    }
    // 円と矩形の当たり判定
    if (Collision.testCircleRect(c1, r1) || Collision.testCircleRect(c1, r2) ||
        Collision.testCircleRect(c2, r1) || Collision.testCircleRect(c2, r2)) {
          this.label2.text = 'Circle And Rect';
          return;  
    }

    var p1 = this.points[0];
    var p2 = this.points[1];
    var bc1 = Circle(c1.x, c1.y, c1.radius);
    var bc2 = Circle(c2.x, c2.y, c2.radius);
    // 円と線分の当たり判定
    if (Collision.testCircleLine(bc1, p1, p2) ||
        Collision.testCircleLine(bc2, p1, p2)) {
          this.label2.text = 'Circle And Line';
          return;  
    }
    
    this.label2.text = '';
  },
});
/*
 * メイン処理
 */
phina.main(function() {
  // アプリケーションを生成
  var app = GameApp({
    // MainScene から開始
    startLabel: 'main',
  });
  // 実行
  app.run();
});
```
:::
[runstantで確認](https://runstant.com/alkn203/projects/9872a6f6)

# おわりに
現在の**Collision**クラスで大体の当たり判定の処理はできるかと思いますが、**線分と線分**、**矩形と線分**、**多角形と多角形**といった当たり判定を追加しても良いかもしれません。ライブラリ開発に少しでもコントリビュートしていけるのが**phina.js**の醍醐味でもあります。