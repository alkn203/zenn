---
title: "【phina.js】迷路生成アルゴリズムとtweenerを組み合わせた表現"
emoji: "🐦"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["phina","javascript","html5","ゲーム開発"]
published: false
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

## 実行結果
![floodfill.gif](/images/floodfill.gif)

## ソースコード
::: details コードを見る
```js

```

## runstant
https://runstant.com/alkn203/projects/c2b2cb2b

## クラスタリング法　アルゴリズム
* マスを部屋とみなし、固有の部屋番号を振る。
* 部屋と部屋の間の上下左右に壁を設置する。
* 壁をランダムに選ぶ。
* 壁を挟んで隣同士の部屋の番号が異なっていたら、壁を取り除いて部屋番号を一方の番号に統一する。
* 隣同士の部屋の番号が同じなら、壁は取り除かない。
* すべての部屋番号が同じになるまで、壁を選ぶ処理を繰り返す。

## 実行結果
![scanlineseedfill.gif](/images/scanlineseedfill.gif)

## ソースコード
::: details コードを見る
```js


```

## runstantプロジェクト
https://runstant.com/alkn203/projects/dcf0eadb
