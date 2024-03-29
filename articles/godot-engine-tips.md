---
title: "Godot Engine Tips"
emoji: "✏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Godot","GodotEngine"]
published: true
---

# Godot Engineの備忘録的Tips

## マウスクリック判定

```gdscript
func _input(event):
  if event is InputEventMouseButton:
    if event.button_index == BUTTON_LEFT and event.pressed:
      print("Left button was clicked at ", event.position)
```

## ノードのマウスクリック判定

ノードのシグナル接続から設定

```gdscript
func _on_Piece_input_event(viewport, event, shape_idx):
  # マウス入力イベントが発生
  if event is InputEventMouseButton:
    # マウスボタンの押下イベント
    if event.is_pressed():
      pass
```

## 子要素をインデックス参照しながらループ

```gdscript
for piece in get_children():
  var index = piece.get_index()
  piece.get_node("Sprite").frame = index
```

## ColorRect

デフォルトのままだとマウス入力を遮断する設定になっている。**Mouse**の**Filter**を**Ignore**に変更すればOK。

## グループ関係

インスタンス作成時に自動でグループに追加されるように、ノードのインスペクタから設定も可能

```gdscript
# 指定のグループに所属しているかどうか
node.is_in_group("group")
# 指定のグループに所属しているノードを配列として返す
var array = get_tree().get_nodes_in_group("group")
# グループに追加
node.add_to_group("group")
# グループから削除する
node.remove_from_group("group")
```

## 独自クラスのコード補完が効くようにする

以下の場合、MyClass.gdに記載

```gdscript
class_name MyClass
extends Area2D
```

## 配列シャッフル

randomize()は必須

```gdscript
# 乱数初期化
randomize()
# 配列をシャッフル
array.shuffle()
```

## 指定範囲でランダムな整数

デフォルトでは専用メソッドがないので余りを使う。

```gdscript
# 0から6でランダムな整数
var num = randi() % 7
```

## 型指定

```gdscript
var sprite: Sprite = panel.get_node("Sprite")
```

## 型判定
intなどの基本型とNode両方に使用可能

```gdscript
if target is Sprite:
    do something
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
const BeamScene = preload("res://Beam.tscn")
var beam = BeamScene.instance()
beam.position = position
beam_layer.add_child(beam)
```

## ルートから辿ったノード取得

```gdscript
onready var beam_layer = get_node("/root/Main/BeamLayer")
```

## スプライトアニメーションの終了時にオブジェクトを削除

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

## オブジェクトが床の上にいるかの判定

**KinematicBody**と**move_and_slide**を使用。
**move_and_slide**の第２引数に、法線ベクトルを指定する必要あり。

```gdscript
# 毎フレーム処理
func _physics_process(delta):
  # 移動と当たり判定
  velocity = move_and_slide(velocity, Vector2(0, -1))
  # 床の上なら
  if is_on_floor():
    # アニメーション変更
    if animated_sprite.animation != "default":
      animated_sprite.play("default")
    # 左クリックでジャンプ
    if Input.is_action_just_pressed("ui_left_click"):
      velocity = Vector2(0, -JUMP_POWER)
      # アニメーション変更
      animated_sprite.play("jump")
  # 重力を加算
  velocity += Vector2(0, GRAVITY)
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

## Area2Dのキーボード移動

```gdscript
extends Sprite

# 定数
const PLAYER_SPEED = 5
const KEY_ARRAY = [
    ["ui_down", Vector2(0, 1)],
    ["ui_up", Vector2(0, -1)],
    ["ui_left", Vector2(-1, 0)],
    ["ui_right", Vector2(1, 0)]]

# 毎フレーム処理
func _process(delta):
  var velocity = Vector2.ZERO
  
  for elem in KEY_ARRAY:
    var dir = elem[0]
    # キーにより方向振り分け
    if Input.is_action_pressed(dir):
      velocity = elem[1]
  # 何かしら入力があれば
  if velocity.x != 0 or velocity.y != 0:
    # プレイヤー位置更新
    position += velocity * PLAYER_SPEED
```

## KinematicBody2Dのキーボード移動

```gdscript
extends KinematicBody2D

const KEY_ARRAY = [
    ["ui_down", Vector2(0, 1)],
    ["ui_up", Vector2(0, -1)],
    ["ui_left", Vector2(-1, 0)],
    ["ui_right", Vector2(1, 0)]]
# プレイヤーの速度
export (int) var speed = 150
# 移動方向ベクトル
var velocity = Vector2(0, 0)

# 毎フレーム処理
func _physics_process(delta):
  # 移動入力受付
  velocity = Vector2.ZERO
  
  for elem in KEY_ARRAY:
    var dir = elem[0]
    # キーにより方向振り分け
    if Input.is_action_pressed(dir):
      velocity = elem[1]
      
  velocity = velocity.normalized() * speed
  # 移動と当たり判定
  var collision = move_and_collide(velocity * delta)

```

## KinematicBody2Dの当たり判定と反射処理

### move_and_collide

```gdscript
# 毎フレーム処理
func _physics_process(delta):
    # 移動と当たり判定
    var collision = move_and_collide(velocity * delta)
    # ヒットあり
    if collision:
        # 反転
        velocity = velocity.bounce(collision.normal)
```


### move_and_slide

move_and_slideの返り値は、衝突後のベクトルとなるため、衝突前のベクトルを変数に退避させておく必要がある。

```gdscript
# 毎フレーム処理
func _physics_process(delta) :
  var prev_velocity = velocity
  # 移動と当たり判定
  velocity = move_and_slide(velocity, Vector2.UP)
  # 壁に接触なら
  if is_on_wall():
    # 反射移動処理
    if get_slide_count() > 0:
      var collision = get_slide_collision(0)
      velocity = prev_velocity.bounce(collision)

```

## ある点を中心に指定角度回転させた座標

```gdscript
# 度からラジアンへ変換
var angle: float = deg2rad(90)
# 回転の原点
var point: Vector2 = children.front().position
# 個々のブロックを90度回転
for block in children:
  block.position = point + (block.position - point).rotated(angle)
```

## 外部エディタ使用設定
Visual Studo Codeの場合
* エディタ→エディタ設定→テキストエディタ→外部
* 実行パス：/usr/bin/code
* 実行フラグ：{project} --goto {file}:{line}:{col}

## ボタンのフォントをスクリプトから設定

```gdscript
var font = DynamicFont.new()
    font.font_data = load("res://mplus-1c-regular.ttf")
    button.set("custom_fonts/font", font)
    button.get("custom_fonts/font").set_size(48)
```

## シェーダー
* インスペクタから CanvasItem > Material > [空] をクリックして、「新規 ShaderMaterial」を選ぶ。
* 表示された Material をクリックして、Shader > [空] をクリックして、「新規Shader」を選ぶ。
* 「Shader」の文字をクリックすると、シェーダーエディタが開く。

### スプライトをグレイスケール化
```glsl
// CanvasItemのシェーダーであることを宣言
shader_type canvas_item;
// 外部からオンオフできるようにする
uniform bool greyscale = false;
// フラグメントシェーダー
void fragment() {
    if (greyscale) {
        // 色を取得
        vec4 color = texture(TEXTURE, UV);
        // グレイスケール値を算出
        float grey = (color.r + color.g + color.b) * 0.333;
        // 反映
        COLOR = vec4(grey, grey, grey, color.a);
    }
    else {
        // 何もしない
        COLOR = texture(TEXTURE, UV);
    }
}
```

スクリプトから操作する場合
```gdscript
get_node("Sprite").material.set_shader_param("greyscale", true)
```
