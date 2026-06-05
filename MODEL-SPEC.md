# WetGregCase — Model Specification

A two-piece, snap-fit 3D-printed enclosure for the **Dilder Full PCB Rev 1**.
The model is generated parametrically by `freecad/wetgreg_case.FCMacro` and
includes visual representations of every major populated component so the fit
can be validated in FreeCAD before printing.

> Status: **v0 — first cut for validation.** All numbers are driven from the
> KiCad model + the two headline clearances the user specified. Everything is
> parametric (the `PARAMS` dict at the top of the macro). Adjust and re-run.

---

## 1. Source geometry (from KiCad: `Dilder Full PCB Rev 1.kicad_pcb`)

| Property | Value | Notes |
|---|---|---|
| Board outline | **97.80 × 42.012 mm** | Rectangle, **R5.0** rounded corners |
| Board thickness | **1.6 mm** | 4-layer, JLC04161H-7628 stackup (KiCad nominal 1.560) |
| KiCad bbox | X 46.70–144.50, Y 45.988–88.00 | Macro shifts min → local origin (0,0) |
| E-ink mount holes | 4 × Ø2.2 (M2) | spacing 66.40 × 24.40 mm |

The macro works in a **local frame**: the board bottom-left corner is `(0,0)`,
`Z = 0` is the **bottom face of the PCB**, and `Z = +1.6` is the top face.

### Component placements (local mm, from KiCad footprints)

| Component | Ref | Side | Footprint X | Footprint Y | Modeled height |
|---|---|---|---|---|---|
| WeAct e-ink screen | J1 | TOP | 12.90 – 84.90 | 6.00 – 36.00 (72 × 30) | front face **11.4 mm above PCB top** |
| Raspberry Pi Pico W | A1 | TOP | 4.90 – 55.90 | 10.29 – 31.29 (51 × 21) | ~3.2 mm of parts above board |
| USB-C | USBC1 | TOP | right edge (+X) | centre Y ≈ 21.0 | 3.3 mm, port faces +X |
| Power slide switch | SW1 | TOP | 46.35 – 54.75 | 37.46 – 42.01 | 2.9 mm |
| LiPo battery (503450) | — | **BACK** | 13.80 – 67.80 | 1.51 – 36.51 (54 × 35) | 5.0 mm |
| 5-way joystick | SW2 | **BACK** | 74.80 – 88.26 | 15.66 – 25.86 | 5.0 mm + cap |

---

## 2. Headline design constraints (user spec)

1. **Screen height** — the WeAct screen front sits **11.4 mm above the top of
   the PCB** (`PARAMS["screen_above"] = 11.40`). This sets the internal height
   of the front plate.
2. **Back-side allowance** — **+2 mm** is added beyond the tallest back-side
   component to clear the Pico board edge and the bolt heads
   (`PARAMS["back_extra"] = 2.00`).
   - Back cavity depth = `max(battery, joystick) + back_extra` = `5.0 + 2.0`
     = **7.0 mm** below the PCB.

---

## 3. Two-piece architecture

```
              ┌───────────────── screen window ─────────────────┐
   Z=+13.0    ╔══════════════════════════════════════════════════╗  ← front plate top (screen front)
              ║   FRONT PLATE (cover, wraps OUTSIDE back plate)   ║
   Z=+5.0 ····║··········· snap nibs ↔ recesses (Z≈2.5) ··········║··· back-plate wall top
   Z=+1.6 ────╫──────────────── PCB top face ────────────────────╫───
   Z= 0.0 ────╫────── PCB rests on LIP (ledge) ──────────────────╫───  ← lip top = PCB bottom
              ║        BACK PLATE (tray) — back cavity 7 mm        ║
   Z=-7.0     ║   floor (battery + joystick + 2 mm clearance)      ║
   Z=-8.6     ╚══════════════════════════════════════════════════╝  ← outside floor
```

### Back plate (tray)
- A shallow tray: floor + perimeter walls (**2.0 mm** thick).
- **Inward lip / ledge** (`lip_w = 1.5 mm`): the PCB drops into the pocket and
  its **back face rests on top of this ledge** at `Z = 0`. The pocket above the
  ledge is sized board **+0.3 mm clearance per side** so the board self-locates.
- **Back cavity** (7 mm deep) below the ledge holds the battery, joystick and
  any protruding bolt heads / Pico edge.
- **Snap recesses** cut into the outer wall face receive the front-plate nibs.
- **USB-C opening** cut through the +X wall (shared cut with the front plate).

### Front plate (cover)
- Wraps **outside** the back plate walls with a `0.2 mm` slip clearance.
- Walls **2.0 mm** thick, descend `5 mm` past the back-plate wall top to overlap.
- **Ceiling** `1.6 mm` thick at `Z = 13.0`, with a **screen window** cut over the
  active area (body 72 × 30, window inset `2.5 mm` = bezel).
- **Cantilever snap nibs** (project 0.8 mm inward, 1.2 mm tall) on the inner wall
  face seat into the back-plate recesses → audible snap, tool-free removal.
- Same **USB-C opening** through the +X wall.

---

## 4. Key dimensions (computed)

| Dimension | Value |
|---|---|
| Back cavity (PCB bottom → floor inner) | 7.0 mm |
| Back plate outside footprint | 102.4 × 46.6 mm |
| Front plate outside footprint | 106.8 × 51.0 mm |
| Overall assembled height | **21.6 mm** (Z −8.6 → +13.0) |
| Wall thickness (both plates) | 2.0 mm |
| PCB fit clearance | 0.3 mm / side |
| Snap slip clearance | 0.2 mm |
| Lip overlap under PCB | 1.5 mm |
| Snap nibs | 6 (2 per long wall, 1 per short wall) |

Both plates verified in FreeCAD as **single, valid, closed (watertight) solids**.

---

## 5. Features checklist

- [x] PCB rests on an internal lip (back plate)
- [x] Front plate snaps around the back plate (6 cantilever nibs)
- [x] 11.4 mm screen clearance above PCB top
- [x] +2 mm back-side allowance for Pico edge / bolts
- [x] Screen window with 2.5 mm bezel
- [x] USB-C side opening (+X edge)
- [x] Component visuals: screen, battery, joystick, Pico W, USB-C, switch, standoffs

---

## 6. Open items / assumptions to confirm during validation

> These are flagged because the KiCad data and the verbal brief differ slightly,
> or because the source files were ambiguous. All are single-parameter fixes.

1. **Pico side.** KiCad places the Pico W on the **TOP (F.Cu)** side, but the
   brief describes "the pico board and bolts" on the **back**. The model follows
   KiCad (Pico on top). If the Pico is actually back-mounted, move it and revisit
   `back_extra`. The 2 mm allowance is honored either way as extra back clearance.
2. **Battery / joystick are on the BACK** (KiCad B.Cu). The back cavity already
   clears them (7 mm). If you want a **joystick access hole** through the back
   floor, that is not yet cut — easy to add.
3. **M3 corner mounting holes** are documented in `BOARD_DESIGN_SPEC.md`
   (Ø3.2, 4 mm inset) but were **not found** in the drill files for this board
   revision. The case currently relies on the snap fit + lip, not bolts. Add
   bolt bosses once the holes are confirmed.
4. **Power switch actuator** — SW1 sits near the +Y top edge. No actuator slot is
   cut yet; add one once the switch travel direction is confirmed.
5. **Speaker (Rev 2 plan)** — `DILDER-PCB-REV2-PLAN.md` reserves a ~35 × 25 mm
   speaker. Not in this board; no cavity reserved.
6. **Component heights** (battery 5 mm, joystick 5 mm, screen module 4.4 mm,
   standoffs ~7 mm) are nominal estimates — measure your actual parts.

---

## 7. Parameters reference

All values live in the `PARAMS` dict at the top of
[`freecad/wetgreg_case.FCMacro`](freecad/wetgreg_case.FCMacro). The most useful:

| Param | Default | Meaning |
|---|---|---|
| `screen_above` | 11.40 | screen front height above PCB top |
| `back_extra` | 2.00 | extra back clearance beyond tallest back part |
| `wall` / `fwall` | 2.00 | back / front wall thickness |
| `floor` / `ceil` | 1.60 | back floor / front ceiling thickness |
| `pcb_clear` | 0.30 | PCB-to-pocket clearance per side |
| `snap_clear` | 0.20 | back-outer ↔ front-inner slip gap |
| `lip_w` | 1.50 | ledge overlap under the PCB |
| `snap_proj` / `snap_h` / `snap_w` | 0.8 / 1.2 / 8.0 | snap nib catch / height / width |
| `scr_win_inset` | 2.50 | screen bezel width |
| `explode_z` | 0.0 | lift front plate N mm for an exploded view |

---

## 8. How to run

```bash
# GUI
FreeCAD → Macro → Macros… → wetgreg_case.FCMacro → Execute

# Headless (regenerate FCStd + STEP exports)
cd freecad && freecadcmd wetgreg_case.FCMacro
```

Outputs:
- `freecad/WetGregCase.FCStd` — full assembly (plates + components)
- `exports/WetGregCase_BackPlate.step`
- `exports/WetGregCase_FrontPlate.step`
