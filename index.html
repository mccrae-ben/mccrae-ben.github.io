# Knock It Over - Mods and Point Scoring System   
## **Game Context**   
- In Knock It Over the player selects from 3 randomly generated shapes with physical properties (box, cyclinder etc…) and throws it at stacks of objects in a room. The goal of the game is to knock as many room objects over as possible to score enough points to move onto the next room.   
- Each throwable object has modifications that alter gameplay.    
    - ex: Throwing this object adds an additional throw   
    - ex: Having this throwable object in the room decrease gravity by X%   
    - ex: Any room object of type X knocked over with this object adds 10 points to your overall score   
- Each room object has base points and a base multiplier   
- **Goal: Create a modification system for the Throwable Objects flexible enough to impact a lot of different aspects like:**   
    - Scoring system (add base score, increase multipliers etc..)   
    - Room physics (change the gravity of the room, make surfaces more or less absorbant etc…)   
    - Permanent or semi-permanent upgrades (number of throws, weight of objects etc…)   
   
   
## **Issues I'm Running Into**   
### Managing the instances of the Throwable:   
- Keeping the instanced Throwable requires logic to know if it was instanced or not, where to keep it in the Hand etc…   
- Deleting the instance causes issues at the end of the round when I want to know if it had a modification that factors into scoring. But deleting the instance means the function   
- Handling different mods stacks:   
    - If it's a 1 time permanent upgrade, who owns that that upgrade has been applied and should be ignored in the future.    
- Proper timing for Applying mod:   
    - Not sure the Round States that anybody can update feels very secure but not sure of a better way to handle that   
    - Who should own checking that the mod *should*  be applied. Meaning should the mod itself check that we're in the right state to process this mod   
- Matching variables across classes that need to be in sync:   
    - ex: a mod might update the number of throws available to the player but the ThrowableHand doesn't know that   
   
   
## Code and Approach   
- A GameManager autoload that tracks:   
    - Round States (STARTED, THROWABLE\_THROWN, FALLEN\_OBJECTS\_PROCESSED, FINAL\_SCORE CALCULATED)   
        - Any class can update the Round States via change\_round\_state() function   
    - Room and object generation   
    - A score calculator   
    - Data relevant to the Round:   
        - Throws left   
        - Array of Objects Thrown this round    
   
    ```
extends Node

enum ROUND_STATES {NOT_SET, ROUND_STARTED, THROWABLE_PICKED, THROWABLE_THROWN, ROOM_OBJECTS_PROCESSED, ROUND_SCORE_CALCULATED, ROUND_CLEARED, ROUND_FAILED}

#SCORING VARIABLES
var captured_room_objects: Array[RoomObject]
var current_total_score: float = 0.0
var current_mults: float = 0.0
var total_throwable_flipped_base_score: int = 0
var total_throws: int = 3:
	set(value):
		total_throws = value
		throws_left_changed.emit(total_throws - current_throw)
var current_throw: int = 0:
	set(value):
		current_throw = value
		if current_throw < total_throws:
			throws_left_changed.emit(total_throws - current_throw)
		else:
			throws_left_changed.emit(0)
			calculate_score()
var current_throwable: ThrowableObject
var throwables_played: Array[ThrowableData]
var throwables_player_go: Array[ThrowableObject]

#ROOM VARIABLES
var current_room: RoomGameObject
var rooms_generated: float = -1
var current_round_state: ROUND_STATES: 
	set(value):
		current_round_state = value
		round_state_changed.emit(value)

signal round_state_changed(current_round_state: ROUND_STATES)

# Called when the node enters the scene tree for the first time.
func _ready() -> void:
	current_round_state = ROUND_STATES.ROUND_STARTED
	Global.room_cleared.connect(_on_room_cleared)	
	generate_room()

#SCORE CALCULATION - CALLED WHEN ALL THROWS ARE USED UP
func calculate_score():
	await get_tree().create_timer(2).timeout #wait for any final objects to be flipped
	GameManager.set_round_state(GameManager.ROUND_STATES.ROOM_OBJECTS_PROCESSED)

	for throwable: ThrowableObject in throwables_player_go:
		throwable.apply_throwable_mod(GameManager.ROUND_STATES.ROOM_OBJECTS_PROCESSED)

	for room_object in captured_room_objects:
		print("did the score do anything", room_object.score)
		#CALCULATE MULTS
		current_mults += room_object.room_object_data.base_mult

		var previous_room_object_score: int = room_object.score
		total_throwable_flipped_base_score += (room_object.score - previous_room_object_score) #This is terrible but avoids double counting since the total score already captured the base score before any mods are applied
		print("throwable score", room_object.score)

	current_total_score = total_throwable_flipped_base_score * current_mults
	final_score_calculated.emit(current_total_score)
	GameManager.set_round_state(GameManager.ROUND_STATES.ROUND_SCORE_CALCULATED)
	
	if current_total_score >= current_room.score_needed_to_clear_room:
		current_room.cleared = true
		GameManager.set_round_state(GameManager.ROUND_STATES.ROUND_CLEARED)
		room_cleared.emit()
	else:
		GameManager.set_round_state(GameManager.ROUND_STATES.ROUND_FAILED)

func get_current_score_required():
	if GameManager.rooms_generated == -1:
		return score_required_progression[int(0)]

	return score_required_progression[int(GameManager.rooms_generated)]

func generate_room():
	var room_data: RoomData = load("uid://coou2f25lykpb")
	var room_instance: RoomGameObject = RoomGameObject.create_instance(room_data)
	add_child(room_instance)
	rooms_generated += 1
	var updated_z_position: float = 30 * rooms_generated
	room_instance.global_position = Vector3(0,0,updated_z_position)

func generate_player():
	pass

func set_round_state(round_state: ROUND_STATES):
	current_round_state = round_state

func _on_room_cleared():
	generate_room()

```
- A Player:   
    - All the logic for throwing the Throwable   
    - A ThrowableHand component that tracks which throwables were generated, are equipped etc…   
        ```
extends Node3D
class_name ThrowableHand

var total_throwable_slots: int = 3
var current_throwable_slot_index: int = -1
var held_throwable: ThrowableObject
	# set(value):
	# 	held_throwable = value
	# 	if value:
	# 		GameManager.set_round_state(GameManager.ROUND_STATES.THROWABLE_PICKED)
var all_throwables: Array[String]
var throwable_slots: Array[ThrowableObject]


func _setup():
	GameManager.round_state_changed.connect(_on_round_state_changed)
	held_throwable = null
	for throwable in all_throwables:
		var throwable_instance = ThrowableObject.create_instance(load(throwable))
		throwable_slots.append(throwable_instance)

func swap_held_throwable(index: int):
	if index > throwable_slots.size()-1 or current_throwable_slot_index == index or throwable_slots[index] == null: #if we already have the equipped throwable selected, we've already played that slot or the request doesn't match the size, ignore it
		return
	
	if held_throwable:
		held_throwable.queue_free()
	
	current_throwable_slot_index = index
	held_throwable = throwable_slots[index]
	add_child(held_throwable)

func go_to_next_throwable():
	swap_held_throwable(current_throwable_slot_index + 1)
	
#Clear the throwable if the round has ended
func _on_round_state_changed(current_round_state: GameManager.ROUND_STATES):
	if current_round_state == GameManager.ROUND_STATES.ROUND_SCORE_CALCULATED:
		if held_throwable:
			held_throwable.queue_free()

```
- A Throwable Class   
    - Each throwable object has a Custom Resource that defines properties about the object including a custom resource for a Mod   
   
    ```
extends RigidBody3D
class_name ThrowableObject

@export var throwable_throw_force = 10
@export var followSpeed = 2
@export var followDistance = 1
@export var throwable_data: ThrowableData
@export var mesh_instance: MeshInstance3D

var current_level: int = 1
var mouse_position
var viewport
var object_held: bool = false
var objectPos
var targetPos
var targetRot
var startingPos
var parent
var thrown: bool = false:
	set(value):
		if value:
			if !Global.throwables_played.has(self.throwable_data):
				Global.throwables_played.append(throwable_data)
				var dupe_throwable_object: ThrowableObject = self.duplicate()
				Global.throwables_player_go.append(dupe_throwable_object)

func _ready() -> void:
	freeze = true
	viewport = get_viewport()
	object_held = true
	parent = get_parent()
	startingPos = Global.player_instance.CAMERA_CONTROLLER.global_transform

static func create_instance(def: ThrowableData) -> ThrowableObject:
	var instance: ThrowableObject = load("uid://b3323vwssxuio").instantiate()
	instance._setup(def)
	return instance 

func _setup(def: ThrowableData):
	throwable_data = def
	mass = def.get_throwable_mass(float(current_level))
	var material: Material = mesh_instance.get_active_material(0)
	if material:
		var updated_material: Material = material
		updated_material.albedo_color = def.color
		mesh_instance.set_surface_override_material(0,updated_material)

	apply_throwable_mod(GameManager.current_round_state)
	GameManager.round_state_changed.connect(apply_throwable_mod)

func _physics_process(delta: float) -> void:
	if object_held:
		hold_object()

func hold_object():
	targetPos = Global.player_instance.CAMERA_CONTROLLER.global_transform
	parent.global_transform = parent.global_transform.interpolate_with(targetPos.translated_local(Vector3(0,0,-3)), .3) # Held object position 
	parent.rotation.x = 0

func reset_throwable():
	parent.global_transform = startingPos
	self.global_transform = startingPos
	freeze = true
	object_held = true

func release_object():
	object_held = false
	freeze = false
	GameManager.set_round_state(GameManager.ROUND_STATES.THROWABLE_THROWN)

func disable_throwable():
	object_held = false

func apply_throwable_mod(_current_round_state: GameManager.ROUND_STATES):
	if throwable_data.throwable_mod_data.mod_script:
		throwable_data.throwable_mod_data.mod_script.apply_throwable_mod(throwable_data)

```
- A Room\_Object Class:   
    - Holds a RoomDefinition custom resource with base\_score and base\_mults values   
    - Tracks the state of the object   
    - Tells the GameManager that it's been flipped and should be counted   
   
   
