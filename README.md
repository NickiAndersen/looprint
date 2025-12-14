# Looprint

**Automated multi-loop printing for Bambu Lab P1 / P1S**

Looprint is a free automated multi-loop G-code builder for Bambu Lab P1 / P1S printers.

It takes your sliced G-code or 3MF file and automatically creates multiple print loops with push-off sequences between each loop, allowing you to print many copies of the same model without manual interaction.

The core logic is based on [Factorian Designs](https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2)' G-code automation concept.

👉 **It is highly recommended to [watch his video](https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2) to fully understand the underlying idea and safety considerations.**

## What is Looprint?

**Looprint:**
- Takes an already sliced file (G-code or 3MF)
- Removes the original start/end code
- Wraps the print in a loop
- Adds cooldown + push-off logic between each print
- Automatically ejects finished prints from the bed
- Repeats the process for as many loops as you choose

**No custom printer profiles, firmware mods, or slicer hacks required.**

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

## How to Use Looprint

### 1. Upload your file

#### For 3MF files
1. Slice your model in Bambu Studio
2. Click the dropdown next to **Print Plate**
3. Select **Export plate sliced file**
4. Upload or drag the exported `.3mf` file into Looprint
5. File is validated automatically

**After generating:**
- Open the downloaded 3MF in Bambu Studio
- Click **Print Plate**
- Do **NOT** slice again

#### For G-code files
1. Slice your model in Bambu Studio
2. Go to **File → Export → Export G-Code**
3. Upload or drag the file into Looprint
4. File is validated automatically

**After generating:**
- Copy the G-code to SD card
- Print from SD card (required)

### 2. Configure loops

- Set how many times the model should be printed
- **Maximum:** 1000 loops
- **Recommended maximum:** 50 loops (to avoid memory issues)

Looprint automatically detects:
- **Model placement:** Left / Center / Right
- Placement is shown in the bed preview

### 3. Advanced settings (optional)

Enable via ⚙️ **Show Advanced Settings**

#### Target Bed Temperature
- Bed temperature before push-off starts
- **Default:** 18 °C (≈ 23 °C plate temp)
- **Range:** 15 °C – 90 °C
- **Max wait time:** 60 minutes
- ⚠️ **Warning if above 35 °C** (higher sticking risk)

#### Z Push Offset
- Distance below model top for push
- **Default:** 30 mm
- **Range:** 5 mm – 120 mm

#### Push Lane Offset
- Distance from model center for left/right push lanes
- **Default:** 30 mm
- **Range:** 10 mm – 60 mm
- Auto-adjusts if too large to prevent crashes
- Maximum possible value is shown if adjusted

#### Push Direction
- Can be manually changed via bed preview zones
- Warning shown if changed from auto-detected direction

#### Safety Confirmation (required)
You must confirm that:
- The model is sliced with **Brim**
- The model is placed at the **edge of the bed**

### 4. Generate and download

- Click **Generate Loop File** to create the full looped print
- Or **Generate Test File** to test only the push-off sequence
- Progress bar shows processing status
- Download the generated file when complete

**Additional options:**
- **Reset to Defaults** resets all settings

## What Looprint Does (Internally)

When generating a looped file, Looprint:
1. Analyzes the file (placement, size, settings)
2. Removes original slicer start/end code
3. Adds optimized custom start code
4. Repeats the print G-code for each loop
5. Inserts cooldown + push-off sequences between loops
6. Adds custom end code
7. Preserves all original temperatures and speeds

## Dynamic Multi-Lane Push System

Looprint calculates three push lanes:
- **Center lane:** `(minX + maxX) / 2`
- **Left lane:** `center – offset`
- **Right lane:** `center + offset`

This ensures stable ejection regardless of model shape.

**If a lane would exceed bed limits:**
- Offset is automatically reduced
- A warning shows the maximum possible value

After push-off, the hotend moves to a safe X position to avoid collisions.

## Important Safety Reminders ⚠️

- Always test with a simple model first
- **Brim is mandatory**
- **Model must be placed at the edge of the bed**
- **G-code must be printed from SD card**
- For 3MF: **do not re-slice after generation**
- Monitor the first loop carefully

## Tips & Best Practices

- Start with 2–3 loops for testing
- Use **Generate Test File** to test push-off only
- Keep the bed clean and well-leveled
- Ensure strong first layer adhesion
- **If prints stick:** Lower cooldown temperature slightly
- **If prints release too easily:** Increase cooldown temperature slightly

## Troubleshooting

### File won't upload
- Ensure valid `.gcode` or `.3mf`

### Wrong placement detected
- Make sure model is clearly on one side of the bed
- Manual override available in advanced settings

### Push-off doesn't work
- Verify Brim is enabled
- Model must be at bed edge
- Use **Generate Test File**

### Temperature issues
- All temps come from the original file
- Check slicer settings

### File too large
- Large files take longer to process
- Watch the progress bar

### Too many loops warning
- Appears above 100 loops
- Recommended max: 50

### Push lane offset auto-adjusted
- Offset was too large
- Check displayed maximum value

### Settings not saving
- Stored in browser localStorage
- Clear browser data if needed

## Need Help?

If you run into issues or want to better understand the automation concept, watch the [Factorian Designs video](https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2) that this system is based on. It explains the mechanics and safety considerations in detail.

---

**Created by Nicki Andersen**

*Free tool. No paywalls. No subscriptions.*

