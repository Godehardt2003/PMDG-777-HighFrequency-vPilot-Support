# PMDG 777 HF Bridge for vPilot

A [vPilot](https://vpilot.rosscarlson.dev/) plugin that makes the **audio panels of the PMDG 777 work with VATSIM** - including **HF (High Frequency)**.

vPilot only reads the standard MSFS COM variables. The PMDG 777 does not keep those in sync with its own audio panels, and it has no notion of HF at all. This plugin watches the cockpit and quietly fixes the standard variables in the background. **Nothing in the cockpit is changed.**

100% vibe coded. No guarantee.

Buy me a coffee: https://www.paypal.com/donate/?hosted_button_id=L9JNJAQE6P4PQ

<!-- Replace with your screenshots -->
<img width="1387" height="621" alt="image" src="https://github.com/user-attachments/assets/6be28500-75f5-4818-9a3a-70f25d11d603" />

<img width="1293" height="566" alt="image" src="https://github.com/user-attachments/assets/a382eb6a-30f1-4e69-b9b4-fdf41d4d72e8" />


## What it does

| Cockpit | What vPilot gets |
|---|---|
| VHF L / VHF R **RECV** on panel 1 **or** panel 2 | `COM RECEIVE` of COM1 / COM2 is switched on |
| **HF** L / HF R RECV on either panel | Same as above: HF L uses COM1, HF R uses COM2 |
| **MIC** on VHF L / HF L (panel 1) | COM1 becomes the transmit radio |
| **MIC** on VHF R / HF R (panel 1) | COM2 becomes the transmit radio |
| HF frequency dialled in with a known mapping | The matching **virtual VATSIM VHF frequency** is tuned on COM1 / COM2 |
| HF no longer in use | The previous VHF frequency is restored |

Good to know:

- Only **left and right** are supported. The centre radio and panel are ignored, vPilot has no use for them.
- The **MIC of panel 1 has priority**. Two microphones cannot transmit at the same time in the sim, so panel 2's MIC is ignored.
- If VHF and HF of the same side are both selected, **HF wins**.
- If you dial an HF frequency that has no mapping, nothing happens.
- HF is also treated as "in use" when only the panel 1 MIC is on HF, otherwise you would transmit on the old VHF frequency.

## Installation
Download: [777HfBridge.zip](https://github.com/Godehardt2003/PMDG-777-HighFrequency-vPilot-Support/releases)
1. Close vPilot.
2. Extract the ZIP with **WinRAR** or **7-Zip**, not with the built-in Windows extractor.
3. Copy `Pmdg777HfBridge.dll` **and** `hf-vhf-mapping.txt` into `%LOCALAPPDATA%\vPilot\Plugins\`.
4. Start vPilot and MSFS. The plugin connects automatically, in any order.

> **Plugin doesn't work at all?** Windows may have blocked the DLL. Right-click `Pmdg777HfBridge.dll` -> Properties -> tick **Unblock** (or "Allow") -> OK, then restart vPilot.

A log is written to `Pmdg777HfBridge.log` next to the DLL and to vPilot's debug messages (prefix `[HF]`).

Requirements: Windows, MSFS 2024, PMDG 777, vPilot 3.x.

## The HF mapping file

VATSIM has no real HF. Oceanic controllers publish a **VHF alias** instead: you tune the alias on a VHF radio and AFV treats it as HF. `hf-vhf-mapping.txt` translates the real HF frequency you dial into the cockpit to that alias:

```
# HF_MHz = VHF_MHz   ; comment
5.649 = 131.900      ; NAT_FSS (NAT Bandbox)
5.598 =              ; NAT-A     <- placeholder, nobody has filled this in yet
```

- HF in MHz exactly as the 777 shows it (2.000 - 29.999).
- An empty VHF value is a placeholder and is ignored.
- The file is reloaded while the plugin is running.

The shipped list is **not complete**. It covers Shanwick, Gander and New York Oceanic; entries tagged `[check]` were read from messy PDF tables and should be verified. Many regions (Santa Maria, Reykjavik, Bodø, the Pacific, ...) are missing completely.

**Help wanted:** if you know a correct alias, spot a wrong one, or an FIR changed its frequencies, please open an **issue** or - even better - a **pull request** against `hf-vhf-mapping.txt`, ideally with a link to the official source.

## Sources of the shipped mappings

- [Shanwick & Gander Oceanic Control Procedures (2023/08)](https://ganderoceanic.ca/assets/resources/files/atc/ShanwickEGGXGanderCZQOOceanicControlProcedures.pdf)
- [vZNY New York Oceanic Pilot Briefing](https://zny-uploads.s3.us-east-1.amazonaws.com/documents/Oceanic%20Pilot%20Briefing.pdf)
- [RadioReference wiki: MWARA](https://wiki.radioreference.com/index.php/MWARA) (real-world NAT families, for the placeholders)

## Disclaimer

Unofficial fan project. Not affiliated with PMDG, Microsoft, VATSIM or vPilot. Use at your own risk.
