# Minesweeper Agent Guidance Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create `agent.md` with durable project guidance for building and extending the offline Python/Tkinter Minesweeper copy.

**Architecture:** Keep `agent.md` as a concise repository-level guide that points future agents to the reviewed design, defines the intended module boundaries, and records the development workflow without implementing gameplay. The document will describe the pure engine/UI separation and the staged roadmap for implementing the engine, tests, UI, and validation.

**Tech Stack:** Python 3.11+, Tkinter, Python standard-library `unittest`, Markdown.

---

## Chunk 1: Create project guidance

**Files:**
- Create: `agent.md`
- Reference: `docs/superpowers/specs/2026-11-02-minesweeper-design.md`

- [ ] **Step 1: Draft the guidance sections**

  Include:
  - Project goal and offline/standard-library constraints.
  - Current scope and explicit out-of-scope features.
  - Repository layout and responsibility of each planned file.
  - Engine/UI boundary and public engine behavior expectations.
  - Coding conventions: small focused modules, type hints where useful,
    explicit errors, no hidden network or persistence behavior, and keeping
    game rules out of Tkinter callbacks.
  - Validation commands:
    `python -m unittest discover -s tests -v` and
    `python -m minesweeper.main`.
  - A staged roadmap: engine model, engine tests, Tkinter UI, integration smoke
    checks, and later optional enhancements.
  - Instructions to consult the reviewed design spec before changing the
    architecture or gameplay contract.

- [ ] **Step 2: Review the document for consistency**

  Confirm that the guidance matches the design spec, uses the exact planned
  paths and commands, does not claim unimplemented files already exist, and
  does not add scope beyond classic offline Minesweeper.

- [ ] **Step 3: Commit the guidance**

  ```bash
  git add agent.md
  git commit -m "docs: add project agent guidance" -m "Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
  ```

  Expected: one new commit containing only `agent.md`.
