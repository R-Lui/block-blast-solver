# Block Blast Solver

A single-page solver for Block Blast that plays round after round: draw your board and the three tray pieces, see every play that fits, then drop the one you want and solve the next tray.

Live: https://r-lui.github.io/block-blast-solver/

## What it does

- **Every play, not just one.** It walks all drop orders, every legal position, and reports each distinct board you can end up with.
- **Continuous rounds.** Double-tap a play (or pin it and press **Drop it**) and those pieces land on the board, with the lines they clear. The tray empties and the board is ready for the next three shapes — so you can keep solving from where you actually are.
- **Undo.** Every drop leaves an Undo in the confirmation, in case you meant a different play.
- **Real game rules.** Pieces keep their shape (no rotation), and a completed row or column clears straight after the drop. Two switches let you relax either rule to see what it changes.
- **When nothing fits**, it says so and lists the deepest runs you can actually make, with a per-piece "fits N spots now" readout that shows you which piece is the problem.
- **A shape library you can read at a glance.** Five strips — Bars, Blocks, Corners, L and T, Steps and strays — each drawn as 5×5 tiles instead of names. Tap one to send it to the ringed slot; the ring moves on by itself, so three taps fill a tray. Slots can also be picked freely, drawn cell by cell, turned, or emptied with ✕.
- **Hover a play** to preview it on your board; tap to pin it. A pinned play follows you in a bar at the bottom with a **Drop it** button.
- **Heat map** tints each empty cell by how often the solutions use it.
- **Board as text** for fast input — eight rows of `X` and `.`, pasted in either direction.

## Using it

Open `index.html` in a browser. Nothing to install, no build step, no dependencies beyond an optional web font.

| Action | How |
| --- | --- |
| Draw the board | Drag across cells; drag across filled ones to erase, or right-click |
| Fill a tray slot | Tap a tile in the shape library — it lands in the ringed slot |
| Draw a custom piece | Tap cells in a slot's own 5×5 grid, or ✕ to empty it |
| Drop a play on the board | Double-tap its card (or pin it and press **Drop it**) |
| Change your mind | Press **Undo** in the confirmation that appears |
| Load a board as text | Open **Board as text**, paste eight rows, press **Load from text** |
| Preview a play | Hover a solution card (or focus it with the keyboard) |

Only complete plays can be dropped — a run that places two of your three pieces has nothing to put down, so the app says so and offers to turn rotations on instead.

## Notes on the search

The solver packs the 8×8 board into two 32-bit words, so testing a placement is two bitwise operations and a step allocates nothing. Results are de-duplicated by the board each play leaves behind, so the same position reached two ways is listed once.

Safety caps stop a pathological position from freezing the tab: 20,000 solutions, 1,200,000 placements, or 3.5 seconds, whichever comes first. When a cap trips, a **Search deeper** button re-runs with the limits raised. A fully open board with three single-cell pieces is the worst realistic case — 41,664 distinct plays in about 300 ms.
