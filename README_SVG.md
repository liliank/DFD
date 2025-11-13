# Generate SVG from Dataflow Diagram

This repository contains the Omron Doctor Dashboard dataflow diagram in Mermaid format. To generate an SVG file, use one of the methods below.

## Quick Start (Recommended)

### Method 1: Mermaid Live Editor (Easiest - 30 seconds)

1. Visit **[https://mermaid.live/](https://mermaid.live/)**
2. Delete the example code in the editor
3. Copy and paste the entire contents of `dataflow.mmd` into the editor
4. The diagram will render automatically
5. Click the **"Download"** button (download icon in top-right)
6. Select **"SVG"** from the dropdown menu
7. Save as `omron-dataflow-diagram.svg`

✅ **This produces the highest quality SVG with perfect rendering**

### Method 2: View HTML File (Interactive)

1. Download `dataflow-diagram.html` from this repository
2. Open it in Chrome, Firefox, or Edge
3. The diagram renders automatically with full interactivity
4. To export to SVG:
   - Right-click on the diagram → "Inspect Element"
   - Find the `<svg>` tag in the developer tools
   - Right-click the `<svg>` tag → Copy → Copy outerHTML
   - Paste into a text editor
   - Save as `diagram.svg`

## Advanced Methods

### Method 3: Mermaid CLI (Requires Node.js)

```bash
# Install mermaid-cli globally
npm install -g @mermaid-js/mermaid-cli

# Generate SVG
mmdc -i dataflow.mmd -o dataflow-diagram.svg -b transparent

# Optional: Generate PNG
mmdc -i dataflow.mmd -o dataflow-diagram.png -b transparent
```

### Method 4: Draw.io / diagrams.net

1. Go to **[https://app.diagrams.net/](https://app.diagrams.net/)**
2. Click **"Create New Diagram"**
3. In the menu: **Arrange** → **Insert** → **Advanced** → **Mermaid**
4. Paste the contents of `dataflow.mmd`
5. Click **"Insert"**
6. Export: **File** → **Export as** → **SVG**

### Method 5: VS Code Extension

If you use Visual Studio Code:

1. Install extension: **"Markdown Preview Mermaid Support"**
2. Open `DATAFLOW_DIAGRAM.md` in VS Code
3. Open Preview (Ctrl+Shift+V or Cmd+Shift+V)
4. The Mermaid diagram renders in the preview
5. Right-click diagram → Inspect → Copy SVG element

### Method 6: GitHub Rendering

GitHub automatically renders Mermaid diagrams in Markdown files:

1. View `DATAFLOW_DIAGRAM.md` on GitHub
2. The diagram renders automatically
3. Use browser developer tools to extract the SVG:
   - Right-click diagram → Inspect
   - Copy the `<svg>` element
   - Save to file

## Online Alternatives

### Kroki.io API

```bash
# Encode and download
cat dataflow.mmd | gzip | base64 -w 0 | xargs -I {} curl "https://kroki.io/mermaid/svg/{}" -o diagram.svg
```

### Mermaid.ink API

```bash
# Encode and download
cat dataflow.mmd | base64 -w 0 | xargs -I {} curl "https://mermaid.ink/svg/{}" -o diagram.svg
```

## Files in This Repository

- **`DATAFLOW_DIAGRAM.md`** - Complete documentation with embedded Mermaid diagram
- **`dataflow.mmd`** - Raw Mermaid source code (use this for conversions)
- **`dataflow-diagram.html`** - Interactive HTML viewer
- **`SVG_EXPORT_GUIDE.md`** - Detailed export instructions
- **`render-mermaid.html`** - Simplified HTML renderer

## Troubleshooting

**Q: The SVG doesn't render in my image viewer**
- Try opening in a web browser instead (Chrome, Firefox, Safari)
- Some image viewers don't support complex SVG features
- Use the Mermaid Live Editor method for best compatibility

**Q: The diagram is too large/small**
- SVG files are scalable - adjust the viewBox attribute
- In Mermaid CLI, use `--width` and `--height` parameters
- In browsers, use CSS to scale: `<img src="diagram.svg" style="width: 100%">`

**Q: Colors don't match the original**
- The Mermaid diagram includes color classes
- Ensure you're using the full diagram from `dataflow.mmd`
- Different renderers may apply slightly different default colors

## Diagram Overview

The dataflow diagram visualizes:

- **User Layer** - Doctors and native mobile apps
- **Frontend Layer** - React Framework7 screens and components
- **Service Layer** - Authentication, API, Network, Cache services
- **Backend API Layer** - 8 different API services
- **Data Storage** - Database layer
- **External Services** - Google Analytics, Pusher WebSocket, Omron devices

**Total Nodes:** 40+ components
**Total Connections:** 60+ data flows
**Color Coded:** 6 architectural layers

## Recommended SVG Settings

When exporting, use these settings for best results:

- **Format:** SVG
- **Background:** Transparent or White
- **Width:** 2000-3000px (scales automatically)
- **Include:** All CSS styles and classes
- **Text:** Convert to paths (for font compatibility)

---

**Last Updated:** 2025-11-13
**Mermaid Version:** Compatible with Mermaid 10.x+
**Source:** Based on Omron Doctor Dashboard staging branch
