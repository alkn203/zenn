---
title: "[phina.js]オブジェクトの表示順について"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

# はじめに
**phina.js**には、 **CSS**の**z-index**のように表示順を指定できる機能は現時点ではありません。しかしながら、表示順の仕組みを理解して工夫することで、ある程度コントロールすることができます。

# オブジェクトの表示順
**phina.js**では、シーンに**後で追加された**オブジェクトほど前面に表示されます。

# シーンに直接追加
```js
var rect1 = RectangleShape().addChildTo(this);
var rect2 = RectangleShape().addChildTo(this);
```

rect2がrect1よりも前面に表示されます。

# グループに追加
```js
var group = DisplayElement().addChildTo(this);
var rect1 = RectangleShape().addChildTo(group);
var rect2 = RectangleShape().addChildTo(group);
```

後からグループに追加されたものが前面に表示されるので、rect2がrect1より前面に表示されます。

# 直接追加とグループの併用
```js
var group = DisplayElement().addChildTo(this);
var rect1 = RectangleShape().addChildTo(this);
var rect2 = RectangleShape().addChildTo(group);
```

* rect2はrect1よりも後に宣言していますが、rect1よりも先にシーンに追加されたグループに追加しています。
* 結果的にrect1がrect2よりも前面に表示されます。

# グループのみ
```js
var group1 = DisplayElement().addChildTo(this);
var group2 = DisplayElement().addChildTo(this);
var rect1 = RectangleShape().addChildTo(group2);
var rect2 = RectangleShape().addChildTo(group1);
```

* 先にグループ毎に表示順を決めておくパターンです。
* 個別の要素の追加順に関係なく、グループの表示順に従うことになります。

# 表示順の変更
シーンに直接追加だと後々操作が不便なので、1つのオブジェクトの場合でもグループに追加していた方が良いです。

# 同じグループ内での変更

```js
var group = DisplayElement().addChildTo(this);
var rect1 = RectangleShape().addChildTo(group);
var rect2 = RectangleShape().addChildTo(group);
group.children.swap(0, 1);
```

* **children**で配列を参照できるので、要素の順番を入れ替えれば表示順を変えることができます。
* 上の例では、**Array**クラスの拡張メソッド**swap**で1番目の要素と2番目の要素を入れ替えています。

[実行サンプル(runstant)](http://runstant.com/alkn203/projects/fd104bc3)

# グループ同士での変更

```js
var group0 = DisplayElement().addChildTo(this);
var group1 = DisplayElement().addChildTo(group0);
var group2 = DisplayElement().addChildTo(group0);
var rect = RectangleShape().addChildTo(group1);
var circle = CircleShape().addChildTo(group2);
group0.children.swap(0, 1);
```

2つのグループを親の役割を持つ別のグループに追加して、グループ毎入れ替えています。

[実行サンプル(runstant)](http://runstant.com/alkn203/projects/89829593)

# 応用例
* 現在のphina.jsでは、タッチイベントはオブジェクトの重なりを考慮していないので、タッチした位置にあるオブジェクトが全て反応してしまいます。
* これまでのテクニックを使って、 **前面のオブジェクトだけをタッチで消せる**ようにしたのが下のサンプルです。

[実行サンプル(runstant)](http://runstant.com/alkn203/projects/08c03bcd)

# サンプルコード
:::details コードを見る
```javascript
// グローバルに展開
phina.globalize();
// 定数
var SCREEN_WIDTH = 640;
var SCREEN_HEIGHT = 960;
var SHAPE_SIZE = 100;
var SHAPE_HALF = SHAPE_SIZE / 2;
var NUM = 25;
var SATURATION = 100;	// 彩度
var LIGHTNESS = 50;	// 輝度
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
    // グループ
    var group = DisplayElement().addChildTo(this);
    var self = this;
    // 繰り返し処理
    (NUM).times(function(i) {
      // RectangleShapeを作成してシーンに追加
      var rect = RectangleShape({
        width: SHAPE_SIZE,
        height: SHAPE_SIZE,
      }).addChildTo(group);
      // 画面上に収まるランダムな位置に配置
      rect.x = Random.randint(SHAPE_HALF, SCREEN_WIDTH - SHAPE_HALF);
      rect.y = Random.randint(SHAPE_HALF, SCREEN_HEIGHT - SHAPE_HALF);
      // 背景色をランダムに設定
      var hue = Random.randint(0, 360);
      rect.fill = 'hsl({0}, 75%, 50%)'.format(hue);
      // タッチ時処理
      rect.onpointend = function() {
        rect.tweener.to({scaleX: 0.1, scaleY: 0.1}, 100)
                    .call(function() {
                      rect.remove();
                      self.setRectInteraction();
                    }).play();
      };
    });
    // 参照用
    this.group = group;
    this.setRectInteraction();
  },
  // タッチ可否設定 
  setRectInteraction: function() {
    // 全体を一旦タッチ可能にする
    this.group.children.each(function(rect) {
      rect.setInteractive(true);
    }); 
    var self = this;
    // グループ総当たりで重なり具合に応じてタッチ可否を設定する
    this.group.children.each(function(rect, i) {
      self.group.children.each(function(target, j) {
        // 重なっていて表示順が下のターゲットはタッチ不可にする
        if (rect.hitTestElement(target) && j < i) {
          target.setInteractive(false);
        }
      });
    });
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
  // fps表示
  //app.enableStats();
  // 実行
  app.run();
});
```
:::