# How to Generate SVG from the Dataflow Diagram

I've created the Mermaid diagram and prepared several files to help you export it to SVG format. Due to environment limitations (no browser, external APIs blocked), I've created HTML files that you can use locally.

## Available Files

Located in `/tmp/`:

1. **`dataflow-diagram.html`** - Standalone HTML file with the diagram (recommended)
2. **`render-mermaid.html`** - Simplified HTML for browser export
3. **`dataflow.mmd`** - Raw Mermaid diagram code

## Method 1: Browser Export (Easiest)

### Steps:

1. **Download the HTML file:**
   - Location: `/tmp/dataflow-diagram.html`
   - Or download from your GitHub: https://github.com/liliank/DFD

2. **Open in a web browser:**
   - Double-click the HTML file, or
   - Open Chrome/Firefox and drag the file into the browser

3. **Export the SVG:**

   **Option A - Using Browser Developer Tools:**
   - Right-click on the rendered diagram
   - Select "Inspect" or "Inspect Element"
   - In the DevTools, find the `<svg>` element
   - Right-click on the `<svg>` tag
   - Select "Copy" → "Copy outerHTML"
   - Paste into a text editor
   - Save as `dataflow-diagram.svg`

   **Option B - Using Browser Extension:**
   - Install an SVG export extension (e.g., "SVG Export" for Chrome)
   - Right-click the diagram
   - Select "Save as SVG"

   **Option C - Screenshot (PNG, not SVG):**
   - Use browser screenshot tools
   - Or use Snipping Tool / Screenshot app

## Method 2: Mermaid CLI (If you have Node.js)

```bash
# Install mermaid-cli globally
npm install -g @mermaid-js/mermaid-cli

# Convert to SVG
mmdc -i /tmp/dataflow.mmd -o dataflow-diagram.svg
```

## Method 3: Online Mermaid Editors

### Option A - Mermaid Live Editor:

1. Go to: https://mermaid.live/
2. Paste the contents of `/tmp/dataflow.mmd`
3. Click the "Download" button
4. Select "SVG" format

### Option B - Draw.io (diagrams.net):

1. Go to: https://app.diagrams.net/
2. File → Import → Select "Mermaid"
3. Paste the contents of `/tmp/dataflow.mmd`
4. File → Export as → SVG

### Option C - GitHub (Automatic Rendering):

Your diagram is already on GitHub at:
```
https://github.com/liliank/DFD/blob/claude/draw-dataflow-diagram-011CV5e4V72LkDjKQFmauxVC/DATAFLOW_DIAGRAM.md
```

GitHub renders Mermaid automatically. To export:
1. View the diagram on GitHub
2. Use browser tools to inspect and copy the SVG element

## Method 4: VS Code Extension

If you use Visual Studio Code:

1. Install the "Markdown Preview Mermaid Support" extension
2. Open `/tmp/DATAFLOW_DIAGRAM.md` in VS Code
3. Open Markdown Preview (Ctrl+Shift+V)
4. Right-click on the rendered diagram
5. Inspect and copy the SVG element

## Method 5: Command Line with Docker (Advanced)

If you have Docker installed:

```bash
# Pull mermaid-cli docker image
docker pull minlag/mermaid-cli

# Generate SVG
docker run --rm -v /tmp:/data minlag/mermaid-cli -i /data/dataflow.mmd -o /data/dataflow-diagram.svg
```

## Method 6: Python Script with Playwright

If you have Python and Playwright:

```python
from playwright.sync_api import sync_playwright
import time

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto('file:///tmp/dataflow-diagram.html')
    time.sleep(3)  # Wait for rendering
    svg = page.query_selector('svg')
    svg_content = svg.inner_html()
    with open('dataflow-diagram.svg', 'w') as f:
        f.write(f'<svg>{svg_content}</svg>')
    browser.close()
```

## Recommended Approach

**For quickest results:**
1. Download `/tmp/dataflow-diagram.html`
2. Open in Chrome or Firefox
3. Use the browser DevTools method described in Method 1

**For best quality:**
1. Go to https://mermaid.live/
2. Paste contents from `/tmp/dataflow.mmd`
3. Download as SVG

## File Locations Summary

```
/tmp/dataflow-diagram.html       - Standalone HTML with instructions (recommended)
/tmp/render-mermaid.html         - Simplified HTML for export
/tmp/dataflow.mmd                - Raw Mermaid code
/tmp/DATAFLOW_DIAGRAM.md         - Full documentation with diagram
```

## Diagram Features

The generated SVG will include:
- **Color-coded layers:** User (blue), Frontend (orange), Service (purple), Backend (green), Storage (pink), External (yellow)
- **All connections and data flows**
- **Styled subgraphs** for architectural layers
- **Scalable vector format** - can be resized without quality loss

## Need Help?

If you encounter issues:
1. Ensure JavaScript is enabled in your browser
2. Try a different browser (Chrome works best)
3. Check that the HTML file downloaded completely
4. Use the Mermaid Live Editor as a fallback

---

**Note:** The diagram is complex with many nodes and connections. The SVG file will be approximately 50-100KB in size.
