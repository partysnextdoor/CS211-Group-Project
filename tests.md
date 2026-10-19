# Test record

All "actual" results below were produced by running the programs. Movement results are shown **before** random insertion. Automated movement/save-load tests: `game2048/test_moves.cpp` (41 passed, 0 failed).

## Part A: 2048 movement (deterministic, before random insertion)
| # | Start / input | Expected | Actual | Result |
|---|---------------|----------|--------|--------|
| 1 | row `0 2 0 2 0`, left | `4 0 0 0 0`, +4 | same | PASS |
| 1 | row `2 2 4 0 0`, left | `4 4 0 0 0`, +4 | same | PASS |
| 1 | row `4 4 8 8 0`, left | `8 16 0 0 0`, +24 | same | PASS |
| 1 | row `2 0 0 4 2`, left (gaps) | `2 4 2 0 0`, +0 | same | PASS |
| 1 | row `2 2 2 0 0`, right | `0 0 0 2 4`, +4 | same | PASS |
| 1 | row `4 4 8 8 0`, right | `0 0 0 8 16`, +24 | same | PASS |
| 1 | row `2 0 4 0 0`, right (gaps) | `0 0 0 2 4`, +0 | same | PASS |
| 1 | col `2 2 2 0 4`, up | `4 2 4 0 0`, +4 | same | PASS |
| 1 | col `2 2 2 0 4`, down | `0 0 2 4 4`, +4 | same | PASS |
| 2 | `2 2 2 0 0` left (three equal) | `4 2 0 0 0`, +4 | same | PASS |
| 2 | `2 2 2 2 0` left (four equal) | `4 4 0 0 0`, +8 | same | PASS |
| 2 | `2 2 2 2 2` left | `4 4 2 0 0`, +8 | same | PASS |
| 2 | col `2 2 2 2 0` down | `0 0 0 4 4`, +8 | same | PASS |
| 3 | `0 0 0 0 2` left (slide, no merge) | `2 0 0 0 0`, +0, changed=true (game: moves+1, one new 2) | same; game loop adds one 2 and counts one move | PASS |
| 4 | `2 4 8 0 0` left; `0 0 2 4 8` right; tiles at top, up | changed=false, +0 (game: no tile, counters unchanged, message shown) | same | PASS |
| 5 | full board, one equal horizontal pair | not game over | not game over | PASS |
| 5 | full board, only vertical pair | not game over | not game over | PASS |
| 5 | full checkerboard-like board, no equal neighbors | game over | game over | PASS |
| 6 | `1024 1024 0 0 0` left | `2048 0 0 0 0`, +2048, highest=2048 | same (win message shown by main when highest rises to >=2048) | PASS |
| 6 | `4 4 8 8 0` left (several merges) | +24 total | +24 | PASS |
| - | new game | two tiles of 2, score 0, moves 0 | same | PASS |
| - | insert_random | exactly one new tile; none on full board | same | PASS |

## Save / load
| # | Test | Expected | Actual | Result |
|---|------|----------|--------|--------|
| 7 | save, change score/moves/board, load | all 25 cells + counters restored exactly | identical | PASS |
| 8 | missing file | error, state unchanged | "Could not open ..." state unchanged | PASS |
| 8 | empty file / wrong marker | rejected, unchanged | rejected | PASS |
| 8 | missing counters, negative, non-integer counter | rejected, unchanged | rejected | PASS |
| 8 | incomplete board (24 values) | rejected, unchanged | "File is incomplete..." | PASS |
| 8 | invalid tiles 1, 3, 6 | rejected, unchanged | "not 0 or a power of two" | PASS |
| 8 | non-numeric cell, extra content | rejected, unchanged | rejected | PASS |
| 8 | values beyond int range (cell, counter) | rejected, unchanged | rejected | PASS |
| 8 | valid `sample_save.txt` | loads score 8, moves 3 | loaded | PASS |
| - | save to unwritable path | accurate error | "Could not open ... for writing." | PASS |
| - | load does not insert a tile or count a move | counters as saved | confirmed in interactive run (Score 4 / Moves 1 before and after load) | PASS |

## Controls (interactive, piped input)
| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 9 | `h` | help text, state unchanged | shown, unchanged | PASS |
| 9 | `x`, `lr`, `z`, `y` at menu | "Invalid command", state unchanged | as expected | PASS |
| 9 | `n` then `n` | "New game cancelled", state kept | as expected | PASS |
| 9 | `n` then `y` | new game, score/moves 0 | as expected | PASS |
| 9 | `e` | "Goodbye!", exit | as expected | PASS |
| 9 | end of input (no `e`) | clean exit | "End of input. Exiting." | PASS |
| 9 | `s` | file written, message shown, state unchanged | as expected | PASS |

## Part B: Luhn
| Input | Expected | Actual | Result |
|-------|----------|--------|--------|
| `4532015112830366` | format OK; passes | passes (sum 50) | PASS |
| `4532015112830367` | format OK; fails | fails (sum 51) | PASS |
| `0000000000000018` | passes, leading zeros kept | passes (sum 10) | PASS |
| `453201511283036` | reject: 15 digits | "exactly 16 characters (got 15)" | PASS |
| `45320151128303666` | reject: 17 digits | "(got 17)" | PASS |
| `453201511283036X` | reject: non-digit | "only digits 0-9" | PASS |
| `4532 015112830366` (internal space) | reject | rejected (length 17) | PASS |
| empty line | reject | rejected (got 0) | PASS |
| several checks then `q` | correct results; clean exit | "Goodbye!" | PASS |
| end of input | clean exit | "End of input. Exiting." | PASS |

## Final clean-extraction build test
ZIP extracted into a new folder; both programs built with the README commands (`g++ -std=c++17 -Wall -Wextra -pedantic ...`) with no warnings and ran correctly (Linux, g++). **PASS**
