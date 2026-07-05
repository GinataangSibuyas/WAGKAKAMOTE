# **🍄 Welcome to Shroom Raider, Player!**

Are you familiar with the game **Tomb Raider**, starring Lara Croft as the main character? 

If you do — congrats! 🎉 But that's not what this game is. (Sadly)

## 🗺️ **GAME OVERVIEW** 

**Shroom Raider** is a 2D grid-based console game written in Python. You may imagine a chessboard where each tile might be a path you can walk on, an obstacle that blocks you, or something you can interact with. 

The game is set in a forest, where you play as Laro Craft 🧑 — not to be confused with Lara Croft from Tomb Raider (though she’d be proud).

Your mission? **Collect all mushrooms 🍄** inside the forest and do not drown in waters 🟦 while interacting with objects like rocks 🪨 and obstacles like trees 🌲. 

Inside the forest, there are tools left for you to use. In case you get lost or stuck, we've got you:

- 🪓 Axe -  to chop down trees
- 🔥 Flamethower - to burn neighboring trees
- and, 🪨 Rock - to be pushed into the water for safe crossing. 
    
Do your best, player! Clear the mission and survive the Mushroom Raid — just don't fall into the water and end the mission badly. 

***Lara Croft raided tombs. You? You raid shrooms. Welcome to the forest, Raider! ✨*** 

## 🕹️ **USER MANUAL** 

Shroom Raider is an app that reads map files (`.txt`) and displays the map using Unicode icons. There are maps made in this folder that you can play with on different levels of difficulty — for more challenges 😉 . 

The key to move around the map is using `W`, `A`, `S`, `D` as input for sequence of moves. The game will execute the player's desired move one `Enter` key is pressed.

The game reads the input characters as the key to move. Writing `W` will move the player up. `WWSAD`, also a valid set of moves, can create a sequence of moves for the player. 

### How to run the Shroom Raider?

📚 **Requirements to run the game**
- Python 3.6+ (recommended 3.8+)
- Terminal that supports Unicode and ANSI color codes (Windows PowerShell/Terminal or Linux/Mac terminal)

🖥️**Launching the Game using Terminal**
- To run the game with the default map (automatically starts at the first level `stage1.txt`):

``` bash
python3 shroom_raider.py
```
or 

``` bash
python3 -m shroom_raider.py
```

- But if you'd like to challenge yourself and jump into a different map with increased difficulty, you may run the game using:

```bash
python3 shroom_raider.py -f <stage_file>
```
    
The <stage_file> in `.txt` format will run the game using the specific file you chose. 
> ***Take note: The game <shroom_raider.py> and the <stage_file> must be in the same directory.***

```bash
python3 shroom_raider.py -f <stage_file> -m <sequence_of_moves> -o <output_file>
```
Here, before you can even launch the game, you can already input the <sequence_of_moves> you want to make.
Also, the <output_file> creates a new file in `.txt` format that shows the current position of obejcts and the player after the move was called out. 

## **⚙️ CODE DESCRIPTION**

This refactored version of the program's reorganized into a class-based architecture. Each section has a role to fulfill making the code readable and modular and easy to debug and modify. The following sections with the included game functions and dictionaries are listed below:

🧩 Code Structure
- Global constant and Enums (Module-Level)
    - UI_Map
    - LEADERBOARD_FILE
    - DIRECTION_VECTORS
- Data Models
    - Scenario Datclass
    - GameState Dataclass
- Core Game Logic
    - ShroomRaiderGame Class
       - 💯 clear_screen()
       - ⏭️ load_scenario()
       - 📍 display_map()
       - 🗃️ parse_map_file()
       - 🔃 load_all_scenarios_from_files()
       - 🏃 move_player()
       - ➡️ movements
       - 🏁 check_win()
       - 🎉 mission_success()
       - 💀 mission_failed()
       - 🏆 leaderboard
       - 🎮 play_game()
- Helper Functions - Internal Helper Methods
    - 🔥 burn_trees()
    - ⏹️ is_border()
- Main execution
    - 🎯 main()
  
> This arrangement does not dictate the code’s overall layout or logic.

## **💾 STEP-BY-STEP ALGORITHM OVERVIEW**

1. Program Start:
The program begins with the help of main(), as it parses command-line arguments given by the user. 

2. Map Loading:
`main()` calls the map file parses function: `load_all_scenarios_from_file(scenario)` to search `.txt` map file inside the directory. Then, parse_map_file(filepath) reads each of the map files found, and converts the text content to a parsed scenario as a dictionary. 

```.txt map file
9 9
TTTTTTTTT
T...+...T
T...~...T
T...R.T.T
T.T.LTT.T
T.x.....T
T.......T
T.......T
TTTTTTTTT
```

> The `.txt` file in the directory must have the **number of rows** and **number or columns** in the first line and the map in its corresponding ASCII representation in the following lines. 

3. Game State Initialization:
The parsed scenarios will then be processed by `load_scenario(scenario)` to update the **global game state** dictionary. After that, `display_map()` will render and load the game using the updated information.


4. Game Execution
The game loop function `play_game(scenario, is_cli=True)` starts. 

To process all the player moves, it calls `move_player(direction)`, which updates the player coordinates and game status depending on the tile the player will land on.

It also executes special action such as burning trees through `burn_trees(r, c)`,

For every move, it also calls is_border(r, c) to avoid the player from moving outside the map. 

Additionally, `clear_screen()` refreshes the display after each execution for a smoother gameplay. 

5. Game Condition:
After every move,  `check_win()` checks if all mushrooms have been collected.  If the player succeeded, `mission_success()` is called, and if not `mission_failed()` is called instead. 

## **🌲 FUNCTION IMPLEMENTATION**

For better understanding, the function and implementation of every dictionary and function is explained below. 

**🌍 Global constants**
These constants are used by different functions in the program. They are separated and organized to make the code easier to read and modify. 
- **UI_MAP** is a dictionary that contains keys of ASCII and values of their corresponding UI characters. This makes it easy for a function to convert the ASCII symbol to UI characters to be shown on the terminal.
- **LEADERBOARD_FILE** is a file used to store persistent scores
- **Direcion (Enum)** is a class that utilizes enum and contains fields of a string of movement and values that maps movement to row and column changes. 
    
| Key | Action     |
|-----|------------|
| **W** | Move Up    |
| **S** | Move Down  |
| **A** | Move Left  |
| **D** | Move Right |

**🚉 Data Models (Dataclasses)** 

To keep the code simple and easy to use,dataclasses are used to represent structured data with clearly defined fields which allow every function to manipulate and update the game state consistently

- **‘Scenario’** stores level information including the map layout, dimensions and starting points. 

  
- **‘GameState’** is a dataclass that serves as a source of information for the program. It contains the following fields:

- `‘map’`: 2D list of characters(each character is a tile code)

- `‘row’, ‘cols’`: an integer indicating the map’s dimension

- `‘player_r’, ‘player_c’`: an integer indicating player’s current row, column location 

- `‘player_item’` : either ‘None’, ‘x’ (axe), or `‘*’` (flamethrower) — states the item that the player picked up

- `‘under_player’` :  tile that was under the player (so moving away restore it)

- `‘is over’`: boolean if game ended

- `‘message’`: status line shown to the player

-`‘mushrooms_collected’`: an integer indicating number of mushrooms collected by the player

- `‘total_mushrooms’`: an integer indicating number of mushrooms in the game

- `‘Item_left_pos’`: ‘None’, ‘x’ (axe), or ‘*’ (flamethrower) — states the items left in the map

**✨ Core Game Logic** 
The ShroomRaiderGame claass acts as the central controller of the application. It contains the following methods:

These functions help in the display of the game. 
- `clear_screen()` clears the terminal after the execution of a string of move/s.
- `load_scenario()` loads a scenario dictionary into the global game state and places the player on the map.
- `display_map()` clears screen and renders the current state. It displays the interface of the game.


These functions read ‘.txt’ file with the help of python built in function “argparse”
- `parse_map_file(filepath)` reads a `.txt` file, trims empty line, reads the first line that indicates the number of rows and columns in the map, reads the map, return a `scenario` dictionary with `r, c`, `map_data`, `star_pos`, and `name`.
- `load_all_scenarios_from_files()` scans the current folder for `.txt` files and returns parsed scenarios.

These are the functions that execute the string of move/s of the player. 
**Movements** 
- `move_player(direction)` is the heart of the movement and interactions in the game as it processes a single player movement/s. This function performs the following steps:
1. Compute destination coordinates.
2. Check boundaries
3. Get the ‘target’ tile or the new tile where the character will be placed.
4. **Depending on ‘target’:**
- ‘~’ (🟦 water) - calls ‘mission_failed()’ and terminate the program
- `T` (🌲 tree) - three situations are possible to happen, either:
    - if player holds an axe `x` they can chop the tree to free their way, 
    - if holding flamethrower `*` they burn the surrounding trees via `burn_trees()`.
    - Otherwise, the tree blocks them.
In the border - display a message that states “Cannot move into the border tile!”. Unless the player has an equipped item. 
- `R` or `-` (🪨rock) -  push the rock by one tile in the same direction the player moves. Two possible scenarios may occur:
    - If rock is pushed into water `~`, it becomes sink tile `⬜`.
    - If rock is pushed into tree ‘T’ and towards the border, the rock and the player won’t move. 
- `+` (🍄 mushroom) - updates the number in the mushroom_collected in the  global state dictionary and becomes `.`.
- `x` or `*` - the player can stand on these tiles. These tiles are items for *pickup*. If the player already holds an item, they cannot pick another. If not holding an item, the code prompts to pick [`P`] or skip 

> this condition can be seen in play_game(scenario, is_cli=True)

Otherwise (‘.’): the move is normal, and it updates `under_player` and `map`.

For every move, `check_win()` is called.
**Game conditions**
- `check _win()` checks if all mushrooms have been collected.
- `mission_success()` calls clear_screen() and displays a message that indicates that the mission is successfully completed.
- `mission_failed()` sets  game[‘is_over’] to True, calls display(map), displays a message that indicates that the mission is failed.

**🏆leaderboard**
The update_leaderboard function records the player's progress in the game. It also stores and keeps other entries with different names of thos who plays the game and saves their progress. 
- All the leaderboard data is stored in the LEADERBOARD_FILE, `leaderboard.txt`.
**Game Loop**

This processes the moves entered by the player in the command line.
- `play_game()` contains the input loop. It facilitates the following game features:
    - **Chain of movements**, for example: WWDA - moving up twice, then moving left, then moving right
    - **‘P’** - picking up items. If the player already holds an item, they cannot pick another. If not holding an item, the code prompts to pick (`P`) or skip.
    - **‘!’** - calls load_scenario(scenario) to restart the game
    - **‘Q’** - updates game is_over to True and terminates the game.

**Helper Functions**
- `is_border()` checks if a coordinate is on the map border.
- `burn_trees(r, c)` performs a wildfire from the starting tree tile to every tree tile in its area, converting these connected trees to ‘.’. After burning, the flamethrower held by the player is consumed and the player can now pick up another item.
  
**Main Execution**

This serves as a bridge between the command line in the terminal to the functions in the program. 
- `main()` uses *‘argparse’* to handle optional arguments to run the game. These optional arguments are for a **map file `(-f)`**, a **sequence of moves `(-m)`**, and **output file `(-o)`**.
  
Conditions are made depending on the optional arguments given. 
There's a condition:
- if map file is only given; if map file and sequence of moves are given; 
- if map file, sequence of moves, and output file are given. They either load the map, execute the sequence of movements to the given map, and optionally write the final game state to an output file; and
- if none of these are given, a default stage file `stage1.txt` will be used. 
    
Moreover, it also checks whether the map file exists or failed to load, if so, it displays a message regarding the error and exits the program.

## **🧩 UNIT TESTING**

***Pytest*** is utilized for unit testing. 

Each unit test is made to verify the correctness of the results of each function in accordance with the game logic. These unit tests aim to check the correctness of the function responsible for map file parsing, scenario loading, edge cases, and core game logic. 

This is done by importing the base file functions in the test file in which the file name is written as `test_<base_file_name>.py`. And so, the functions are named in the same format.

The following functions are tested:

**Map File Parser and Scenario Loading:**

To test map file parsers and Load Scenario, **tmp_path** fixture is used. It is a built-in function in pytest, which creates a unique directory for a specific test function. This is helpful as parser functions search the directory for the ‘.txt’ file. 

- `test_parse_map_file()` asserts if the function returns the correct details in connection with the information written inside the file. 
    
- `test_load_scenario()` asserts if the global state game dictionary is updated correctly based on the information obtained from parse_map_file.

To test the game core logic function and edge cases, a temporary map is created using a helper function. 

**Edge cases:**

- `test_is_border` utilizes nested loops to assert if the given coordinate is in the border of the map.

**Core game logic:**
- `test_move_player` asserts `move_player`’s result for each player’s move on one coordinate to another. It checks if the functions return the right result in conditions such as: invalid moves, game over, and movements in borders. 
- `test_move_player_into_water` utilizes **monkeypatching** function that replicates `mission_failed()` to cancel the sys.exit() so it does not interpret the test. This is done to assert that falling into water sets the correct game-over state.
- `test_move_player_tree_with_axe` and `test_move_player_tree_with_flamethrower` asserts if `move_player` returns correct outcome when the player is interacting with the tree while holding an axe or flamethrower. 
- `test_move_player_trees_are_blocked` and `test_move_player_border_trees`  check if a tree properly blocks a player that does not hold a tool when it tries to move towards the direction of a tree.
- `test_move_player_rocks` asserts `move_player`’s result when the player tries to move towards the direction of a rock, and the rock pushes to the same direction, either into the water or land. 
- `test_move_player_normal` asserts `move_player`’s condition when collecting mushrooms. It checks if the function properly detects if all the mushrooms are already collected. 
- `test_burn_trees asserts` if the flamethrower used by the player returns the proper results. It ensures that burning spread correctly. 
- `test_check_win` and `test_mission_success` asserts if the program terminates the program and returns a message saying all mushrooms are collected.
- `test_mission_failed` utilizes **monkeypatching** function that replicates `mission_failed()` to cancel the `sys.exit()`, to assert if it terminates the program when the player fell into the water.

### **🤔 HOW TO RUN PYTEST**

To run ***pytest*** on your terminal, ensure the base file and the test file are in the same directory. After that, you run the command:

```
pytest
```
*In this case, you can run this as is since there's only one base file and one test file in the provided directory.*

You are to expect this result if the ***pytest*** is successful. ✅

```
platform linux -- Python 3.12.3, pytest-7.4.4, pluggy-1.4.0
rootdir: /home/pc_name_/project-1-shroom-raider-team-ang/shroom_raider
collected 15 items

test_shroom_raider py ...............                                                                 [100%]

============================================= 15 passed in 0.08s ============================================
```

The test cases verify the updates made by the  functions involve in map parsing, edge case, and game core logic. 

The unit testings done is reasonably thorough as it cover these major functions. Moreover, for cases like map parsing and scenario loading, it utilizes temporary directory to get proper assertions. It also uses monkeypatch for functions that terminates (such as `sys.exit()`) the program, to ensure that proper updates happened before the termination of the program. 

### **🤔 HOW ADD MORE TESTS**

Since the test file used a `tmp_path` fixture, the easiest way to modify test cases is to edit the `temp_file(rows, cols)` function.

```
def temp_file(rows=9, cols=9):
    """ Used as a "temporary" map <stage_file>"""
    map_data = [
        "TTTTTTTTT",
        "T...+...T",
        "T...~...T",
        "T...R.T.T",
        "T.T.LTT.T",
        "T.x...*.T",
        "T.......T",
        "T.......T",
        "TTTTTTTTT",
    ]
    return {
        "r": rows,
        "c": cols,
        "map_data": map_data,
        "start_pos": (4, 4),
        "name": "fake_stage"
    }
```

Here, you can simply write the map inside the `<stage_file>` you want to use in testing. However, you must ensure that every column is a joint string with the rows contained in a sequence, which in this case, is a list. 🌟

# **💝 LEVEL DESIGN CONTEST - BONUS POINTS**

### 1. Maze 
### 1. Pond

