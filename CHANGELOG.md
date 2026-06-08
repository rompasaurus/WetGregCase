# WetGregCase — Revision History

Design revisions of the parametric FreeCAD enclosure, newest at the bottom.
Every part is driven by the `Parameters` spreadsheet in `freecad/wetgreg_case.FCMacro`.

| Rev | Change |
|---|---|
| init | Two-piece snap-fit case scaffold + GitHub repo |
| v1 | Flush lap joint, real KiCad STEP assembly import, joystick thumbpiece, switch slot |
| v2 | Slim case — screen+joystick on the front, thin USB-C-height backplate, curved front |
| v3 | Backplate as board carrier, screen wrap, joystick boss, window before pins |
| v4 | Closed back panel, curved shell (no undercut gap), recessed screen, round joystick boss |
| v5 | Screen window sized inside the 4 screw holes (fully covered) + back-edge curve |
| v6 | Screen window extended in Y to 1 mm shy of the screen-PCB edges |
| v7 | Narrow switch slot to actuator travel; tighten the USB-C opening |
| v8 | Slot the PCB shelf under the USB-C connector |
| **v9** | **Parametric PartDesign rewrite with a `Parameters` spreadsheet (like dilder mk2)** — clean B-rep, slices cleanly |
| v10 | Thin PCB side lip + snap bead/groove closure; narrow window −X edge |
| v11 | Fix mate z-fighting — all-around clearance on the lap tongue |
| v12 | Front plate becomes the tray (PCB on internal ledge); backplate = clip-on cap |
| v13 | Notch the front ledge around the 2 mount holes nearest the Y axis |
| v14 | Rounded frame features (no corner overhang); patch battery wall hole; narrower front |
| v15 | Clamshell flip — front = screen lid (lip), backplate = deep curved cover (USB+Pico) |
| v16 | Pad the PCB ledge down into a structural side rib |
| v17 | Extend battery gap into the curved front-bottom for cable routing |
| v18 | Keep the front floor solid under the battery gap (no cut-through) |
| v19 | Fix PCB rib overhang at the curve (straight-wall depth + 45° chamfer) |
| v20 | Rib meets the front face and meshes with the curve (no chamfer) + screen clearance |
| v21 | Raise front-cover depth +4 mm (PCB shelf 11.4 → 15.4) |
| v22 | Real snap-fit closure — discrete snap beads inside the lap (no added thickness) |
| v23 | Move +X snaps to flank the USB-C port (keep the port hole intact) |
| v24 | Fix snap recess depth (don't cut the back curve) + −X snap missing the extended wall |
| v25 | USB-C clearance gap in the front +X tongue (connector settles in) |
| v26 | Reduce front-cover depth back to the screen standoff (screen rests at the window) |
| v27 | +1 mm front-cover thickness; shave 2 mm off each end of the window's long edges (−4 mm); auto-export 3MF print meshes |
| v28 | Trim the +X end 1 mm past the USB-C (drops the +X snaps, keeps 5); cut a power-switch clearance gap in the front −Y tongue |
| v29 | **Undo the +X slab trim** (it flat-chopped the corner curve). Shorten by reducing `board_w` 97.8 → 96.8 so the whole case regenerates 1 mm shorter with all fillets/curves intact; restore the 2 +X snaps flanking the USB-C |
| v30 | Deepen the backplate 1 mm (`back_above` 7.5 → 8.5) so it closes flush over the Pico's micro-USB |
| v31 | Stronger clasp: drop the front-tongue relief cuts at the USB-C & switch (`Cut_FC_USB`/`Cut_FC_Switch`) so the bead runs unbroken; +0.4 mm bead projection (0.4 → 0.8); +0.4 mm walls all around (`wall` 2.0 → 2.4, `corner_r` → 7.7) to keep a solid recess skin and mate smoothly; +0.2 mm USB-C opening depth (`usb_z1` 5.0 → 5.2) |
| v32 | Extend the window's Y-axis edge 1 mm toward the Y axis (`win_x0` 12.6 → 11.6) |
| v33 | Extend the USB-C opening depth another 0.4 mm (`usb_z1` 5.2 → 5.6) |
| v34 | Extend the USB-C opening depth another 0.4 mm (`usb_z1` 5.6 → 6.0; ~at the inner ceiling 6.1) |
| v35 | Carve the USB-C slot up into the curved top cap (`usb_z1` 6.0 → 7.5) so the connector clears the curvature; ~1 mm cap roof remains, curve intact off the USB window |
| v36 | Run the USB-C slot clean through the curved cap to the back face (`usb_z1` 7.5 → 9.0 ≥ cap top) — open-topped notch, no curve overhang; cap intact outside the USB window |
| v37 | Start the USB-C cut `back_curve` (6 mm) further inboard so it also removes the top cap's inward lean over the USB window (the rounded edge curves toward −X near the top); cap intact off the USB window |
| v38 | Pull the USB-C cut back from the base (`usb_z1` 9.0 → 7.0): no longer cuts through the back face, leaves a ~1.5 mm solid cap roof (still clears the connector ~4.9) |
| v39 | Front-cover outer lip at the USB notch (`Lip_FC_USB`, `usb_lip_h`=2.1): fills the lower part of the backplate's USB cutout flush with the outer surface, covering the bead/seam; +0.4 mm back-cover depth (`back_above` 8.5 → 8.9) so the Pico clears the shell |
| v40 | Raise the power-switch slot 1 mm (`sw_z0` 1.0 → 2.0, `sw_z1` 3.3 → 4.3); confirmed it cuts clean through the −Y wall |
| v41 | Reduce the USB-C cutout depth 0.6 mm (`usb_z1` 7.0 → 6.4); thicker ~2.5 mm cap roof, still clears the connector |
| v48 | **Board now rests ON the bead** (`board_rest_z` 0.63 → 2.13): the board bottom sits ~at the bead top, lifting the USB-C base (2.80) clear of the tongue top (2.8). So the **USB tongue relief is removed entirely** (`Cut_FC_USB` gone) — the bead/tongue fully connect under the connector — and the **lip meets the bead top** (`usb_lip_h` → 2.10). Back deepened (`back_above` → 10.2) so the now-higher Pico clears; front thinner (`screen_top` → 8.91); `usb_z1` → 7.2 for the raised connector. Overall 19.31 → 19.11 |
| v47 | **Raise the board so the PCB top is flush with the bead top** (new `board_rest_z`=0.63; bead restored to the lap centre). Lifts the USB-C/switch into the backplate (accessible) and lets the front USB lip grow (`usb_lip_h` 0.67 → 1.3). Front thinner (`screen_top` → 10.41; overall 19.94 → 19.31); Pico still clears (back unchanged). Slider tab thinner + rounded (more flush to the side) and tracks the raised switch |
| v46 | **Slide-switch (MSK12C02) slider — v1.** Enlarge the −Y slot to a slider window (`sw_*`/`sld_*` params) exposing the actuator; add a printed `SwitchSlider` part (thumb tab + neck + knob grip pocket) that rides the slot. Valid solid, fits with clearance. Grip + full case-rail capture need the real knob width/travel/protrusion (assumed 1.2/1.5/0.6 mm) |
| v45 | Lower the snap bead so its top is flush with the PCB top (`bead_z` = board_t − bead_h; `board_t` 1.6 → 1.47 real). USB tongue relief kept minimal — raising the board to clear it would push the Pico into the back cover (thicker), which conflicts with the connector sitting below the PCB top |
| v44 | Bring the −X short side in 0.9 mm (`ext_xmin` 1.2 → 0.3) so the wall clears the screen overhang (−0.394) by 0.2 mm; keep the screen-clearance pocket from breaching the tightened wall. Restore the USB lip to meet the connector (`usb_lip_h` 0.5 → 0.67) |
| v43 | **Front cover 1.36 mm thinner** to match reality: real screen sits 8.64 mm from the board, so `screen_top` 12.4 → 11.04 (`ledge_depth` → 8.64). Window inner now meets the screen so the board seats on the shelf; overall height 21.3 → 19.94 |
| v42 | **Calibrate the reference assembly to reality.** Keep the screen at its authoritative STEP position (`screen_cal`=0; was shoved −1.36 mm onto the case front floor) → it now sits ~1.36 mm behind the window as in reality. Extend the rib's screen-clearance to the real screen back (`screen_back`). Fix the USB-C colliding with the front cover: lower the lip below the connector (`usb_lip_h` 2.1 → 0.5) and re-add a narrow tongue relief at the USB window (lip + flanking beads kept) |

## Current key dimensions (v48)

| | |
|---|---|
| Outer footprint | 102.5 × 47.4 mm (−X brought in to clear the screen +0.2) |
| Overall height | 19.11 mm |
| Front-cover depth | `screen_top` = 8.91 mm (real screen 8.64 − board_rest_z + wall) |
| Backplate depth | `back_above` = 10.2 mm (USB-C + Pico; clears micro-USB + Pico-to-shell) |
| Wall / curves | 2.4 mm walls · r7.7 corners · r6 front **and** back edge curves |
| Screen window | X 11.6–63.6, Y 7.1–35.1 (inside the screw holes; long edges shaved) |
| Closure | flush lap + discrete snap beads (2/long wall + 1 −X + 2 +X flanking the USB-C = 7), 0.8 mm projection, tongue continuous at the USB/switch |
| Controls | side USB-C opening · −Y switch slot · joystick boss + thumbpiece |

## Outputs (regenerated by the macro)
- `freecad/WetGregCase.FCStd` — full assembly + Parameters spreadsheet
- `exports/WetGregCase_{FrontCover,BackPlate,Thumbpiece}.step`
- `freecad/WetGregCase-{FrontCover,BackPlate,Thumbpiece,SwitchSlider}.3mf` — print-ready meshes
- `exports/WetGregCase_SwitchSlider.step`
