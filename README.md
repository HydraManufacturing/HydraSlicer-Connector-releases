<p align="center">
  <img src="AddInIcon.svg" alt="HydraSlicer Connector" width="120"/>
</p>

<h1 align="center">HydraSlicer Connector</h1>

<p align="center">
  <strong>Fusion 360 Add-in for Hybrid Additive-Subtractive Manufacturing</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platform-Fusion%20360-blue?style=flat-square" alt="Platform"/>
  <img src="https://img.shields.io/badge/OS-Windows%20%7C%20macOS-lightgrey?style=flat-square" alt="OS"/>
  <img src="https://img.shields.io/badge/language-Python-yellow?style=flat-square" alt="Language"/>
  <img src="https://img.shields.io/badge/machine-CHAMP%20X3-red?style=flat-square" alt="Machine"/>
</p>

---

A Fusion 360 add-in that bridges the gap between CAD/CAM design and hybrid manufacturing on the **CHAMP X3**. HydraSlicer Connector automates the creation of interlayer machining setups and exports hybrid-optimized 3MF files with embedded G-code for use with [HydraSlicer](https://github.com/HydraManufacturing).

## Features

| Feature | Description |
|---------|-------------|
| **Manufacturing Setup** | Automatically creates interlayer CAM setups at user-selected Z heights, with additive model, stock bodies, and final machining setup |
| **Interlayer Setup** | Configure interlayer machining planes and parameters for mid-print milling operations |
| **Export 3MF + G-code** | Export solid bodies as 3MF with embedded interlayer and final machining G-code |
| **Manage Templates** | Save and load material/process templates for repeatable manufacturing configurations |
| **Clear Setup** | Clean up all HydraSlicer-generated setups, models, and metadata from the document |

---

## Installation

### Option 1 — Installer (Recommended)

Download the installer for your platform from the [latest release](https://github.com/HydraManufacturing/HydraSlicer-Connector-releases/releases/latest):

| Platform | File |
|----------|------|
| **macOS** | `HydraSlicer-Connector-vX.X.X.dmg` |
| **Windows** | `HydraSlicer-Connector-Setup-vX.X.X.exe` |

- **macOS:** Open the DMG and double-click the `.pkg` inside. The add-in is installed automatically and Fusion 360 will prompt to enable it on next launch.
- **Windows:** Run the `.exe` installer. No admin rights are required — it installs into your user profile.

### Option 2 — Manual

1. Locate the Fusion 360 Add-ins folder:

   | OS | Path |
   |----|------|
   | **Windows** | `%APPDATA%\Autodesk\Autodesk Fusion 360\API\AddIns` |
   | **macOS** | `~/Library/Application Support/Autodesk/Autodesk Fusion 360/API/AddIns` |

   > **Tip:** In Fusion 360, go to **Utilities > Add-Ins > Scripts and Add-Ins** and click the folder icon next to "My Add-Ins".

2. Extract the ZIP from the [latest release](https://github.com/HydraManufacturing/HydraSlicer-Connector-releases/releases/latest) into the Add-ins folder so the structure looks like:

   ```
   AddIns/
   └── HydraSlicer-Connector/
       ├── HydraSlicer-Connector.py
       ├── HydraSlicer-Connector.manifest
       ├── commands/
       └── lib/
   ```

3. In Fusion 360, go to **Utilities > Add-Ins**, find **HydraSlicer-Connector**, and click **Run**.

---

## Usage Guide

### Overview

Once enabled, a **HydraSlicer** tab appears in the Manufacturing workspace ribbon:

```
┌──────────────────────────────────────────────────────────────────┐
│                     HYDRASLICER TAB (Ribbon)                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  [ Manufacturing  ]  [ Interlayer ]  [ Templates ]  [ Export  ]  │
│  [    Setup       ]  [   Setup    ]  [  Manage   ]  [  3MF    ]  │
│                                                                  │
│                                           [ Clear Setup ]        │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

### Step 1 -- Design Your Part

In the **Design** workspace, create or import the solid body you want to manufacture with the hybrid additive-subtractive process.

### Step 2 -- Switch to Manufacturing

Click the **Manufacturing** workspace. The **HydraSlicer** tab appears in the ribbon.

### Step 3 -- Run Manufacturing Setup

Click **Manufacturing Setup** to configure the hybrid workflow:

1. **Select the target body** to manufacture
2. **Choose interlayer Z heights** -- construction planes where mid-print machining will occur
3. **Configure scaling** -- optional XY/Z scale compensation (percentage)
4. **Configure stock expansion** -- XY/Z material expansion for machining allowance (mm)
5. **Set the post processor** -- select a `.cps` file for G-code generation

The command automatically creates:
- An **Additive Model** (scaled/expanded copy for printing)
- **Interlayer Setups** at each Z height with proper stock and machining target bodies
- A **Final Machining Setup** for post-print finishing operations

### Step 4 -- Add CAM Operations

For each interlayer setup and the final machining setup:

1. Open the setup in the CAM browser
2. Add machining operations (facing, adaptive, scallop, etc.)
3. **Generate toolpaths** for all operations (right-click setup > Generate)

### Step 5 -- Export for HydraSlicer

1. Click **Export 3MF** in the HydraSlicer tab
2. In the dialog:

   | Field | Action |
   |-------|--------|
   | **Body** | Select the solid body to export |
   | **Post Processor** | Choose the `.cps` post processor from the dropdown |
   | **Save Path** | Set the output `.3mf` file path |

3. Click **OK** -- the add-in will:
   - Detect all interlayer setups from stored metadata
   - Post-process each setup to extract G-code
   - Package the mesh geometry and all G-code into a single `.3mf` file

### Step 6 -- Open in HydraSlicer

Open the exported `.3mf` file in **HydraSlicer**. Interlayer machining operations are automatically detected with their Z heights, and you can proceed with hybrid slicing for the CHAMP X3.

---

## Commands

### Manufacturing Setup

Creates the full hybrid manufacturing model from a selected body:

- Slices the body at each interlayer construction plane
- Creates properly bounded stock and target geometry for each interlayer
- Centers geometry on origin using bottom-face centroid for WCS alignment
- Stores all metadata in document attributes for later export

### Interlayer Setup

Configure and manage interlayer machining planes:

- Select construction planes at desired Z heights
- Adjust interlayer parameters
- Preview interlayer slicing positions

### Manage Templates

Save and load manufacturing configurations as reusable templates:

- Material and process parameters
- Scale and expansion defaults
- Post processor selection

### Export 3MF

Exports the part with all machining data embedded:

- Validates that all setups have generated toolpaths
- Post-processes each interlayer and final machining setup
- Outputs a single `.3mf` file containing mesh + G-code

### Clear Setup

Removes all HydraSlicer-generated content from the document:

- Deletes manufacturing models, setups, and derived bodies
- Clears stored metadata attributes
- Returns the document to its pre-setup state

---

## 3MF Export Format

The exported `.3mf` file follows the [3MF specification](https://3mf.io/) with custom metadata for hybrid manufacturing:

```
exported_part.3mf (ZIP archive)
├── [Content_Types].xml
├── _rels/.rels
├── 3D/
│   └── 3dmodel.model                      # Part geometry (mesh)
└── Metadata/
    ├── Hydra_Slicer_interlayer.json        # Interlayer metadata (v2)
    ├── Hydra_Slicer_custom_gcode.txt       # Legacy v1 format
    ├── interlayer_1.gcode                  # Interlayer machining G-code
    ├── interlayer_2.gcode
    └── final_machining.gcode               # Post-print finishing G-code
```

---

## Coordinate System

- **X/Y origin**: Centroid of the bottom faces of the additive body
- **Z origin**: Bottom of the part (build plate level)
- **Units**: Millimeters in exported G-code (Fusion uses cm internally)

The Manufacturing Setup command centers geometry on origin automatically. HydraSlicer transforms the G-code based on object placement on the build plate.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Add-in doesn't appear | Verify the folder is in the correct Add-Ins directory and contains `HydraSlicer-Connector.manifest` |
| HydraSlicer tab missing | Switch to the **Manufacturing** workspace -- the tab only appears in the CAM environment |
| No post processors in dropdown | Ensure you have `.cps` files in Fusion's personal post folder (**Manage > Post Library**), or use the bundled `reprapHydra.cps` |
| Export fails with no G-code | Make sure all setups have operations with **generated toolpaths** (right-click > Generate) |
| Setup validation warnings | Check the export dialog's interlayer info panel -- it shows the status of each setup |
| Coordinate mismatch | Ensure Manufacturing Setup was run with default centering -- the WCS origin uses bottom-face centroid |

---

## Requirements

- **Autodesk Fusion 360** (with Manufacturing/CAM extension)
- **Python 3.x** (bundled with Fusion 360 — no separate install needed)
- A compatible post processor (`.cps`) -- `reprapHydra.cps` is included

---

## For Developers

The source code and development documentation live at [HydraManufacturing/HydraSlicer-Connector](https://github.com/HydraManufacturing/HydraSlicer-Connector).

---

## License

Proprietary -- Hydra Manufacturing. All rights reserved.
