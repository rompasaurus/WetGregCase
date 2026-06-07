# WetGregCase — Design Progression (manual FreeCAD walkthrough)

How to recreate this enclosure by hand in FreeCAD, step by step. The macro
(`freecad/wetgreg_case.FCMacro`) automates all of this from the `Parameters`
spreadsheet, but the steps below are what it does, in order.

**Coordinate frame (local):** `local = (KiCad_X − 46.70, KiCad_StepY + 88.0, Z)`
so the board sits at X 0..97.8, Y 0..42.012, with **Z = 0 at the PCB bottom**
face. +Z is the Pico/USB side (back), −Z is the screen/joystick side (front).

The case is a **clamshell**: a deep **front lid** (screen, Z −12.4 → 0) and a
deep **back cover** (Pico/USB, Z 0 → 8.5) that clips onto it.

---

## 0. New document
1. `File → New`. Save as `WetGregCase.FCStd`.

## 1. Import & align the Dilder PCB assembly (reference)
1. `File → Import` → `Dilder-PCB/Case Design/Dilder_PCB_FullAssembly.step`.
2. Select the imported part → **Edit → Placement** → Position `(−46.70, +88.0, 0)`.
   This lands the KiCad board into the local frame (board bottom at Z=0).
3. **Strip the unpopulated header pins:** delete the tall solid at the +Y edge
   (local X 8.4..28.7, Y 37..39.5, reaching Z +10.1) — those are empty holes.
4. **Seat the screen to the front face:** select the WeAct panel solids
   (local X −0.4..71.6, Y 6.1..36.1, on −Z) and move them down in Z so the
   screen's front face sits just behind the front lid's inner face.
5. Keep this assembly visible, semi-transparent — it's the fit reference only.

## 2. Parameters spreadsheet
1. `Spreadsheet workbench → New sheet`, name it **`Parameters`**.
2. Add one row per dimension and **set an alias** on the value cell. Key ones:
   `board_w=96.8`, `board_h=42.012`, `wall=2.4`, `clear=0.3`, `ext_xmin=1.2`,
   `corner_r=7.7`, `screen_top=12.4`, `front_curve=6.0`, `back_above=8.5`,
   `back_curve=6.0`, `ledge_in=2.0`, `ledge_depth=10.0`, `lap=3.0`, `lap_fit=0.2`,
   `bead_proj=0.8`, `bead_h=1.2`, `snap_w=6.0`, `win_x0=12.6/x1=63.6/y0=7.1/y1=35.1`,
   `usb_*`, `sw_*`, `joy_*`, `thumb_*`, `bat_*`.
3. Drive every dimension below from these aliases (`Parameters.alias`) so editing
   a cell + **Ctrl+Shift+R** updates the model.

> Outer footprint corners (used below): X `−(ext+clear+wall)` .. `board_w+clear+wall`,
> Y `−(clear+wall)` .. `board_h+clear+wall`. Inner cavity = board + clear.

---

## 3. Front plate (screen lid) — PartDesign Body "FrontCover"
1. **New Body.** Sketch a rectangle on the XY plane at **Z = −12.4** covering the
   outer footprint (≈ 103.6 × 46.6). **Pad** it up to **Z = 0** (length `screen_top` = 12.4).
2. **Fillet** the 4 vertical edges → **7.7 mm** (rounded corners).
3. **Fillet** the front (−Z, bottom) perimeter edge → **6.0 mm** (the aggressive
   "looks-thin" curve).
4. **Thickness** (shell): select the open **+Z (top) face**, value **2.4 mm**,
   reversed/inward → a constant-wall curved shell, open at the top. *(Shelling
   after the fillet is what avoids the undercut gap the curve would otherwise make.)*
5. **PCB rib / ledge:** sketch a rounded **frame** (outer = wall inner; inner =
   2 mm further in, `ledge_in`) and **Pad it down** from Z=0 to the **front floor**
   (`ledge_depth` = `screen_top − wall` = 10.0). This is the lip the PCB rests on at
   Z=0, baked into the walls as a structural rib. Because the front wall curves
   inward below ≈−5.4, the straight rib outer ends up **inside** the curved wall,
   so the rib **merges with the curve** (no gap, no overhang, no poke-through) and
   meets the front face. Then Pocket a **screen-clearance box** (screen footprint
   +0.3, Z from the front floor up to just past the screen back) so the rib clears
   the screen on the −X edge.
6. **Screen window:** Pocket a rounded rectangle through the front frame, sized
   **inside the 4 M2 screw holes** so the frame covers them: X 12.6..63.6,
   Y 7.1..35.1 (1 mm shy of the screen PCB; X tucks the right-side 8-pin connector).
7. **Joystick boss:** add a **Ø20 cylinder**, **5 mm** tall, at (81.55, 21.30)
   from the front face inward — room for the solar-panel connector + thumbpiece.
8. **Battery / cable gap:** Pocket a blind notch at CN1 (X 41.5, −Y side) from the
   inside for routing battery cables. Its floor stops at **`bat_floor` (2.4 mm)
   above the front face** — i.e. just above the front floor — so the **front skin
   stays fully solid and printable** (no cut-through; the front is curved here, so
   the floor must clear the whole 2.4 mm front wall, not a thin slice). Start at the
   wall inner — **do not** cut the outer wall, and keep it below the window (Y < 7).
9. **Drill notches:** Pocket the rib away around the 2 mount holes closest to the
   Y axis (X 2.4, Y 9.0 and 33.4).
10. **Clip tongue + snap beads:** Pad a rounded frame ring up from Z=0 by
    `lap−lap_fit` (lap = 3 mm) at the inner wall, 0.2 mm clearance all round. Then
    add **discrete snap beads** (boxes) on the tongue's outer face — 2 per long
    wall + 1 on −X + **2 on +X flanking the USB-C port** (**7 total**) — each with a
    top lead-in chamfer. The bead projects
    `bead_proj` = 0.8 mm past the relief wall, so the thin tongue flexes ~0.6 mm and
    **clicks firmly** into the backplate recesses. The tongue is **continuous** all
    the way around — no relief cuts at the USB-C or switch — so the bead runs
    unbroken there for a stronger clasp. All inside the lap → **no added case
    thickness**. *(The 2.4 mm wall, +0.4 over the original 2.0, is what leaves a
    ~0.5 mm solid skin behind the deeper recess so the back wall isn't cut through.)*
11. **Joystick hole:** Pocket a **Ø12** cylinder through everything at (81.55, 21.30).

> **Sizing the +X length:** the case length is driven entirely by `board_w`
> (`OX1 = board_w + clear + wall`). To shorten the case, *reduce `board_w`* — never
> slab-cut the +X end, which would flat-chop the corner fillet and edge curve.
> `board_w` = 96.8 (1 mm under the KiCad nominal, matching the real board).

## 4. Back cover — PartDesign Body "BackPlate"
1. **New Body.** Rectangle on XY at **Z = 0** (outer footprint). **Pad** up to
   **Z = 8.5** (`back_above`).
2. **Fillet** vertical edges → 7.7 mm.
3. **Fillet** the **+Z (top) back edge** → 6.0 mm (same curve as the front).
4. **Thickness** (shell): open the **−Z (bottom) face**, 2.4 mm inward → curved
   closed back, walls, open toward the front. Houses the USB-C + Pico.
5. **Lap relief + snap recesses:** Pocket a rounded frame (inner wall, over the
   `lap` band from Z=0 up) to receive the front tongue. Then Pocket a **recess at
   each snap bead** in the relief wall — the relief wall stays solid between
   recesses, so the beads must flex past it and snap home.
6. **USB-C opening:** Pocket through the +X wall, Y 16.2..25.8, Z 0..5.2, with
   1 mm rounded corners.
7. **Power-switch slot:** Pocket through the −Y wall, X 47.5..53.5, Z 1.0..3.3.

## 5. Thumbpiece — PartDesign Body "Thumbpiece"
1. **New Body.** Additive **cylinder** Ø11 × 4.5 at (81.55, 21.30), front face.
2. **Subtractive sphere** R12 on the outer face → concave thumb dish.
3. **Subtractive box** 3.3 × 3.3 × 3.5 from the inner face → socket for the
   K1-1506SN-01 actuator.

## 6. Check
- Each Body should be a **single, closed (watertight) solid**.
- Front↔Back **interference = 0** when assembled (clip seats with 0.2 mm slip).
- Overall size ≈ **103.4 × 47.4 × 20.9 mm** (board_w 96.8; walls +0.4; curves intact).

---

## Why each measure
| Measure | Reason |
|---|---|
| translate (−46.70, +88, 0) | maps the KiCad STEP into the local frame |
| front-cover depth `screen_top` = 12.4 | screen standoff + 1 mm; screen rests just behind the window |
| shell (Thickness) after the fillet | curved wall with no undercut gap |
| ledge rib Z 0 → −10.0 (front floor) | PCB rests at Z=0; rib runs to the front floor, meshes with the curve, clears the screen |
| window inside the screw holes | frame fully covers the M2 holes + 8-pin connector |
| back cover Z 0 → 8.5 | clears the Pico (5.26) **and its micro-USB** under a wall-thick curved back |
| discrete snap beads inside the lap (0.55 mm interference) | front/back click together; no external features, no added thickness |
| ext_xmin = 1.2 | front = PCB + walls (not over-wide); clears the screen edge |
