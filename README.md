# Looprint (Beta)

**Automated multi-loop printing for Bambu Lab P1 / P1S / A1 / X1 / X1C**

Looprint is a free automated multi-loop G-code builder for Bambu Lab P1, P1S, A1, X1, and X1C printers.

It takes your sliced G-code or 3MF file and automatically creates multiple print loops with push-off sequences between each loop, allowing you to print many copies of the same model without manual interaction.

The core logic is based on [Factorian Designs](https://factoriandesigns.com/)' G-code automation concept.

👉 **It is highly recommended to watch his videos to fully understand the underlying idea and safety considerations:**
- [A1/A1 Mini video](https://factoriandesigns.com/print-automation-bambu-lab-a1-a1-mini)
- [P1/P1S/X1/X1C video](https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2)

## What is Looprint?

**Looprint:**

- Takes an already sliced file (G-code or 3MF)
- Removes the original start/end code
- Wraps the print in a loop
- Adds cooldown + push-off logic between each print
- Automatically ejects finished prints from the bed
- Repeats the process for as many loops as you choose

**No custom printer profiles, firmware mods, or slicer hacks required.**

## Supported Printers

### P1 / P1S (CoreXY)
- **Push System:** X-axis push (gantry moves)
- **Bed Sweep:** Full bed sweep (7 passes, X-axis)
- **Push Lanes:** Dynamic multi-lane system (Left, Center, Right)
- **Z Offset Rule:** 30mm below model top
- **Special Features:**
  - Start code with optional filament flush
  - M190 cooldown before push-off
  - End sound sequence

### X1 / X1C (CoreXY)
- **Push System:** X-axis push (gantry moves) - same as P1/P1S
- **Bed Sweep:** Full bed sweep (7 passes, X-axis)
- **Push Lanes:** Dynamic multi-lane system (Left, Center, Right)
- **Z Offset Rule:** 30mm below model top
- **Special Features:**
  - Start code with optional filament flush
  - Cutter sequence (G1 Y-3) before push-off (X1-specific)
  - M190 cooldown before push-off
  - End sound sequence
  - Automatic printer model detection from 3MF metadata (BL-P001 or BL-P002)

### A1 (Bed Slinger)
- **Push System:** Y-axis push (bed moves, gantry stationary)
- **Bed Sweep:** Wiggle sweep (X: -48 to 256mm)
- **Push Calculation:** Automatic Y-axis push using Factorian's 41mm rule
- **Z Offset Rule:** 40mm below model top (Factorian's rule)
- **Special Features:**
  - Automatic model center Y calculation
  - Clog detection support (G392 S0)
  - Temperature offset (-4°C)
  - Requires 50mm clearance to the left for wide sweep

## Supported File Types

### G-code

**Export from Bambu Studio:**

- File → Export → Export G-Code

**Supported extensions:** `.gcode`, `.gco`, `.g`, `.txt`

**Must be printed from SD card (required)**

### 3MF

**Export from Bambu Studio:**

- Dropdown next to Print Plate → Export plate sliced file

**Supported extension:** `.3mf`

**Can be sent directly to the printer from Bambu Studio**

✅ **Start and end code are added automatically for all file types**

**Note:** Maximum file size: 150MB

## How to Use Looprint

### 1. Upload your file

#### For 3MF files
1. Slice your model in Bambu Studio
2. Click the dropdown next to **Print Plate**
3. Select **Export plate sliced file**
4. Upload or drag the exported `.3mf` file into Looprint
5. File is validated automatically
6. **Printer model is automatically detected from 3MF metadata**

**After generating:**

- Open the downloaded 3MF in Bambu Studio
- Click **Print Plate**
- Do **NOT** slice again

#### For G-code files
1. Slice your model in Bambu Studio
2. Go to **File → Export → Export G-Code**
3. Upload or drag the file into Looprint
4. File is validated automatically
5. **Select printer model manually** (P1/P1S, X1/X1C, or A1)

**After generating:**

- Copy the G-code to SD card
- Print from SD card (required)

### 2. Select Printer Model

- **For 3MF files:** Printer model is automatically detected and locked (P1/P1S, X1/X1C, or A1)
- **For G-code files:** Select your printer model (P1/P1S, X1/X1C, or A1) from the dropdown
- The interface adapts based on your printer selection:
  - **P1/P1S/X1/X1C:** Shows push direction selector (Left/Center/Right) in bed preview
  - **P1/P1S/X1/X1C:** Shows "Enable Start Line Purge" checkbox (optional filament flush in start code)
  - **A1:** Shows "Automatic Y-axis" (no manual selection needed)

### 3. Configure loops

- Set how many times the model should be printed
- **Maximum:** 1000 loops
- **Recommended maximum:** 50 loops (to avoid memory issues)

Looprint automatically detects:

- **Model placement:** Left / Center / Right (P1/P1S/X1/X1C) or automatic Y-axis (A1)
- Placement is shown in the bed preview
- **P1/P1S/X1/X1C:** Bed preview with clickable zones for manual push direction override
- **A1:** Bed preview showing model placement with automatic Y-axis push

### 4. Advanced settings (optional)

Enable via ⚙️ **Show Advanced Settings**

#### Target Bed Temperature
- Bed temperature before push-off starts
- **Default:** 18 °C (≈ 23 °C plate temp)
- **Range:** 15 °C – 90 °C
- **Max wait time:** 60 minutes
- ⚠️ **Warning if above 35 °C** (higher sticking risk)
- **A1 Note:** Temperature offset of -4°C is automatically applied

#### Z Push Offset
- Distance below model top for push
- **Default:** 30 mm (P1/P1S) / 40 mm (A1 - Factorian's rule)
- **Range:** 5 mm – 120 mm
- **A1:** Uses Factorian's 41mm rule for automatic calculation

#### Push Lane Offset (P1/P1S/X1/X1C only)
- Distance from model center for left/right push lanes
- **Default:** 30 mm
- **Range:** 10 mm – 60 mm
- Auto-adjusts if too large to prevent crashes
- Maximum possible value is shown if adjusted
- **Not applicable for A1** (uses automatic Y-axis push)

#### Push Direction (P1/P1S/X1/X1C only)
- Can be manually changed via bed preview zones
- Warning shown if changed from auto-detected direction
- **A1:** Automatic Y-axis push (no manual selection)

#### Enable Start Line Purge (P1/P1S/X1/X1C)
- Optional filament flush in start code
- **Default:** Enabled (uses start code with flush - matches Factorian's P1 template)
- **When disabled:** Uses start code without filament flush sequence (faster start)
- Useful when switching materials or after pause. Disable for faster start when using same filament.

#### Push-off Speed
- Speed of the push-off motion
- **Default:** 300 mm/min
- **Range:** 100 – 1000 mm/min
- Lower = slower, safer for delicate prints

#### Full Bed Sweep (optional)

**P1/P1S/X1/X1C:**
- Sweeps entire bed in 7 passes after push-off to clear debris
- **Pattern:** Safe one-way back-to-front (7 passes across X-axis)
- **Sweep Speed:** Default 300 mm/min (range: 100–1000 mm/min)
- **Sweep Z Height:** Default 1 mm (bed level, range: 0.5–50 mm)
- Always runs 7 passes with automatic spacing
- Respects safe bed boundaries to avoid collisions

**A1:**
- Performs wiggle sweep after push-off
- **Pattern:** X-axis wiggle from -48mm to 256mm
- **Sweep Speed:** Default 300 mm/min (range: 100–1000 mm/min)
- **Sweep Z Height:** Default 1 mm (bed level, range: 0.5–50 mm)
- **⚠️ Important:** Requires 50mm clearance to the left of the bed

Can be tested separately with **Generate Sweep Test File** button

#### Safety Confirmation (required)

Checkbox to confirm your model was sliced with Brim enabled. Brim replaces the standard purge line behavior, ensuring a clean and consistent first layer.

You must confirm that:

- The model is sliced with **Brim** (replaces standard purge line behavior)
- The model is placed at the **edge of the bed**

### 5. Generate and download

- Click **Generate Loop File** to create the full looped print
- Or **Generate Test File** to test only the push-off sequence
- Or **Generate Sweep Test File** (only visible when Full Bed Sweep is enabled) to test only the sweep sequence
- Progress bar shows processing status
- Download the generated file when complete

**Additional options:**

- **Reset to Defaults** resets all settings

## What Looprint Does (Internally)

When generating a looped file, Looprint:

1. **Detects printer model** (from 3MF metadata or manual selection)
2. Analyzes the file (placement, size, settings)
3. Removes original slicer start/end code
4. Adds optimized custom start code (printer-specific)
   - **P1/P1S/X1/X1C:** Optional filament flush based on user setting
5. Repeats the print G-code for each loop
6. Inserts cooldown + push-off sequences between loops:
   - **P1/P1S/X1/X1C:** X-axis push with dynamic multi-lane system
   - **X1/X1C:** Includes cutter sequence (G1 Y-3) before push-off
   - **A1:** Y-axis push using Factorian's 41mm rule
7. Optionally adds bed sweep sequence (if enabled):
   - **P1/P1S/X1/X1C:** Full bed sweep (7 passes)
   - **A1:** Wiggle sweep (X: -48 to 256mm)
8. Adds custom end code (printer-specific)
   - **P1/P1S:** Includes M190 cooldown, dynamic push, and end sound
   - **X1/X1C:** Includes M190 cooldown, cutter sequence, dynamic push, and end sound
9. Preserves all original temperatures and speeds
10. Adds Looprint watermark with GitHub link

## Printer-Specific Features

### P1 / P1S (CoreXY)

#### Dynamic Multi-Lane Push System
Looprint calculates three push lanes:

- **Center lane:** `(minX + maxX) / 2`
- **Left lane:** `center – offset`
- **Right lane:** `center + offset`

This ensures stable ejection regardless of model shape.

**If a lane would exceed bed limits:**

- Offset is automatically reduced
- A warning shows the maximum possible value

After push-off, the hotend moves to a safe X position to avoid collisions.

#### Start Code Options
- **With Filament Flush:** Includes filament flush sequence (default - matches Factorian's P1 template)
- **Without Filament Flush:** Standard start code without flush (faster start)

#### End Code Features
- **M190 Cooldown:** Waits for bed to cool before push-off
- **Dynamic Push:** Multi-lane push system (Left, Center, Right)
- **End Sound:** Plays completion sound sequence

#### Full Bed Sweep
- 7 passes across the X-axis
- Safe one-way back-to-front pattern
- Automatically spaced to respect bed boundaries

### X1 / X1C (CoreXY)

#### Dynamic Multi-Lane Push System
Same as P1/P1S - calculates three push lanes (Left, Center, Right) for stable ejection.

#### Start Code Options
- **With Filament Flush:** Includes filament flush sequence (default - matches Factorian's P1 template)
- **Without Filament Flush:** Standard start code without flush (faster start)

#### End Code Features
- **M190 Cooldown:** Waits for bed to cool before push-off
- **Dynamic Push:** Multi-lane push system (same as P1/P1S)
- **End Sound:** Plays completion sound sequence
- **Cutter Sequence:** Performs `G1 Y-3` before push-off (X1-specific requirement)

#### Full Bed Sweep
- 7 passes across the X-axis
- Safe one-way back-to-front pattern
- Automatically spaced to respect bed boundaries

### A1 (Bed Slinger)

#### Automatic Y-Axis Push
- Uses Factorian's 41mm rule for automatic calculation
- Bed moves (Y-axis), gantry stays stationary
- Model center Y is automatically calculated from G-code coordinates
- No manual push direction selection needed

#### Wiggle Sweep
- X-axis wiggle from -48mm to 256mm
- Clears debris after push-off
- **⚠️ Requires 50mm clearance to the left of the bed**

#### Special G-code Features
- Clog detection support (G392 S0)
- Temperature offset (-4°C)
- Automatic `first_layer_center_no_wipe_tower` extraction and replacement
- M17 Z0.0 removal before M18 X Y Z
- M400 added after push-off

## Logic Firewall

Looprint uses a strict "Logic Firewall" architecture to prevent logic bleed-over between printer engines:

- **P1/X1 Engine:** CoreXY kinematics, X-axis push, full bed sweep
- **A1 Engine:** Bed slinger kinematics, Y-axis push, wiggle sweep
- Strict separation ensures correct G-code generation for each printer type
- Automatic validation prevents cross-engine function calls
- X1 inherits from P1 engine but includes X1-specific features (cutter sequence, end sound, optional flush)

## Important Safety Reminders ⚠️

- **⚠️ Beta Notice:** Looprint is still in beta. Do not leave your printer unattended — always stay nearby and monitor the process during automated looping.
- **Brim is mandatory** - This replaces the standard purge line behavior, ensuring a clean and consistent first layer.
- **Model must be placed at the edge of the bed**
- **G-code must be printed from SD card**
- For 3MF: **do not re-slice after generation**
- Monitor the first loop carefully
- **A1 users:** Ensure 50mm clearance to the left for wiggle sweep
- **P1/P1S/X1/X1C users:** Ensure model placement allows for dynamic push lanes (Left, Center, Right)

## Tips & Best Practices

- Start with 2–3 loops for testing
- Use **Generate Test File** to test push-off only
- Use **Generate Sweep Test File** to test sweep sequence only (when Full Bed Sweep is enabled)
- Keep the bed clean and well-leveled
- Ensure strong first layer adhesion
- **If prints stick:** Lower cooldown temperature slightly
- **If prints release too easily:** Increase cooldown temperature slightly
- **Full Bed Sweep / Wiggle Sweep** is useful for clearing debris between loops, especially with multiple prints
- **A1 users:** Verify model placement allows for wiggle sweep clearance
- **X1/X1C users:** Test with "Generate Test File" to verify push-off sequence

## Troubleshooting

### File won't upload
- Ensure valid `.gcode` or `.3mf`
- **Maximum file size:** 150MB

### Wrong placement detected
- Make sure model is clearly on one side of the bed
- Manual override available in advanced settings (P1/P1S/X1/X1C only)

### Push-off doesn't work
- Verify Brim is enabled
- Model must be at bed edge
- Use **Generate Test File**
- **A1:** Check that model center Y is correctly calculated

### Temperature issues
- All temps come from the original file
- Check slicer settings
- **A1:** Temperature offset of -4°C is automatically applied

### File too large
- **Maximum file size:** 150MB
- Large files take longer to process
- Watch the progress bar

### Too many loops warning
- Appears above 100 loops
- Recommended max: 50

### Push lane offset auto-adjusted (P1/P1S/X1/X1C only)
- Offset was too large
- Check displayed maximum value

### Settings not saving
- Stored in browser localStorage
- Clear browser data if needed

### A1-specific issues

#### Wiggle sweep collision warning
- Ensure 50mm clearance to the left of the bed
- Check model placement in bed preview

#### Y-axis push not working
- Verify model center Y is correctly calculated
- Check that `first_layer_center_no_wipe_tower` is extracted correctly
- Use **Generate Test File** to debug

### X1/X1C-specific issues

#### Cutter sequence not working
- Verify end code includes `G1 Y-3` before push-off
- Check that M190 cooldown completes before cutter sequence
- Use **Generate Test File** to debug

#### Start code flush option
- "Start Line Purge" checkbox controls filament flush in start code
- Default: Enabled (matches Factorian's P1 template)
- Disable for faster start when using same filament

## Need Help?

If you run into issues or want to better understand the automation concept, watch the Factorian Designs videos that this system is based on. They explain the mechanics and safety considerations in detail:

- **[A1/A1 Mini video](https://factoriandesigns.com/print-automation-bambu-lab-a1-a1-mini)** - For A1/A1 Mini users
- **[P1/P1S/X1/X1C video](https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2)** - For P1/P1S/X1/X1C users

---

## License

This project is licensed under the MIT License.

© 2025 Nicki Andersen

---

**Created by Nicki Andersen**

*Free tool. No paywalls. No subscriptions.*

**GitHub:** https://github.com/NickiAndersen/looprint

