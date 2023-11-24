extends Button

var roll_results: Array

# Define textures for different rarity types
var common_textures: Array
var rare_textures: Array
var very_rare_textures: Array
var ultra_rare_textures: Array

const COMMON_PROBABILITY = 0.5
const RARE_PROBABILITY = 0.8
const VERY_RARE_PROBABILITY = 0.95

func _ready():
	# Find and store TextureRect elements
	roll_results = [$RollResult1, $RollResult2, $RollResult3]

	# Define items and their corresponding probabilities
	common_textures = load_textures(["res://Item/Item - 01 (Front).jpg", "res://Item/Item - 02 (Front).jpg", 
		"res://Item/Item - 03 (Front).jpg", "res://Item/Item - 07 (Front).jpg", "res://Item/Item - 12 (Front).jpg"],
		[0.2, 0.2, 0.2, 0.2, 0.2])  # Adjust probabilities accordingly

	rare_textures = load_textures(["res://Skill/Skill - 01 (Front).jpg", "res://Skill/Skill - 02 (Front).jpg", 
		"res://Skill/Skill - 03 (Front).jpg", "res://Skill/Skill - 04 (Front).jpg", "res://Skill/Skill - 05 (Front).jpg"],
		[0.1, 0.1, 0.2, 0.3, 0.3])  # Adjust probabilities accordingly

	very_rare_textures = load_textures(["res://Weapon/Weapon - 01 (Front).jpg", "res://Weapon/Weapon - 02 (Front).jpg", 
		"res://Weapon/Weapon - 03 (Front).jpg", "res://Weapon/Weapon - 04 (Front).jpg", "res://Weapon/Weapon - 05 (Front).jpg"],
		[0.2, 0.2, 0.2, 0.1, 0.3])  # Adjust probabilities accordingly

	ultra_rare_textures = load_textures(["res://Encounter/Encounter - 01 (Front).jpg"],
		[1.0])  # Adjust probability accordingly

func _process(_delta):
	pass

func _on_pressed():
	print("Button Pressed!")

	# Display the results on TextureRect elements
	for i in range(3):
		var random_rarity_number = randf()  # Generate a random number between 0 and 1 for rarity

		# Determine the rarity based on the random number
		var rarity = match_random_number_to_rarity(random_rarity_number)

		# Select a random item from the appropriate list based on rarity
		var selected_texture = match_rarity_to_texture(rarity)

		# Set the TextureRect texture
		roll_results[i].texture = selected_texture

# Function to match a random number to a rarity type
func load_textures(file_paths: Array, probabilities: Array) -> Array:
	var textures: Array = []
	for i in range(file_paths.size()):
		var texture = ResourceLoader.load(file_paths[i])
		for j in range(int(probabilities[i] * 100)):  # Adjust the factor for more precision
			textures.append(texture)
	return textures

func match_random_number_to_rarity(random_number: float) -> String:
	if random_number < COMMON_PROBABILITY:
		return "Common"
	elif random_number < RARE_PROBABILITY:
		return "Rare"
	elif random_number < VERY_RARE_PROBABILITY:
		return "Very Rare"
	else:
		return "Ultra Rare"

func match_rarity_to_texture(rarity: String) -> Texture:
	var selected_textures: Array
	if rarity == "Common":
		selected_textures = common_textures
	elif rarity == "Rare":
		selected_textures = rare_textures
	elif rarity == "Very Rare":
		selected_textures = very_rare_textures
	else:
		selected_textures = ultra_rare_textures

	var random_texture_index = randi() % selected_textures.size()
	return selected_textures[random_texture_index]
