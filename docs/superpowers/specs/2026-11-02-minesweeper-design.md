# Minesweeper Design

## Goal

Create an offline copy of Minesweeper in Python with a Tkinter desktop interface.
The first release will implement classic Minesweeper gameplay using only the
Python standard library.

## Scope

The first playable version includes:

- Beginner: 9x9 board with 10 mines
- Intermediate: 16x16 board with 40 mines
- Expert: 30x16 board with 99 mines
- Covered, revealed, and flagged cells
- Left-click reveal and right-click flag toggle
- First-click mine safety
- Neighbor mine counts and zero-cell flood fill
- Win and loss states
- Mine counter, status display, and restart with the current preset

Custom board sizes, persistence, sound, networking, and online services are
out of scope.

## Architecture

Use small modules with clear boundaries:

- `minesweeper/engine.py` contains pure game rules and state transitions.
- `minesweeper/ui.py` contains the Tkinter widgets, event bindings, and
  rendering logic.
- `minesweeper/main.py` provides the application entry point.
- `tests/` contains standard-library `unittest` tests for the engine.

The engine owns board dimensions, mine placement, neighboring-mine counts,
cell visibility, flags, first-click setup, flood-fill reveals, and game
outcomes. The UI translates user actions into engine calls and redraws the
view; it does not implement game rules.

The primary engine API will be a `MinesweeperGame` class. Coordinates use
zero-based `(row, column)` pairs. `reveal(row, column)` and
`toggle_flag(row, column)` return an `ActionResult` containing `changed`
(a boolean), `action` (one of `revealed`, `flag_added`, `flag_removed`, or
`ignored`), and the resulting `game_state` (one of `ready`, `playing`, `won`,
or `lost`). The engine also exposes read-only board dimensions, mine count,
remaining mine count, and game state, plus `restart()`. `cell(row, column)`
returns a `CellView` for one coordinate, and `cells()` provides the current
views in row-major order for rendering and tests. A cell view reports whether
the cell is a mine, its adjacent-mine count, and a visibility state of
`hidden`, `revealed`, or `flagged`.

## Behavior and Data Flow

At startup, the UI creates the selected preset and renders every cell as
covered. On the first reveal, the engine places mines away from the selected
cell (the clicked cell itself is guaranteed safe), computes adjacent-mine
counts, and reveals the cell. A zero-count cell recursively reveals adjacent
safe cells. The engine accepts an optional `random.Random` instance so tests
can control mine placement without relying on global randomness.

Right-clicking a hidden cell toggles its flag and updates the remaining-mine
counter. Revealing a mine changes the state to lost and exposes mines.
Revealing every non-mine cell changes the state to won. Restart creates a new
board using the current preset.

Invalid coordinates and invalid configurations raise clear `ValueError`
exceptions because they indicate programming/configuration errors. Coordinate
validation occurs first, so invalid coordinates raise `ValueError` even after
the game ends. For valid coordinates, revealing a flagged or already revealed
cell, flagging a revealed cell, adding a flag when the flag limit is reached,
and any action after a win/loss are safe `ignored` no-ops reported by the
action result with `changed=False` and the current `game_state`. Flags do not
affect win detection. The remaining-mine counter is
`mine_count - number_of_flags` and cannot become negative because flags are
limited to the configured mine count. On loss, all unflagged mines become
`revealed`; existing flags remain `flagged`, including incorrectly placed
flags. Thus a flagged mine is still represented by the `flagged` visibility
state even after loss.

## Testing

Use Python's built-in `unittest` module. Tests should cover:

- Preset dimensions and mine counts
- First-click safety
- Neighbor mine counts
- Flood-fill behavior
- Flag toggling
- Mine exposure on loss
- Win detection
- Restart/reset behavior

Use deterministic random seeds where a test depends on mine placement. Keep
the UI thin; engine tests should provide the majority of behavior coverage.

## Operational Conventions

Support Python 3.11 or newer. From the repository root, launch the game with
`python -m minesweeper.main` and run the test suite with
`python -m unittest discover -s tests -v`. The expected layout is:

```
minesweeper-copy/
├── agent.md
├── minesweeper/
│   ├── __init__.py
│   ├── engine.py
│   ├── main.py
│   └── ui.py
└── tests/
```

The initial UI should provide a preset selector, restart control, remaining
mine counter, status display, and a clickable grid. It should use distinct
visual states for covered, revealed, flagged, and mined cells while remaining
usable at the expert preset.

## Project Guidance

`agent.md` will document the project goal, constraints, architecture, coding
conventions, test commands, and staged roadmap for future coding agents.
