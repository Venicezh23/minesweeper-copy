# Minesweeper Project Guide

## Goal

Build an offline copy of Minesweeper in Python with a Tkinter desktop
interface. Keep the first release dependency-free by using only the Python
standard library.

The reviewed design specification is
`docs/superpowers/specs/2026-11-02-minesweeper-design.md`. Consult it before
changing the gameplay contract, public engine API, or module boundaries.

## Scope

The first release includes:

- Beginner, intermediate, and expert presets
- Covered, revealed, and flagged cells
- Left-click reveal and right-click flag toggle
- First-click mine safety
- Adjacent-mine counts and zero-cell flood fill
- Win and loss states
- Mine counter, status display, and restart

Keep custom boards, persistence, sound, networking, and online services out of
the initial release.

## Planned Layout

```text
minesweeper-copy/
├── agent.md
├── minesweeper/
│   ├── __init__.py
│   ├── engine.py
│   ├── main.py
│   └── ui.py
└── tests/
```

- `minesweeper/engine.py`: Pure game rules and state transitions.
- `minesweeper/ui.py`: Tkinter widgets, event bindings, and rendering.
- `minesweeper/main.py`: Application entry point.
- `tests/`: Standard-library `unittest` coverage for the engine.

## Design Rules

- Keep game rules out of Tkinter callbacks.
- Keep the engine independent of the UI so it can be tested without a display.
- Use zero-based `(row, column)` coordinates consistently.
- Validate invalid coordinates and configurations explicitly with `ValueError`.
- Treat valid user actions that cannot change the game as safe no-ops with
  explicit action results.
- Inject randomness into the engine when tests need deterministic mine
  placement; do not rely on global random state.
- Prefer small, focused modules and clear public interfaces over premature
  abstractions.
- Do not add network access, persistence, telemetry, or external dependencies
  without an explicit change in scope.

The engine should expose a `MinesweeperGame` with `reveal`, `toggle_flag`,
`restart`, `cell`, and `cells` operations, plus read-only board dimensions,
mine counts, and game state. Preserve the `ActionResult` and `CellView`
contracts described in the design specification.

## Development and Validation

Support Python 3.11 or newer. Run commands from the repository root:

```bash
python -m unittest discover -s tests -v
python -m minesweeper.main
```

Use focused tests while developing, then run the full test suite before
finishing a change. A UI smoke check should only require a display when the
Tkinter application is launched.

## Roadmap

1. Implement the engine model, presets, cell views, action results, and state
   transitions.
2. Add engine tests for placement, counts, flood fill, flags, loss, win, and
   restart behavior.
3. Build the Tkinter UI with preset selection, restart, counter, status, and a
   usable expert grid.
4. Connect UI events to engine actions and perform an integration smoke check.
5. Consider optional enhancements only after the classic offline experience is
   stable and tested.
