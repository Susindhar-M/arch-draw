# ArchDraw — Processor Architecture Designer

A simple, fast, no-install schematic tool for sketching processor/digital
architectures: modules, muxes, gates, registers, buses, and the wiring
between them. It's a single self-contained HTML file — open it in a browser
and start drawing.

>  **Vibe coded.** This whole tool was built through back-and-forth
> conversation with [Claude](https://claude.ai), describing what was needed
> and iterating on it turn by turn, rather than hand-written line by line.
> No code review beyond "does it do the thing" — use accordingly.

---

## Features

### Components
- **Module box** — a fully custom block. Add/remove pins yourself, name
  them, mark each as input or output, and place them on the **left, right,
  top, or bottom** edge.
- **External port** — a free-floating connector for "off-page" signals
  (power, clock in, top-level I/O, etc). Can be set as a source or sink, and
  wires into it skip the usual input/output matching rule.
- **Fixed-shape logic**: Mux, AND, OR, NOT, Decoder — drawn in their
  familiar textbook shapes with a fixed pin layout, but editable bit width
  (and input-line count for the decoder).
- **Sequential**: Register, D Flip-Flop.
- **Arithmetic**: full Adder (A, B, Cin → Sum, Cout).
- **Text label** — a free annotation with no pins, for labeling sections of
  the diagram.

### Wiring
- Click a pin to start a wire, **click again to drop a bend**, click the
  target pin to finish — every segment is automatically kept horizontal or
  vertical (no diagonals), so you get clean Manhattan-style routing with
  full control over how it's routed.
- Press **Backspace** mid-route to undo the last bend, **Esc** to cancel the
  wire entirely.
- Wires only connect an input to an output (and reject the attempt with an
  error otherwise) — except external ports, which can wire into either.
- **Buses**: mark a wire as a bus and give it a range (e.g. `31:0`) — it
  renders as a thicker line with the range label floating above it. The
  label can be dragged anywhere.
- Pins always land exactly on the grid, so wires line up cleanly.

### Editing
- Click any shape or wire to select it, then hit **Edit** in the toolbar
  (or just select + Edit — there's no double-click, by design) to open its
  settings: name, color, bit width, and pins.
- Pins can be individually deleted from the editor, even on fixed-shape
  components.
- Pin labels, port name labels, and bus labels can all be **dragged**
  directly on the canvas to reposition them.
- **Rotate** any symbol 90° at a time (toolbar button or `R`) — pins and
  labels rotate with it and stay connected correctly.

### Workflow
- **Duplicate** (`D`) and **Delete** (`Del`/`Backspace`) for the selected
  item.
- **Save to library** — turn any module into a reusable component that
  shows up in the palette for future placement.
- **Undo** (`Ctrl+Z` / `⌘Z`) — covers placing, moving, rotating, deleting,
  duplicating, wiring, and editing.
- **Save file** / **Load file** — exports/imports the whole diagram
  (including your library) as a `.json` file.
- **Export PDF** — choose **Color** (tuned for a white page) or
  **Black & white** (print-friendly, bus wires stay distinguishable by
  thickness alone). If the automatic download gets blocked by the browser,
  a dialog offers "Open in new tab" and "Download again" as fallbacks, and
  if PDF rendering itself fails, it falls back to downloading a `.svg` you
  can open and print to PDF manually.

---

## How to use it

1. **Open `arch-draw.html`** in any modern browser (Chrome, Firefox,
   Edge, Safari). No install, no server, no build step.
2. **Place a component**: click an item in the left palette, then click
   anywhere on the canvas to drop it.
3. **Wire it up**: click a pin → click to add bends if you want to route
   around something → click the target pin to finish.
4. **Configure it**: click the shape to select it, click **Edit** in the
   top toolbar, and set its name, color, bit width, or pins. Click **Save**
   in the dialog when done.
5. **Adjust bus wires**: select the wire, click **Edit**, check **Is bus**,
   and type a range like `31:0`.
6. **Move things around**: drag any shape by its body; drag pin/port/bus
   labels individually if you want them somewhere other than the default
   spot.
7. **Build a module library**: select a module you've configured, click
   **Save to library** — it'll appear under "My modules" in the palette for
   reuse in this diagram (and it's included when you save the file).
8. **Save your work**: click **Save file** to download a `.json` of the
   whole diagram. Click **Load file** later to bring it back.
9. **Export for sharing**: click **Export PDF**, pick Color or Black &
   white, and it downloads a PDF (or SVG, if PDF generation isn't available
   in your browser).

### Keyboard shortcuts

| Key | Action |
|---|---|
| `D` | Duplicate selected shape |
| `R` | Rotate selected shape 90° |
| `Del` / `Backspace` | Delete selected shape or wire — or, while routing a wire, undo the last bend |
| `Esc` | Cancel the wire currently being drawn, or close a dialog |
| `Ctrl+Z` / `⌘Z` | Undo |

---

## File format

`Save file` produces a plain JSON file with three top-level keys:
`symbols`, `wires`, and `library`. It's human-readable and diffable, so
it's fine to check into version control alongside your project if you want
history on your architecture diagrams.

## Known limitations

- Wire waypoints are stored in absolute canvas coordinates — if you move a
  symbol after routing a multi-bend wire to it, the bends won't
  automatically re-route; you'll need to touch them up.
- Decoder output count is driven by the "input lines" setting (2–4 lines →
  4–16 outputs); individual outputs can still be deleted afterward if you
  don't need all of them.
- This is a lightweight sketching tool, not a netlist/HDL-aware editor —
  there's no simulation, no syntax checking beyond the input/output wiring
  rule, and no export to Verilog/VHDL.

