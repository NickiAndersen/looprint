<div align="center">
  <img src="logo.png" alt="Looprint Logo" width="200">
</div>

# Looprint

**Automated multi-loop printing for Bambu Lab printers**

Looprint is a free automated multi-loop G-code / 3MF builder for Bambu Lab printers.  
It allows you to print many copies of the same model automatically by looping the print and pushing finished parts off the build plate.

👉 **Web tool & instructions:** https://nickiandersen.github.io/looprint/

No firmware mods.  
No custom printer profiles.  
No paywalls.

---

## Supported printers
- **P1 / P1S**
- **X1 / X1C**
- **A1 / A1 Mini**

---

## What Looprint does
- Takes an already sliced **G-code or 3MF**
- Removes original start/end code
- Wraps the print in a loop
- Adds cooldown and push-off logic between prints
- Automatically ejects finished prints
- Repeats the process for as many loops as you choose

All printer-specific logic is handled automatically.

---

## How to use
1. Slice your model normally in Bambu Studio  
2. Upload the exported G-code or 3MF to Looprint  
3. Choose number of loops  
4. Generate and print  

👉 **All detailed instructions, settings, and safety notes are built directly into the Looprint interface.**

---

## Important notice ⚠️
⚠️ **Beta Notice:** Looprint is still in beta.  
Not all features have been tested on hardware. Do not leave your printer unattended — always stay nearby and monitor the process during automated looping.

---

## Credits
The core automation concept is inspired by **Factorian Designs**.  
Watching his videos is highly recommended to understand the mechanics and safety considerations:

- A1 / A1 Mini  
  https://factoriandesigns.com/print-automation-bambu-lab-a1-a1-mini
- P1 / P1S / X1 / X1C  
  https://factoriandesigns.com/print-automation-bambu-lab-p1-x1-2

---

## License
MIT License

© 2025 Nicki Andersen

---

**Created by Nicki Andersen**
*Free tool. No subscriptions. No paywalls.*
