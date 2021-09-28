# Exercise-03c-Music-and-Sound

Exercise for MSCH-C220, 28 September 2021

A demonstration of this exercise is available at [https://youtu.be/VR7JY77brQo](https://youtu.be/VR7JY77brQo)

This exercise will add sound effects, music, and a few more juicy features to our match-3 game. The exercise will provide you with several features that should help with the implementation of Project 03.

Fork this repository. When that process has completed, make sure that the top of the repository reads [your username]/Exercise-03c-Music-and-Sound. Edit the LICENSE and replace BL-MSCH-C220-F21 with your full name. Commit your changes.

Press the green "Code" button and select "Open in GitHub Desktop". Allow the browser to open (or install) GitHub Desktop. Once GitHub Desktop has loaded, you should see a window labeled "Clone a Repository" asking you for a Local Path on your computer where the project should be copied. Choose a location; make sure the Local Path ends with "Exercise-03c-Music-and-Sound" and then press the "Clone" button. GitHub Desktop will now download a copy of the repository to the location you indicated.

Open Godot. In the Project Manager, tap the "Import" button. Tap "Browse" and navigate to the repository folder. Select the project.godot file and tap "Open".

If you run the project, you will see a main menu followed by a simple match-3 game. Your task will be to add music, sound effects, and a coin-collecting animation. You will also need to add a background image and update the fonts for any text.

---

Open res://UI/Menu.tscn. Add a new Sprite child node to Menu, and make it the top child (appearing above Background). The texture for the sprite should be res://Assets/background1.png. Turn off Sprite->Offset->Centered. Update its position.x = -130

Add a Custom Font to the Label node (res://Assets/FFF_NEPSZA-BADSAG-Bold.otf, size=36). Add a Custom Font to the Play and Quit buttons (res://Assets/Ignotum.otf, size=20)

Open res://Game.tscn. Add res://Assets/background2.png as the background for this scene. Update the /Game/HUD/Score label to use res://Assets/Ignotum.otf, size=18

The next part of the assignment is to record three (short) sound effect in Audacity. Save them as 1.wav, 2.wav, and 3.wav, and copy them into the Assets folder for the project.

You will then need to write a simple melody in MuseScore. It doesn't have to be pretty or elaborate. Export the music as an Ogg Vorbis file (music.ogg) and copy it into the Assets folder for the project.

Back in Godot, add four AudioStreamPlayer nodes to Game. Name them Music, 1, 2, and 3. 

Add res://Assets/music.ogg as the Stream for the Music node. Set Autoplay=On. Attach an empty script to the Music node and save it as res://UI/Music.gd. Add a finished() Signal to the node, and the resulting function should appear as follows:
```
func _on_Music_finished():
	play()
```

I will leave it as an exercise for you to figure out how and when to play the sound effects. Remember, you will need to find the nodes from within a script (most likely in res://Pieces/Piece.gd). To play the sound, you just need to call the particular node's play() method.

Finally, we will set up an update-score animation. Create a new 2D Scene. Change the name of Node2D to Coin. Add two Sprite nodes (name the second one Highlight) and a Tween node. Also, add a Timer node, set it to autostart and set its timeout for 2.5. The Texture for the Sprite node should be res://Assets/coin.png. The Highlight node should have res://Assets/coin_highlight.png. Attach a script to the Coin node (save it as res://Coin/Coin.gd).

We want the coin to scale from (0,0) to (1,1) in 0.25 seconds, and then to do the following:
 * Animate the global_position from its current global_position to (20,15) over 0.75 seconds
 * Animate the scale from (1,1) to (0.2,0.2) over 0.5 seconds
 * Animate the modulate.a from 1.0 to 0.0 over 2.0 seconds

(remember to start() the $Tween node after each interpolate_property call). All of that can be done in the _ready() function.

The rest of the script can look like this:
```
extends Node2D

var c = 0

func _ready():
	# animate the behavior
	pass

func _physics_process(_delta):
	$Highlight.modulate.a = (sin(c)/2)+0.5
	c += 0.5
```
Select the Timer node and add a timeout() signal. Attach it to the Coin.gd script; the only job of the resulting method is to queue_free()


When you are done, save the scene as res://Coin/Coin.tscn.

In res://Pieces/Piece.gd, add this above the `_ready()` function:
```
var Coin = preload("res://Coin/Coin.tscn")
```

Then append this to the die() function:
```
	if Effects == null:
		Effects = get_node_or_null("/root/Game/Effects")
	if Effects != null:
		var coin = Coin.instance()
		coin.position = target_position
		Effects.add_child(coin)
```

Save the script and the scene.

---

Test the game and make sure it is working correctly. You should be able to see all (and hear!) the changes you have made.

Quit Godot. In GitHub desktop, you should now see the updated files listed in the left panel. In the bottom of that panel, type a Summary message (something like "Completes the exercise") and press the "Commit to master" button. On the right side of the top, black panel, you should see a button labeled "Push origin". Press that now.

If you return to and refresh your GitHub repository page, you should now see your updated files with the time when they were changed.

Now edit the README.md file. When you have finished editing, commit your changes, and then turn in the URL of the main repository page (https://github.com/[username]/Exercise-03c-Music-and-Sound) on Canvas.

The final state of the file should be as follows (replacing my information with yours):
```
# Exercise-03c-Music-and-Sound

Exercise for MSCH-C220, 28 September 2021

Adding music, sound, typeface changes, and several "juicy" features to a simple match-3 game.

## To play

Select and drag tiles using the mouse.


## Implementation

Built using Godot 3.3.3

## References
 * [Juice it or lose it — a talk by Martin Jonasson & Petri Purho](https://www.youtube.com/watch?v=Fy0aCDmgnxg)
 * The match-3 game is derived from code provided by [MisterTaftCreates](https://github.com/mistertaftcreates/Godot_match_3), with an accompanying [YouTube series](https://www.youtube.com/playlist?list=PL4vbr3u7UKWqwQlvwvgNcgDL1p_3hcNn2)
 * [Letter Tiles](https://kenney.nl/assets/letter-tiles) and [background images](https://www.kenney.nl/assets/background-elements-redux) provided by kenney.nl.
 * [Puzzle Pack 2, provided by kenney.nl](https://kenney.nl/assets/puzzle-pack-2)
 * Explosion spritesheet provided by [Ville Seppanen](https://opengameart.org/content/explosion-animated)
 * [Ignotum Typeface](https://fontesk.com/ignotum-font/)
 * [Nepszabadag Typeface](https://fontesk.com/nepszabadsag-font/)
 * Screen Shake script provided by [KidsCanCode](https://kidscancode.org/godot_recipes/2d/screen_shake/)
 

## Future Development

Shaders

## Created by 

Jason Francis
```
