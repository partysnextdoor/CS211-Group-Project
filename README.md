# CS211 Group Project: 2048 + Luhn Validation

- **Session / group:** MW / Group 19  
- **Members (GitHub usernames):** Isaiah Woodford (@partysnextdoor), Issac Hedrich (@isaachedrich-prog)
- **Repository:** [https://github.com/OWNER/cs211-project-mw-group01](https://github.com/partysnextdoor/CS211-Group-Project) 

## Programs
**game2048/** - playable 5x5 console 2048 with scoring, win/game-over detection and a single-slot save/load.
**luhn/** - checks whether a 16-digit number passes the Luhn checksum. A pass does *not* mean a card exists or is authorized.

## Build (from the top-level project folder)
```
g++ -std=c++17 -Wall -Wextra -pedantic game2048/main.cpp game2048/2048_helper.cpp -o game2048_app
g++ -std=c++17 -Wall -Wextra -pedantic luhn/main.cpp luhn/luhn.cpp -o luhn_app
```
Run with `./game2048_app` and `./luhn_app` (on Windows/MinGW: `game2048_app.exe`).
Optional movement/save-load test driver:
```
g++ -std=c++17 -Wall -Wextra -pedantic game2048/test_moves.cpp game2048/2048_helper.cpp -o test_moves && ./test_moves
```

## 2048 controls
| Key | Action |
|-----|--------|
| u d l r | slide up / down / left / right |
| s | save to `savegame.txt` (one slot; saving again **replaces** it) |
| o | open/load `savegame.txt` |
| n | new game (asks y/n) |
| h | help |
| e | exit |

Rules: equal adjacent tiles merge once per move; merged value is added to score. A successful move (including a slide with no merge) counts as one move and adds one new 2 in a random empty cell. A move that changes nothing does nothing. Reaching 2048+ announces a win and play continues. Game over = no empty cells and no equal neighbors; then only s, o, n, h, e work.
The program shows board, score, move count and highest tile at startup and after moves, new game and successful load.

## Save file
`savegame.txt` is read/written in the **current working directory** (no absolute paths). Format: marker `CS211_2048_V1`, then `score moves`, then 5 rows of 5 integers (see `game2048/sample_save.txt`). Loading validates everything (marker, nonnegative counters, exactly 25 cells that are 0 or powers of two >= 2, no extra content, int range) into temporaries first; on any failure the current game is left unchanged and an error is shown. Loading restores the exact board/counters without inserting a tile or counting a move.

## Luhn usage
Enter a 16-digit number (leading zeros kept; input is read as a string). Malformed input (wrong length, non-digit, internal space) gets a format message and another attempt; well-formed numbers report pass/fail with the checksum total. Enter `q` to quit.

## Design notes
Movement functions return `MoveResult {changed, points}` and never insert tiles or touch counters, so they are testable deterministically; `main` handles insertion, score and move count. Each line is processed by one shared `slide_line` helper (right/down reverse the line first).

## Known limitations
- Single save slot; the random sequence is not restored on load.
- Score is stored as `int`.
- Console input is line-based (press Enter after each command).
