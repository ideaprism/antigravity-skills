---
description: Create, edit, and analyze PowerPoint presentations (.pptx). Supports creating from scratch (HTML->PPTX), working with templates, and conducting editing via OOXML.
---

# PPTX Creation, Editing, and Analysis

## 1. Capabilities & Workflow Selection

Choose the appropriate workflow based on your goal:

| Goal | Workflow | Key Tools |
|------|----------|-----------|
| **Research & Plan** | `PLANNING` mode | `browser_subagent`, `search_web` |
| **Create from Scratch** | `html2pptx` | `html2pptx.js`, `generate_image`, `run_command` |
| **Use Template** | `rearrange` + `replace` | `rearrange.py`, `replace.py`, `inventory.py` |
| **Edit Existing** | `OOXML` | `unpack.py`, `pack.py`, `validate.py` |
| **Analyze Content** | Extract text | `markitdown` |

## 2. Environment Setup

Before starting, ensure dependencies are installed.
// turbo
Run this command to check/install tools:
```bash
# Verify essential tools
npm list -g pptxgenjs playwright react-icons sharp || npm install -g pptxgenjs playwright react-icons sharp
pip show markitdown defusedxml python-pptx || pip install "markitdown[pptx]" defusedxml python-pptx
```

## 3. Planning & Research (Enhancement)

**Unique to Antigravity**: Use browser tools to ensure content accuracy and design quality.

- **Content Research**: usage `browser_subagent` to find latest statistics, quotes, or detailed information for the slides.
- **Design Inspiration**: Search for "modern presentation trends [topic]" to inform color/layout choices.
- **Asset Generation**: Use `generate_image` to create unique backgrounds or illustrations.

## 4. Workflows

### A. Reading & Analysis

To read the text content of a presentation:
```bash
python -m markitdown /absolute/path/to/file.pptx
```

To inspect slide layouts visually (create thumbnails):
```bash
python {workspace_root}/.agent/resources/pptx/scripts/thumbnail.py /absolute/path/to/file.pptx {workspace_root}/thumbnails
```

### B. Creating from Scratch (html2pptx)

**Context**: detailed guide at `references/html2pptx.md`.
Use `view_file {workspace_root}/.agent/resources/pptx/references/html2pptx.md` to read it.

1.  **Draft Content**: Create HTML files for each slide using the specific structure required.
    *   *Tip*: Use `generate_image` to create assets, then reference them in HTML.
2.  **Convert**: Run the node script using the resource library.
    ```bash
    # You must create a small JS script that imports from the resource path
    # const { html2pptx } = require('{workspace_root}/.agent/resources/pptx/scripts/html2pptx.js');
    ```

### C. Using a Template

**Context**: Detailed steps for `inventory`, `rearrange`, and `replace`.

1.  **Inventory**: Extract template structure.
    ```bash
    python {workspace_root}/.agent/resources/pptx/scripts/inventory.py /path/to/template.pptx inventory.json
    ```
2.  **Rearrange**: Create a working deck with correct slide count/order.
    ```bash
    python {workspace_root}/.agent/resources/pptx/scripts/rearrange.py /path/to/template.pptx working.pptx 0,1,1,2
    ```
3.  **Replace**: Inject content into placeholders.
    - Create `replacements.json` (refer to `inventory.json` for shape IDs).
    - Run replacement:
    ```bash
    python {workspace_root}/.agent/resources/pptx/scripts/replace.py working.pptx replacements.json final_output.pptx
    ```

### D. Editing Existing (OOXML)

**Context**: Detailed guide at `references/ooxml.md`.
Use `view_file {workspace_root}/.agent/resources/pptx/references/ooxml.md`.

1.  **Unpack**:
    ```bash
    python {workspace_root}/.agent/resources/pptx/ooxml/scripts/unpack.py /path/to/file.pptx {workspace_root}/temp_ooxml
    ```
2.  **Edit**: Modify XML files in `{workspace_root}/temp_ooxml`.
3.  **Validate**:
    ```bash
    python {workspace_root}/.agent/resources/pptx/ooxml/scripts/validate.py {workspace_root}/temp_ooxml --original /path/to/file.pptx
    ```
4.  **Pack**:
    ```bash
    python {workspace_root}/.agent/resources/pptx/ooxml/scripts/pack.py {workspace_root}/temp_ooxml /path/to/new_file.pptx
    ```

## 5. Design Guidelines

- **Colors**: Define a `theme_palette` in your plan.
- **Fonts**: Use web-safe fonts (Arial, Roboto, Helvetica).
- **Images**: Always use absolute paths for images embedded in slides.

## 6. Verification

After creating the PPTX:
1.  Run `thumbnail.py` to generate visual previews.
2.  Use `generate_image` (edit mode) or similar if you need to fix visual assets.
3.  Present `walkthrough.md` with the generated thumbnails to the user.
