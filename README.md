# Block Blast Solver

A single-page solver for Block Blast: draw your board and the three tray pieces, and it lists every play that fits all three — ranked by the lines each one clears.

Live: https://r-lui.github.io/block-blast-solver/

## What it does

- **Every play, not just one.** It walks all drop orders, every legal position, and reports each distinct board you can end up with.
- **Real game rules.** Pieces keep their shape (no rotation), and a completed row or column clears straight after the drop. Two switches let you relax either rule to see what it changes.
- **When nothing fits**, it says so and lists the deepest runs you can actually make, with a per-piece "fits N spots now" readout that shows you which piece is the problem.
- **Hover a play** to preview it on your board; click to pin it. On a phone, a pinned play follows you in a bar at the bottom.
- **Heat map** tints each empty cell by how often the solutions use it.
- **Board as text** for fast input — eight rows of `X` and `.`, pasted in either direction.

## Using it

Open `index.html` in a browser. Nothing to install, no build step, no dependencies beyond an optional web font.

| Action | How |
| --- | --- |
| Draw the board | Drag across cells; drag across filled ones to erase, or right-click |
| Draw a piece | Tap cells in its 5×5 editor, or pick a shape from the list |
| Load a board as text | Open **Board as text**, paste eight rows, press **Load from text** |
| Preview a play | Hover a solution card (or focus it with the keyboard) |

## Notes on the search

The solver packs the 8×8 board into two 32-bit words, so testing a placement is two bitwise operations and a step allocates nothing. Results are de-duplicated by the board each play leaves behind, so the same position reached two ways is listed once.

Safety caps stop a pathological position from freezing the tab: 20,000 solutions, 1,200,000 placements, or 3.5 seconds, whichever comes first. When a cap trips, a **Search deeper** button re-runs with the limits raised. A fully open board with three single-cell pieces is the worst realistic case — 41,664 distinct plays in about 300 ms.
