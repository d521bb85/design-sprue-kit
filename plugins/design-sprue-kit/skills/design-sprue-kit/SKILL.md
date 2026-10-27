---
name: design-sprue-kit
description: Design a 3D-printable model kit on a sprue (parts snap out of a frame) from a model description and a printer model, with a retail-style step-by-step assembly PDF.
---

# Design Sprue Kit

Turn a description of any subject (machine, animal, building, vehicle, toy, mechanism) into a print-in-place sprue kit plus an assembly guide that reads like one from a bought kit.

## Inputs
- **Target model**: what it is, size or detail level, and whether it moves.
- **Printer model**: required. If it is missing, ask for it. Look up the bed size, nozzle, and any **bed exclusion zones** (e.g. Bambu X1/P1 front-left ~18×28 mm), and confirm the specs with a web search if unsure. Default to a 0.4 nozzle and 0.2 layer unless told otherwise.
- **Language**: write all on-part text, labels, and the guide in the user's language.

## Design principles
You have freedom in form and mechanism. Choose what makes the model look good and teach well. Keep these constraints:

- **Architecture**: a stack of flat plates (2.5D) on alignment pins works well for printed sprue kits. Other construction is fine if every part still prints flat with no supports.
- **Make every dimension a multiple of the layer height.** Moving parts in a plate layer get 0.2 mm axial clearance on each side, e.g. a 2.6 mm part in a 3.0 mm layer.
- **Fits** (starting values for 0.4 nozzle and FDM PLA): keyed press fit 0.15 mm; rotating fit 0.3 mm radial; sliding 0.25–0.4 mm. Minimum wall 1.6 mm (4 lines). Minimum feature 0.8 mm.
- **Keyed shafts**: use D-profile shafts and pins so each part fits in only one orientation.
- **Support every rotating assembly at both ends.** Use bearing holes in the outer walls, and make the journals part of the crank or lever. A part held only by a pin will wobble.
- **Use blind holes so parts can't fall through during assembly.** The first plate in the stack gets blind holes; the last plate can have through holes. Pins sit in blind holes so they stay put.
- **Ease of assembly vs. detail.** Merge parts where it costs no detail. Keep separate parts where they carry meaning, motion, or color.
- **Visibility**: put text, engraving, and detail only on faces that remain visible once the model is built. Use windows, contrasting colors, labels, and indicators to explain how the model works.
- **Clearance from everything static.** Check every moving part against everything that stays still: stands, raised text, handles, and bosses included.

## Verification
Build the full 3D assembly and check it with code before you make the sprue:
- Check every pair of parts for intersections across the full motion cycle, with fine angular steps.
- Measure minimum gaps between parts in slices and flag any below the fit tolerances.
- Confirm assembly order: each step's parts can enter along a free path, and nothing gets trapped.
- Check that labels, part numbers, and printed text are legible at the printed size.
Fix the design and re-run the checks until they pass, then go on to the sprue.

## Sprue
- **Prefer one plate** that fits the bed, with a margin, outside the exclusion zones. Chamfer or rotate the frame to clear a zone, and confirm clearance with the convex hull.
- **Gates**: about 0.8 × 0.6 mm, with a 0.4 mm-high neck at the part so it snaps off cleanly. Use more gates on large plates and one or two on small parts.
- Number every part: raised number tags on the runners, and engraved numbers on large plates.
- Nest small parts inside plate cutouts to save space, gated to the surrounding plate.
- Export a single manifold STL and verify it is watertight and one body.
- For prototype fixes, print the changed parts bare, without a sprue, to save plastic. Fold them into the sprue in the next revision.

## Assembly guide (PDF)
Make it look like a retail kit manual: clean, visual, no development notes.
- **Cover**: a hero render, the model name, and a short spec line (size, part count, printer settings).
- **Contents**: the sprue map with part numbers, and a parts list.
- **Before you start**: how to remove parts, how fits work, and how to read the steps.
- **Steps**: one logical action per step. Each step shows a render of the 3D model with new parts in color, slightly offset from their final position, and parts already placed in grey. Use numbered callouts, short imperative text, and a tip only where a mistake is likely.
- **Finish**: a function check (e.g. the motion cycle), and troubleshooting as a short table.
- Render images from the same verified 3D model (e.g. three.js through headless Chromium, or matplotlib projections). Never draw them by hand.

## Deliverables
The sprue STL, the assembly PDF, and a short summary of print settings: layer height, no supports, elephant-foot compensation of about 0.15 mm. Optional extras are an interactive 3D viewer, an assembly animation, and slicing or sending the job to the user's printer if a computer is linked.

## Tooling (suggested)
Python with shapely for 2D, manifold3d for 3D booleans, slicing, and STL export, matplotlib for maps, and reportlab for the PDF. Keep the generator parametric so fits and dimensions can be changed after the first test print.
