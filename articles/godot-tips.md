P---
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

## フレーム処理の一番最後にプロパティを設定する　　　
```gdscript
panel.get_node("CollisionShape2D").set_deferred("disabled", true)
```

## RigidBody2D
### 初動処理

```gdscript
direction = Vector2(0, 1)
velocity = direction * ball_speed
apply_impulse(Vector2.ZERO, velocity)
```
### 定速度変更
```gdscript
# 衝突位置で反射角度を変える
direction = (position - body.position).normalized()
velocity = direction * ball_speed
linear_velocity = velocity
```
## 衝突したオブジェクトの判定
```gdscript
func _on_Ball_body_entered(body):
    # ブロックとの衝突
    if body.is_in_group("Blocks"):
        body.queue_free()
    # パドルとの衝突
    if body.get_name() == "Paddle":
```

## エディタのインスペクタからプロパティを設定可能にする
```gdscript
export (int) var frame_index = 0
```

## スプライトのサイズ取得
```gdscript
var sprite_size = get_node("Sprite").texture.get_size()
```

## 画面サイズ取得
```gdscript
onready var screen_size = get_viewport_rect().size
```

## マウスドラッグ判定
```gdscript
var dragging = false

func _input(event):
    if event is InputEventMouseButton:
        if event.pressed:
        # ドラッグフラグ
        dragging = true
        else:
            dragging = false

    if event is InputEventMouseMotion:
        if dragging:
            # マウス位置取得
		var pos = event.position
                # パドルの横座標を追従させ、画面からはみ出ないようにする
                var pos_min = sprite_size.x / 2
                var pos_max = screen_size.x - sprite_size.x / 2
                position.x = clamp(pos.x, pos_min, pos_max)
```







