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

## グループ関係
インスタンス作成時に自動でグループに追加されるように、ノードのインスペクタから設定も可能

```gdscript
# 指定のグループに所属しているノードを配列として返す
var opened_arr = get_tree().get_nodes_in_group("open_hand")
# グループに追加
opened.add_to_group("drop_hand")
# グループから削除する
opened.remove_from_group("open_hand")
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

# 指定範囲でランダムな整数
デフォルトでは専用メソッドがないので余りを使う。
```gdscript
# 0から6でランダムな整数
var num = randi() % 7
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
サイズのプロパティがないため。
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
const Beam = preload("res://Beam.tscn")
var beam = Beam.instance()
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
画面と同サイズのArea2Dを作成
```gdscript
func _on_Screen_area_exited(area):
    area.queue_free()
```

## コリジョンマスクの変更
エディタのレイヤー番号マイナス1で指定

```gdscript
# レイヤー3との当たり判定を有効に
body.set_collision_mask_bit(2, true)
```

## TileMapでヒットしたタイルの情報を得る
```gdscript
var collision = move_and_collide(velocity * delta)
# Confirm the colliding body is a TileMap
if collision:
	if collision.collider is TileMap:
		# Find the character's position in tile coordinates
		var tile_pos = collision.collider.world_to_map(position)
		# Find the colliding tile position
		tile_pos -= collision.normal
		# Get the tile id
		var tile_id = collision.collider.get_cellv(tile_pos)
```


## Area2DとKinematicBody2Dの当たり判定
Area2D側から行う必要あり。
```gdscript
# 当たり判定
func _on_Explosion_body_entered(body):
	# 当たった相手のやられ処理
	body.disable()
```

## TileMapで座標とタイル座標を相互変換する
```gdscript
var bomb = Bomb.instance()
bomb.tile_pos = tilemap.world_to_map(position)
bomb.position = tilemap.map_to_world(bomb.tile_pos)
bomb_layer.add_child(bomb)
```

## TileMapでタイル情報を参照する・置き換える
位置はVector2で指定

```gdscript
# 指定した位置のタイルをチェック
var tile = tilemap.get_cellv(tile_pos)
# タイルを床に置き換える
tilemap.set_cellv(tile_pos, NONE)
```

## Tween関係（Ver.3.5+）

```gdscript
# 移動後コールバック関数呼び出し
var tween1 = get_tree().create_tween()
tween1.tween_property(g1, "position", g2.position, SWAP_DURATION)
tween1.tween_callback(self, "_after_swap")

# 縮小・削除後コールバック関数呼び出し
tween.tween_property(dummy, "scale", Vector2(), SWAP_DURATION)
tween.tween_callback(dummy, "queue_free")
tween.tween_callback(self, "_after_remove")

# 並行処理と連続処理の切り替え
var tween = get_tree().create_tween()
tween.set_parallel(true)

for dummy in dummy_layer.get_children():
  tween.tween_property(dummy, "scale", Vector2(), DURATION)

tween.set_parallel(false)
tween.tween_callback(self, "_after_remove")
```

