# Text Adventure World

A two-player cooperative console adventure game written in **C++** — players move through rooms, collect keys, solve riddles, dodge bombs and use springs and teleporters to reach the final room together.

Built as the team project of the **Object Oriented Programming and C++** course at the Academic College of Tel Aviv-Yaffo (MTA), by **Elior Chen** and **Ori Shitrit** (course grade: 94).

> Moved here from the original course repository, where we developed it together using branches and pull requests.

## Features

- **Two players on one keyboard**, each with their own controls, lives and score
- **Rooms loaded from text files** — every room is a `.screen` file with a map grid plus a data section (doors, keys, switches, dark areas, teleporters)
- **Game elements:** doors that need keys or switches, bombs, heavy obstacles that need enough force to push, springs that launch players, riddles, dark rooms with a torch, teleporters
- **Record & replay:** save a play session with `-save` and replay it with `-load` (or verify it headlessly with `-load -silent`)
- **Cross-platform:** conditional compilation for Windows and macOS/Linux terminal input and output

## Build & run

```bash
g++ -std=c++17 src/*.cpp -o game
./game                 # play
./game -save           # play and record the session
./game -load           # replay a recorded session
./game -load -silent   # replay without drawing and check the results
```

Or with CMake:

```bash
cmake -S . -B build && cmake --build build
./build/game
```

Run the game from the repository root so it can find the room files (`adv-world_*.screen`) and `riddles.txt`.

## Controls

| Action | Player 1 | Player 2 |
|---|---|---|
| Move up / left / down / right | `W` `A` `X` `D` | `I` `J` `M` `L` |
| Stay / dispose item | `S` / `E` | `K` / `O` |
| Restart room | `R` | `R` |

**Scoring:** key 10 · door 20 · riddle 10 · finishing first 100 / second 50.

## Project structure

| Part | Files |
|---|---|
| Entry point and game modes | `Main.cpp`, `GameBase`, `KeyboardGame`, `FileGame` |
| Board and rendering | `Screen`, `Maps.h`, `BoardChars.h`, `Point` |
| Players and movement | `Player`, `Spring`, `Obstacle` |
| Game elements | `Door`, `Key`, `Switch`, `Bomb`, `Riddle`, `Torch`, `Collectible` |
| Record and replay | `Steps`, `Results` |

## Room file format

Each room file has a map grid followed by a data section:

```
DARK <x> <y> <x> <y>
DOOR <x> <y> DoorID <id> OPEN <0|1> KEYS <neededKeys> RULE <rule>
KEY <x> <y> DoorID <id>
SWITCH <x> <y> DoorID <id>
TELEPORT <x1> <y1> <x2> <y2>
```

Doors are drawn with the digits `2`–`9`, the index of the room they lead to. A door to the final room uses the number of rooms + 1.
