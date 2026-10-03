# Minesweeper (Android)

A native Android implementation of Minesweeper with a configurable board size,
mine density, and board color scheme.

## What it does

- Main menu → Settings screen → Game screen flow (`MainMenuActivity`,
  `SettingsActivity`, `MainActivity`).
- Settings lets you pick row/column count (via `SeekBar`s synced to
  `EditText`s), mine percentage, and 4 custom board colors (covered cell,
  uncovered cell, flag, mine) via color chip groups — all persisted to
  `SharedPreferences` and reloaded on next launch.
- The game screen builds the board as a grid of buttons sized from those
  settings, supports tapping to reveal and long-press to flag, and ends the
  game with a win/loss state.

## Tech stack

Java, Android SDK (AppCompatActivity, SeekBar/RadioGroup/ChipGroup UI),
SharedPreferences for settings persistence.

## How it works

Game rules live in `GameLogic.java`, separated from the UI:
- `placeMines()` randomly places mines across the grid, then `computeAdj()`
  precomputes each cell's adjacent-mine count.
- `reveal(r, c)` recursively flood-fills outward from any tapped cell with
  zero adjacent mines, stopping at numbered boundary cells — the standard
  Minesweeper cascade-reveal behavior.
- `isWin()` checks that every non-mine cell has been revealed.
- Flags block reveal on a cell until toggled off (`toggleFlag`), and hitting
  a mine reveals all mines via `revealAllMines()`.

## Build & run

Open the project root in Android Studio and run the `app` configuration on
an emulator or device (Gradle-managed, no extra setup required).
