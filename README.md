# Product License Editor

**Supermarket Simulator · Custom Products Loader (CPL) 2.1.2 compatible**

A free, browser-based visual editor for `products.json` files used by the **Custom Products Loader** mod.  
It also bridges data from the **Custom Product Placement** mod so you can import real in-game placement coordinates (box layouts + storage/shelf grids) directly into your CPL product packs.

Created by [ThaWizanator](https://twitch.tv/thawiznator).

Live GitHub Page: [CPL Editor](https://thawizanator.github.io/cpl-product-editor/)
---

## Why this exists

Making custom product packs for Supermarket Simulator normally means hand-editing a large JSON file. That is error-prone and slow, especially for:

- Grid layouts inside boxes (`GridLayoutInBox`)
- Grid layouts on shelves / storage (`GridLayoutInStorage`)
- Optional fields (vending, hand offsets, custom box models, etc.)
- Keeping IDs unique and data valid

This editor gives you a clean form-based UI, a 3D box visualizer, search, undo/redo, import/export tools, and a one-click way to merge placement data created with the Custom Product Placement mod.

**Related mods**

| Mod | Nexus link | What it does |
|-----|------------|--------------|
| **Custom Products Loader (CPL)** | [nexusmods.com/.../mods/1589](https://www.nexusmods.com/supermarketsimulator/mods/1589) | Loads custom products + licenses from `products.json` packs |
| **Custom Product Placement** | [nexusmods.com/.../mods/1297](https://www.nexusmods.com/supermarketsimulator/mods/1297) | In-game tool (default key **N**) that lets you tweak product placement on shelves and inside boxes and saves the results to JSON |

The editor is built around the CPL `products.json` schema (the same structure used by the MoreProducts / CPL ecosystem). It does **not** replace either mod — it is an offline authoring tool that works with the files those mods use.

---

## Quick start (first time)

1. Download or clone this repository.
2. Open `Product License Editor-claudeDOTcom.html` (or whatever the current filename is) in a modern browser.  
   **Recommended:** Chrome or Edge (full File System Access API support for direct save + folder pickers).  
   Firefox and Safari work for basic load/edit/download, but direct “Save” and smart folder pickers are limited.
3. Click **📁 Open File…** and select a `products.json` from a CPL product pack, **or** click **📋 Import / Export JSON** and paste the contents.
4. The editor activates. Use the top toolbar to move between licenses and products. Edit fields on the right. Changes are live in memory.
5. When finished:
   - **💾 Save** (if you linked a file) or **💾 Download** / **📋 Copy** to get the updated JSON.
   - Put the file back into your CPL product pack folder and restart (or reload) the game as usual.

You can also start from scratch with **🆕 New File**.

---

## Main features

### File handling
- Open existing `products.json`
- Create a brand-new empty pack
- **Direct Save** (Chrome/Edge) after linking a file once
- Download a fresh `products.json` or copy the JSON to clipboard
- Close / start new with optional “Save first” prompts
- Automatic validation + safe defaults on load (missing fields, duplicate IDs, empty product lists, etc.) with a clear Import Report

### Navigation & editing
- Toolbar counters + First / Prev / Next / Last for licenses and products
- Add new license, add new product, duplicate product, delete product or entire license
- Collapsible sections for every major data group
- Optional fields are clearly marked and can be left empty (they are omitted from the output JSON)

### Search
- Search by License ID, Product ID, License Name (partial), or Product Name (partial)
- Instant jump to the matching entry

### Grid import (the bridge to Custom Product Placement)
1. Load your CPL `products.json` first.
2. Click **📐 Import Grid Layouts…**
3. Select the JSON produced by Custom Product Placement (the file that contains a `Data` array with product IDs and placement values).
4. The editor matches products by **Product ID** and writes the placement data into:
   - `GridLayoutInStorage` (shelf / display layout)
   - `GridLayoutInBox` (when the source has a “Box layout” section)

Unmatched products are skipped; a toast tells you how many were matched vs skipped.

### 3D Box Visualizer
Open **📐 Open 3D Box Visualizer** from the “Grid Layout In Box” section.

- Interactive Three.js view of the box + product instances
- Load a custom `.obj` model (optional) so you can see your real mesh
- Box size presets (`_20x10x7`, `_15x15x15`, etc.)
- Origin / pivot presets (bottom-center, corners, face centers…)
- Live controls for:
  - First object position (anchor)
  - Spacing (X/Y/Z)
  - Product count & scale
  - Placement grid (columns × rows)
  - Rotation in degrees
- Camera snap buttons (Front / Back / Top / Bottom / Left / Right)
- **Save to Grid Layout** writes the values straight into the current product’s `GridLayoutInBox`

This is the easiest way to dial in nice box packing without guessing numbers.

### Config Converter (JSON ↔ Google Sheets)
- Convert structured `products.json` ↔ flat TSV that pastes cleanly into Google Sheets
- Useful for bulk editing names, prices, IDs, etc. in a spreadsheet, then converting back
- “Show Current” pre-fills from the loaded data
- “Load JSON into Editor” after converting the other direction

### Asset path helpers
- File pickers for icons, OBJ, MTL (inserts relative paths such as `products_icons/...` or `objects_meshes/...`)
- Optional **Set Mod Root Folder…** so subsequent pickers try to start inside `products/<LicenseName>/` or `plugins/<filename>/` instead of your Downloads folder

### Quality-of-life
- Undo / Redo (Ctrl+Z / Ctrl+Shift+Z) with a 50-step history
- Keyboard shortcuts (see below)
- Toasts + status bar feedback
- Dropdowns automatically gain any new Display Type / Category / Box Size values found in loaded data
- Pretty-printed output JSON with compact vector arrays

---

## Keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| `←` / `→` | Previous / Next product |
| `Ctrl` + `←` / `→` | Previous / Next license |
| `Ctrl` + `S` | Save (if linked) or Copy JSON |
| `Delete` | Delete current product |
| `Ctrl` + `Delete` | Delete current license |
| `Ctrl` + `Z` | Undo |
| `Ctrl` + `Shift` + `Z` (or `Ctrl` + `Y`) | Redo |
| `Esc` | Close Settings drawer |

Shortcuts are ignored while typing in an input/textarea so you don’t accidentally navigate away.

---

## Understanding the data you edit

A typical CPL pack looks like this (simplified):

```json
{
  "$schema": "../schemas/product_config.json",
  "ProductLicenses": [
    {
      "ID": 100100,
      "LicenseName": "My Cool License",
      "RequiredPlayerLevel": 1,
      "PurchasingCost": 250,
      "Products": [
        {
          "ID": 200000,
          "ProductName": "Example Soda",
          "ProductBrand": "Example Co",
          "ProductIcon": "products_icons/soda.png",
          "BoxIcon": "products_icons/soda_box.png",
          "ProductDisplayType": "SHELF",
          "Category": "DRINK",
          "ProductPrefab": {
            "objPath": "objects_meshes/soda.obj",
            "mtlPath": "objects_meshes/soda.mtl"
          },
          "ProductAmountOnPurchase": 12,
          "BasePrice": 1.49,
          "MinDynamicPrice": 1.0,
          "MaxDynamicPrice": 3.5,
          "OptimumProfitRate": 150,
          "MaxProfitRate": 300,
          "ItemGridSize": [1, 1],
          "GridLayoutInBox": { ... },
          "GridLayoutInStorage": { ... }
          // many optional fields...
        }
      ]
    }
  ]
}
```

### Important sections in the editor

| Section | What it controls |
|---------|------------------|
| **License Details** | ID, name, required player level, cost of the license card |
| **Product Info** | ID, name, brand, display type (SHELF / FREEZER / FRIDGE), category |
| **Assets & Prefab** | Icon paths, OBJ/MTL paths for the 3D model |
| **Pricing & Quantity** | How many you get when buying, base / dynamic prices, profit rates, item grid size on shelf |
| **Behavior & Display** (optional) | Vending flag, bakery flag, cannot-rotate, custom category, placement speed, name color |
| **Vending Machine** (optional) | Slot/column limits, scale, rotation, crate size |
| **Hand & Model Tweaks** (optional) | Hand offset/rotation, prefab local position/rotation/scale |
| **Custom Box Model** (optional) | Separate box mesh + its transform |
| **Grid Layout In Box** | How products are packed inside the cardboard box (the 3D visualizer lives here) |
| **Grid Layout In Storage** | How products sit on shelves / in storage displays |

Optional fields are only written to the JSON when you actually set a value. Clearing them removes them from the output.

### ID recommendations
CPL authors commonly use high IDs (e.g. licenses ≥ 9000, products ≥ 90000) to avoid collisions with vanilla content. The editor will auto-reassign duplicate or missing IDs on load and tell you what it did.

---

## Typical workflows

### A. Edit an existing product pack
1. Open the pack’s `products.json`.
2. Use Search or the toolbar to find the product.
3. Tweak prices, names, icons, grids, etc.
4. Download / Save and replace the file in your mod folder.

### B. Create a brand-new pack
1. **New File** → a starter license + product is created.
2. Fill in License Details and Product Info.
3. Point the icon and model paths to files that will live next to the JSON (or under the usual `products_icons/` / `objects_meshes/` relative folders).
4. Use the 3D visualizer to set a sensible box layout.
5. Add more products / licenses as needed.
6. Export the JSON and build the rest of the pack folder (icons, models, etc.).

### C. Bring in-game placement data into a CPL pack
1. In-game, use Custom Product Placement (key **N**) to perfect the layout of a product on a shelf or inside a box. The mod writes a JSON file.
2. In this editor, load your CPL `products.json`.
3. **Import Grid Layouts…** and select the Custom Product Placement JSON.
4. Matched products receive the new coordinates. Review them, then save.

### D. Bulk edit in Google Sheets
1. Load data → open the Config Converter → “Show Current” → convert **To Sheets**.
2. Copy the TSV into Google Sheets, edit columns freely.
3. Copy the sheet range back, convert **To JSON**, then “Load JSON into Editor”.

---

## Browser compatibility notes

| Feature | Chrome / Edge | Firefox / Safari |
|---------|---------------|------------------|
| Open / Download / Copy | ✅ | ✅ |
| Direct in-place Save | ✅ (after linking) | ❌ (falls back to download) |
| Directory picker (Mod Root) | ✅ | ❌ |
| 3D Visualizer | ✅ | ✅ (WebGL required) |
| Everything else | ✅ | ✅ |

For the best experience use Chrome or Edge.

---

## Tips & troubleshooting

- **“Invalid / missing fields” on load**  
  The editor never refuses a file just because fields are missing. It fills safe defaults and shows an Import Report so you can review what was changed.

- **Dropdown is empty after loading**  
  The editor automatically adds any unknown Display Type / Category / Box Size values it finds. You should see a toast when new options are added.

- **3D visualizer looks wrong**  
  Make sure the box size preset matches the size you intend to use in-game. Load your actual `.obj` if you have one — the default blue box is only a placeholder.

- **Grid import matched 0 products**  
  Product IDs in the Custom Product Placement file must exactly match the `ID` fields in your CPL `products.json`.

- **Paths after picking an asset**  
  The picker only inserts a relative path based on the filename. Double-check that the path matches the real folder structure inside your mod pack.

- **Undo history**  
  Every meaningful change takes a snapshot. Large packs with dozens of licenses still work fine; the history is capped at 50 steps.

- **Settings drawer**  
  Currently a placeholder shell. Future options will appear there.

---

## File structure of this project

```
├── Product License Editor-….html   ← the entire application (single file)
└── README.md                       ← this file
```

Everything runs client-side. No server, no install, no accounts. Your data never leaves your machine.

---

## Credits & links

- **Editor author:** [ThaWizanator](https://twitch.tv/thawiznator)
- **Custom Products Loader** by tomcyk91 & Leptoon → [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1589)
- **Custom Product Placement** by FluffyNinjaKitty → [Nexus Mods](https://www.nexusmods.com/supermarketsimulator/mods/1297)
- Built with plain HTML/CSS/JS + [Three.js](https://threejs.org/) (r128) for the 3D visualizer

---

## Disclaimer

This is an unofficial community tool. It is not affiliated with or endorsed by the developers of Supermarket Simulator, Custom Products Loader, or Custom Product Placement. Use at your own risk. Always keep backups of your product packs and save games before replacing files.

Enjoy building custom products!
