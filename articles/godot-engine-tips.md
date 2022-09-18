---
title: "Godot Engine Tips"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Godot","Godot Engine"]
published: true
---

# Godot Engineの備忘録的Tips

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
class_name MyPanel
extends Area2D
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

## RigidBody2D関連
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

## Area2Dの単純移動処理
```gdscript
func _process(delta):
    position.y += speed * delta
```

## ポーズ処理
```gdscript
get_tree().paused = true
```
## 掛け捨てタイマー
```gdscript
yield(get_tree().create_timer(1.0), "timeout")
```

## シーン遷移
```gdscript
get_tree().change_scene("res://Title.tscn")
```

## 動的インスタンス作成
```gdscript
onready var beam_scene = preload("res://Beam.tscn")
var beam = beam_scene.instance()
beam.position = position
beam_layer.add_child(beam)
```
## ルートから辿ったノード取得
```gdscript
onready var beam_layer = get_node("/root/Main/BeamLayer")
```

# スプライトアニメーションの終了時にオブジェクトを削除
```gdscript
extends AnimatedSprite

## destroy self when animation is finished.
func _on_Explosion_animation_finished():
    queue_free()
```

## タイマー停止と再開処理
```gdscript
func _process(delta):
    #
    if ufo_timer.is_stopped():
        var ufo = ufo_scene.instance()
        ufo.position = Vector2(screen_size.x, 64)
        ufo_layer.add_child(ufo)
        ufo_timer.start()
```

## 画面から出たオブジェクトを削除
```gdscript
func _on_Screen_area_exited(area):
    area.queue_free()
```

## コリジョンマスクの変更
エディタのレイヤー番号マイナス1で指定

```gdscript
# レイヤー3との当たり判定を有効に
body.set_collision_mask_bit(2, true)