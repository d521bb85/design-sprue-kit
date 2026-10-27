---
name: design-sprue-kit
description: Design a 3D-printable model kit on a sprue (parts snap out of a frame) from a model description and a printer model, with a step-by-step assembly guide. Use when the user wants a printable kit, a snap-together model, or parts on a sprue.
license: MIT
compatibility: Needs code execution and file output. Web search helps for printer specs.
---

# Design Sprue Kit

Turn a description of a subject and the user's printer into a kit that prints as one sprue and an assembly guide that leads from the sprue to the finished model.

A good kit is:
- **True to the request**: it shows what the user asked for, and it works if the user asked for something that works.
- **Interesting**: it has a clear idea, such as a mechanism you can see working, a cutaway, or a detail that rewards a closer look.
- **Well thought out**: every part has a reason to exist, and every joint has one obvious way to fit.
- **Easy to build**: each part comes off cleanly, goes in only one way, and stays in place while you work.
- **Printable on this printer**: it fits the bed, needs no supports, and the fits work at this nozzle and material.

Work through the steps below in order. Go back to an earlier step whenever a later one shows a problem.

## 1. Understand the request

Find out what the model is, what it should show or do, how big and detailed it should be, and who will build it. The printer model is required. Ask for it if it is missing, and ask about anything else only if the answer would change the design. If the kit has springs, snaps or other bending parts, also ask for the exact filament, type and brand: blends such as matte PLA are more brittle than plain PLA. Write all text on the parts and in the guide in the user's language.

## 2. Learn the printer

Get the bed size, the keep-out areas of the bed (such as a wipe or purge zone), the nozzle and the usual layer heights. Read them from the printer profile of the user's slicer if one is installed. Otherwise check the specs online, or ask the user if you cannot search. From these, set the design rules for this kit: the smallest wall and feature, the layer height that all dimensions follow, the clearances for fixed, sliding and rotating fits, and the steepest unsupported slope (start at 35° from vertical). Keep these as parameters so they can change after a test print.

## 3. Choose the concept

Decide what makes this model worth building: the main idea, the motion or feature it shows, and the level of detail that serves it. Choose how the kit is built so that every part prints flat without supports. Plan how parts join and how the builder tells them apart. Write this down as a short brief and show it to the user before you go further.

## 4. Plan the parts and the build order together

List the parts and the order they go in. For each part, know which face lies on the bed, how it joins its neighbours, and why it is separate. Merge parts when merging loses nothing. Keep parts separate when they move, carry a different colour, or explain something. Details, labels and text go only on faces that stay visible once the model is built.

## 5. Build the 3D model

Build every part and the assembled model in code, driven by the parameters from step 2. Keep the model as the single source for everything that follows, including the checks, the sprue and the guide images. Keep the build order as one list that drives both the checks in step 6 and the steps of the guide, so they cannot drift apart. Before you rely on a helper such as an extrusion or a renderer, run it on a simple test shape and look at the result, to catch broken or mirrored geometry early.

## 6. Verify by assembling it yourself

Before you make the sprue, build the kit in the model, step by step, in the planned order, as a builder would. For each step, check:

- **The part can get there.** It has a free path from outside to its final place, and nothing already fitted blocks it.
- **It goes in only one way.** A wrong orientation or a wrong neighbour does not fit.
- **It stays put.** Nothing falls out or slips when the model is turned over or the next part goes in.
- **Nothing collides.** No two parts overlap, and every gap is at least the clearance for its fit.
- **Earlier steps stay possible.** No part gets trapped, and no later part needs one taken out again.

Then test the finished model:

- Run every motion through its full range in small steps. Check moving parts against everything static, including stands, raised text and bosses.
- Model every spring, snap and flexing arm in its bent shape at each step of its travel, not only at rest. Check it against other parts and against itself, and keep its peak strain under the limit for the filament (start at 2% for PLA, lower for brittle blends).
- For anything that moves under force, compute force against travel with friction from 0.2 to 0.5. At every friction it must return on its own with the spring force at least 1.5 times the friction, a click must drop at least 30% from its peak, and the force at the end of travel must stay under 10 N.
- Confirm every part prints: it has a flat face for the bed, no unsupported slope steeper than the limit, and no feature below the minimum.
- Render the model and every step, then look at the images. Check that the idea from step 3 reads clearly and that text is legible at print size.

When a check fails, fix the design and run all the checks again. Do not move on until they all pass. Tell the user what you checked and what you changed.

## 7. Lay out the sprue

Arrange all parts on one frame that fits the bed. Slicers usually centre an imported file, so the frame must clear every keep-out area when centred, unless you deliver it already positioned in a slicer project. If the parts truly do not fit, use more sprues and tell the user why. Start from these rules and check each one in the model:

- Every part is at least 3.5 mm from any runner, tag or other part, measured through the part's full height, not only at the bed.
- Every cell of the frame is sized for the parts and the runners it actually holds.
- For a 0.4 mm nozzle, runners are about 2.4 mm wide and 2 mm high. Gates are about 0.8 × 0.6 mm, narrow to a 0.4 mm neck at the part, and never touch a visible face.
- Number tags sit on the runners next to their parts, inside the frame and never on a part.

Check that the result is a single valid printable body.

## 8. Write the assembly guide

Make a PDF that reads like the manual of a bought kit: clean, visual, with no development notes. Include a cover, the sprue map and parts list, and short "before you start" notes. Then give the steps, each with one action. In each step, show new parts in colour near their place and fitted parts in grey, and add a short instruction. Add a tip only where a mistake is likely. End with a check that the model works and a short troubleshooting table. Every picture comes from the verified model, and the steps come from the same build-order list as the checks in step 6.

## 9. Check the guide as a builder

Read the guide with only the sprue in hand. Check that every part number in the guide matches the sprue, and that every part is used exactly once. Each step should be clear from its picture and text alone. Look at every picture: it is framed on the action, and every arrow and label points at the right part. Re-render any that miss, and fix anything that makes you stop and think.

## 10. Make it ready to print and deliver

A bare model file opens in a slicer as geometry only. Deliver a project for the user's slicer with the sprue positioned and the settings that matter built in, such as walls, bottom layers, infill, layer height and filament. If the slicer can run here, slice the project as the last check: report the print time and grams of filament, and look at the first layer. If neither is possible, list the settings. Give the user the print file, the guide, and a short summary of the design and the checks it passed. If a test print shows a problem, change the parameters or the design and repeat from the earliest step it affects.
