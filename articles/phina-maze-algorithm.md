---
title: "【phina.js】迷路生成アルゴリズムとtweenerを組み合わせた表現"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: true
---

## はじめに

以前に代表的な迷路生成アルゴリズムを**phina.js**で作っていたのですが、今回それを**tweener**を使ったアニメーションとして視覚化してみました。

## 穴掘り法 アルゴリズム

* 最初にすべてのマスを埋める。
* 掘り始めるマスをランダムに選ぶ。
* 選んだマスから、上下左右でランダムな方向に2マス先が掘れるかを調べる。
* 掘れるのであれば、2マス先まで掘る。
* 掘った先を分岐点として登録し、再帰処理で調べる。
* どこにも掘れなくなった分岐点は、候補から削除する。
* 登録した分岐点がなくなるまで繰り返す。

![digmaze.gif](/images/digmaze.gif)

## 穴掘り法 ソースコード

::: details コードを見る

```js
phina.globalize();
// 定数
var TILE_SIZE = 64; // 壁のサイズ
var WALL_NUM_X = 9; // 横の壁数（奇数）              
var WALL_NUM_Y = 15; // 縦の壁数（奇数）              
var MAZE_WIDTH = TILE_SIZE * WALL_NUM_X; // 迷路横サイズ
var MAZE_HEIGHT = TILE_SIZE * WALL_NUM_Y; // 迷路縦サイズ
var OFFSET = TILE_SIZE / 2;
var FLOOR = 0;
var WALL = 1;
// 上下左右方向ベクトル
var DIR_ARRAY = [Vector2.UP, Vector2.DOWN, Vector2.LEFT, Vector2.RIGHT];
// アセット
var ASSETS = {
  // 画像
  image: {
    'tile': 'https://cdn.jsdelivr.net/gh/alkn203/assets_etc@master/tiles3.png',
  },
};
// マップデータ
var MAP = [
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1],
  [1,1,1,1,1,1,1,1,1]];

// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: MAZE_WIDTH,
      height: MAZE_HEIGHT,
    });
    // マップクラス
    this.map = phina.util.Map({
      tileWidth: TILE_SIZE,
      tileHeight: TILE_SIZE,
      imageName: 'tile',
      mapData: MAP,
    }).addChildTo(this);
    // 分岐点候補
    this.branches = [];
    // 穴を掘るば位置
    this.digPos = [];
       // 最初の穴掘り（奇数位置）
    var randI = Array.range(1, WALL_NUM_X, 2).random();
    var randJ = Array.range(1, WALL_NUM_Y, 2).random();
    this.map.setTileByIndex(randI, randJ, FLOOR);
    // 起点に穴掘り開始
    this.dig(randI, randJ);
    //
    this.showDig();
  },
  // 穴掘り処理
  dig: function(i, j) {
    
    var map = this.map;
    var self = this;
    
    var nextI = i;
    var nextJ = j;
    // 分岐点登録
    this.registBranch(i, j);
    // ランダムな順番で
    var canDig = DIR_ARRAY.shuffle().some(function(direction) {
      var i2 = i + direction.x * 2;
      var j2 = j + direction.y * 2;
      // 掘れる範囲なら
      if ((i2 > 0 && i2 < WALL_NUM_X) && (j2 > 0 && j2 < WALL_NUM_Y)) {
        // 2マス先が壁か調べる
        if (map.checkTileByIndex(i2, j2) === WALL) {
          // 壁なら2マス先まで掘るフラグ設定
          map.setTileByIndex(i + direction.x, j + direction.y, -1);
          map.setTileByIndex(i2, j2, -1);
          // 掘る位置を格納
          self.digPos.push(Vector2(i + direction.x, j + direction.y));
          self.digPos.push(Vector2(i2, j2));
          // 次の起点
          nextI = i2;
          nextJ = j2;
          return true;
        }
      }
    });
    // 掘り進められるのであれば
    if (canDig) {
      // 2マス先を開始位置にして再帰処理
      this.dig(nextI, nextJ);
   }
    else {
      // 分岐点として使えないので削除
      this.deleteBranch(nextI, nextJ);
      // これまでの分岐点から掘り進めるものを探す
      this.searchBranch();
    }
  },
  // 分岐点を登録
  registBranch: function(indexI, indexJ) {
    // ダブり回避
    var result = this.branches.some(function(branch) {
      return (branch.i === indexI && branch.j === indexJ);
    });
    
    if (!result) {
      this.branches.push({i: indexI, j: indexJ});
    }
  },
  // 使えない分岐点を削除
  deleteBranch: function(indexI, indexJ) {
    this.branches.eraseIfAll(function(branch) {
      return (branch.i === indexI && branch.j === indexJ);
    });
  },
  // 使える分岐点を探す
  searchBranch: function() {
    if (this.branches.length > 0) {
      var rand = this.branches.random();
      this.dig(rand.i, rand.j);
    }
  },
  // 徐々に穴を掘る様子を見せる
  showDig:function() {
    var self = this;

    this.digPos.each(function(pos, i) {
      self.tweener.wait(100)
                  .call(function() {
                     self.map.setTileByIndex(pos.x, pos.y, FLOOR);
                  });
    });
    this.tweener.play();
  },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    width: MAZE_WIDTH,
    height: MAZE_HEIGHT,
    assets: ASSETS,
  });
  app.run();
});
```

:::

## runstant

<https://runstant.com/alkn203/projects/c2b2cb2b>

## クラスタリング法　アルゴリズム

* マスを部屋とみなし、固有の部屋番号を振る。
* 部屋と部屋の間の上下左右に壁を設置する。
* 壁をランダムに選ぶ。
* 壁を挟んで隣同士の部屋の番号が異なっていたら、壁を取り除いて部屋番号を一方の番号に統一する。
* 隣同士の部屋の番号が同じなら、壁は取り除かない。
* すべての部屋番号が同じになるまで、壁を選ぶ処理を繰り返す。

![clusteringmaze.gif](/images/clusteringmaze.gif)

## クラスタリング法 ソースコード

::: details コードを見る

```js
phina.globalize();
// 定数
var ROOM_SIZE = 64; // 部屋のサイズ
var WALL_X_WIDTH = ROOM_SIZE; // 横壁の幅
var WALL_X_HEIGHT = ROOM_SIZE / 10; // 横壁の高さ
var WALL_Y_WIDTH = WALL_X_HEIGHT; // 縦壁の幅
var WALL_Y_HEIGHT = WALL_X_WIDTH; // 縦壁の高さ
var ROOM_NUM_X = 10; // 横の部屋数              
var ROOM_NUM_Y = 15; // 縦の部屋数              
var MAZE_WIDTH = ROOM_SIZE * ROOM_NUM_X; // 迷路横サイズ
var MAZE_HEIGHT = ROOM_SIZE * ROOM_NUM_Y; // 迷路縦サイズ
var OFFSET = ROOM_SIZE / 2;
// メインシーン
phina.define('MainScene', {
  superClass: 'DisplayScene',
  // コンストラクタ
  init: function() {
    // 親クラス初期化
    this.superInit({
      width: MAZE_WIDTH,
      height: MAZE_HEIGHT,
    });
    // 背景色
    this.backgroundColor = 'gray';
    // グリッド
    var gx = Grid(MAZE_WIDTH, ROOM_NUM_X);
    var gy = Grid(MAZE_HEIGHT, ROOM_NUM_Y);
    // グループ
    var roomGroup = DisplayElement().addChildTo(this);
    var wallGroup = DisplayElement().addChildTo(this);
    // 迷路初期化
    ROOM_NUM_X.times(function(i) {
      ROOM_NUM_Y.times(function(j) {
        // 部屋作成
        var room = Room().addChildTo(roomGroup);
        // Gridを利用して配置
        room.setPosition(gx.span(i) + OFFSET, gy.span(j) + OFFSET);
        // 部屋番号
        room.num = j * ROOM_NUM_X + i;
        room.label = Label(room.num).addChildTo(room);
        // 横壁作成
        if (j > 0) {
          var wallx = WallX().addChildTo(wallGroup);
          wallx.setPosition(room.x, room.top);
        }
        // 縦壁作成
        if (i > 0) {
          var wally = WallY().addChildTo(wallGroup);
          wally.setPosition(room.left, room.y);
        }
        // 部屋は非表示
        room.hide();
      });
    });
    // 壊すべき壁
    this.wallToRemove = [];
    // 参照用
    this.roomGroup = roomGroup;
    this.wallGroup = wallGroup;
    
    this.breakWall();
    
    this.showBreak();
  },
  // ランダムに壁を壊す
  breakWall: function() {
    var wall = this.wallGroup.children.random();
    // 横壁
    if (wall.width === WALL_X_WIDTH) {
      var roomTop = this.getRoom(wall.x, wall.y - ROOM_SIZE / 2);
      var roomBottom = this.getRoom(wall.x, wall.y + ROOM_SIZE / 2);

      if (roomTop.num !== roomBottom.num) {
        if (roomTop.num > roomBottom.num) {
          this.replaceRoomNumber(roomTop.num, roomBottom.num);
        }
        else {
          this.replaceRoomNumber(roomBottom.num, roomTop.num);
        }
        //wall.remove();
        this.wallToRemove.push(wall);
      }
    } 
    // 縦壁
    else {
      var roomLeft = this.getRoom(wall.x - ROOM_SIZE / 2, wall.y);
      var roomRight = this.getRoom(wall.x + ROOM_SIZE / 2, wall.y);

      if (roomLeft.num !== roomRight.num) {
        if (roomLeft.num > roomRight.num) {
          this.replaceRoomNumber(roomLeft.num, roomRight.num);
        }
        else {
          this.replaceRoomNumber(roomRight.num, roomLeft.num);
        }
        //wall.remove();
        this.wallToRemove.push(wall);
      }
    }
    // 終了チェック
    if (!this.isRoomSameNumberAll()) {
      this.breakWall();    
    }
  },
  // 全ての部屋が同じ番号かどうかチェック
  isRoomSameNumberAll: function() {
    var first = this.roomGroup.children.first;
    var result = true;
    
    this.roomGroup.children.some(function(room) {
      if (room.num !== first.num) {
        result = false;
        return true;
      } 
    });
    return result;
  },
  // 指定された部屋番号に書き換える
  replaceRoomNumber: function(numFrom, numTo) {
    this.roomGroup.children.each(function(room) {
      if (room.num === numFrom) {
        room.num = numTo;
        room.label.text = numTo;
      } 
    });
  },
  // 指定された位置の部屋を得る
  getRoom: function(x, y) {
    var result = null;
    
    this.roomGroup.children.some(function(room) {
      if (room.x === x && room.y === y) {
        result = room;
        return true;
      } 
    });
    return result;
  },
  //
  showBreak:function() {
    var self = this;
    
    this.wallToRemove.each(function(wall) {
      self.tweener.wait(100)
                  .call(function() {
                    wall.remove();
                  });
    });
    this.tweener.play();
  },
});
// 部屋クラス
phina.define('Room', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: ROOM_SIZE,
        height: ROOM_SIZE,
        fill: null,
        stroke: null,
      });
    },
});
// 横壁クラス
phina.define('WallX', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: WALL_X_WIDTH,
        height: WALL_X_HEIGHT,
        fill: 'silver',
        stroke: null,
      });
    },
});
// 縦壁クラス
phina.define('WallY', {
  // RectangleShapeを継承
  superClass: 'RectangleShape',
    // コンストラクタ
    init: function() {
      // 親クラス初期化
      this.superInit({
        width: WALL_Y_WIDTH,
        height: WALL_Y_HEIGHT,
        fill: 'silver',
        stroke: null,
      });
    },
});
// メイン
phina.main(function() {
  var app = GameApp({
    startLabel: 'main',
    width: MAZE_WIDTH,
    height: MAZE_HEIGHT,
  });
  app.run();
});


```

:::

## runstantプロジェクト

<https://runstant.com/alkn203/projects/dcf0eadb>
