---
title: "Godot Engine Tips"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Godot"]
published: false
---

# Godot Engineの主に自分用Tips

## ノードのマウスクリック判定
ノードのシグナル接続から設定

```gdscript
func _on_Piece_input_event(viewport, event, shape_idx):
    # マウス入力イベントが発生
    if event is InputEventMouseButton:
        # マウスボタンの押下イベント
        if event.is_pressed():
            # 親の関数を呼び出す
            get_parent().move_piece(self)
```

## 子要素をインデックス参照しながらループ

```gdscript
for piece in get_children():
    var index = piece.get_index()
    piece.get_node("Sprite").frame = index
```

## 独自クラスのコード補完が効くようにする
以下の場合、MyPanel.gdに記載

```gdscript
extends Area2D
class_name MyPanel
```
## 配列シャッフル
randomize()は必須

```gdscript
# 乱数初期化
randomize()
# 配列をシャッフル
bomb_array.shuffle()
```

## 型指定
```gdscript
var sprite: Sprite = panel.get_node("Sprite")
```

# フレーム処理の一番最後にプロパティを設定する　　　
```gdscript
panel.get_node("CollisionShape2D").set_deferred("disabled", true)
```
