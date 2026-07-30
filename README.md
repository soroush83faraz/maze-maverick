# Maze Maverick

A terminal number-maze game in C++ (single file, `main.cpp`). Create mazes, let the built-in solver crack them, or play them yourself against the clock — with saved maps, per-player stats, game history, and a leaderboard.

## The puzzle

A maze is a grid of integers. You start at the top-left cell and must reach the bottom-right cell:

- `0` cells are walls and cannot be entered.
- The path must take an exact number of steps (for a *basic* maze, `width + height - 2`; for an *advanced* maze, a path length you choose).
- The values of the cells you visit must sum to the number written in the goal cell.

## Features

- **Create a maze**
  - *Basic*: enter the dimensions; the generator lays down a random right/down solution path of non-zero values, writes the path sum into the goal cell, adds a few random walls (never on the solution path), and fills the rest with random values.
  - *Advanced*: you also choose the path length (solution paths can wander in all four directions), the min/max number of walls, and the cell value range.
  - Every created maze is saved under `./maps/<name>/map.txt` and can be reused later.
- **Solve a maze** — a recursive depth-first solver that tracks both step count and running sum finds a valid path in a saved or freshly entered maze and prints the board with the solution highlighted in green.
- **Playground** — play a saved or new maze interactively:
  - Move with `w`/`a`/`s`/`d`, undo a step with `z`, quit with `q`.
  - A live timer runs in a background thread and the board redraws with your trail in blue.
  - On finishing you get a WON/LOST screen; on a loss your wrong steps are shown in red against the correct solution in green.
  - Results are recorded under a player name you enter, and you can retry immediately.
- **History** — the last 10 games (player, maze, duration, result, date) shown as a table.
- **Users** — per-player stats: games played, wins, last win date, total playtime.
- **Leaderboard** — top 3 players, ranked by wins with playtime as the tiebreaker.

Maps, user stats, and history live in `./maps/`, `./users/`, and `./history/`, created automatically next to the executable on first run.

## Build

Windows-only: the game uses `<conio.h>` (`getch`), `<windows.h>`, and `system("cls")`. It also needs C++17 (`<filesystem>`) and `std::thread`, so use a reasonably recent MinGW-w64 GCC (8 or newer):

```bash
g++ -std=c++17 -O2 -o maze main.cpp
```

(If you get thread-related link errors, add `-pthread`.)

## How to play

Run `maze.exe` and navigate the menus:

```text
Maze Maverick
1. create a maze
2. solve a maze
3. playground
4. history
5. users
6. leader board
7. exit
```

A typical loop: create a basic maze (option 1), then head to the playground (option 3), pick it from your saved maps, and race the timer:

```text
 w(Up)  s(Down)  d(Right)  a(Left)  z(Go Back)  q(Quit)
00:07
```

Win by reaching the bottom-right cell in exactly the required number of steps with your cells summing to the goal value.
