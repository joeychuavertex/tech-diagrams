---
name: tech-diagrams
description: Generates polished, customizable tech diagrams as SVG or interactive HTML artifacts from plain-English prompts
version: 1.0.0
---

# Tech Diagram Skill

Generates polished, customizable tech diagrams as interactive HTML artifacts (with downloadable SVG) — from a plain-English prompt. Supports swimlane, left-to-right flowchart, top-down flowchart, network/cloud topology, and data flow diagram layouts. Offers Corporate theme by default with easy theme switching (Clean/Minimal, Blueprint, Colorful, Gold/Amber). Output is an interactive HTML artifact with live theme switcher and SVG download button.

If the prompt is vague or missing key details, ask clarifying questions before generating.

## Skill Pipeline

### Step 1 — Prompt Interrogation

Before generating anything, assess whether you have enough information. Ask about any of the following if missing or ambiguous:

| Missing Info | Example Clarifying Question |
|---|---|
| System components / actors | "What are the main components or services involved?" |
| Data or control flow direction | "How do these components communicate — request/response, events, queues?" |
| Layout preference | "Would you like a swimlane, flowchart, network topology, or data flow diagram?" |
| Output preference | "Should I produce a downloadable SVG file, or an interactive in-chat artifact with theme controls?" |
| Theme preference | "Which visual theme — Corporate (blue/grey), Clean/Minimal, Blueprint (dark), Colorful, or Gold/Amber?" |
| Node style | "Would you like plain labelled boxes, or illustrated nodes with SVG icons?" |

**Do not ask about things you can reasonably infer.** If the user says "show me a microservices diagram for an e-commerce app", you can infer components like API Gateway, Order Service, etc. Ask only when genuinely stuck.

### Step 1b — Node Style

Always ask the user whether they want **plain boxes** or **icon nodes** unless they have already specified:

- **Plain boxes (default)** — clean rectangular nodes with bold title + subtitle text only. Fast to generate, easy to edit.
- **Icon nodes** — each node contains a hand-drawn-style SVG icon above its label (image frame, document, bar-chart, database cylinder, robot, snowflake, gear, cloud, phone, etc.). More visual, closer to tools like Eraser.io or Whimsical.


### Step 2 — Layout Selection

Choose the layout that best matches the user's description (or the one they requested):

- **Cloud Architecture Diagram** — cloud services, microservices, distributed systems
- **Sequence Diagram** — time-ordered interactions between actors/services
- **Entity Relationship Diagram** — database schemas, entity connections
- **Swimlane Flowchart** — process flows with parallel lanes (actors/teams)
- **Left-to-Right Flowchart** — linear process or data pipeline (left = input, right = output)
- **Top-Down Flowchart** — hierarchical or step-by-step flows
- **Network/Cloud Topology** — physical/cloud infrastructure layout
- **Data Flow Diagram** — data movement and transformation

See [references/layouts.html](references/layouts.html) for detailed SVG coordinate templates for each layout.

### Step 3 — Theme System

All themes are defined as CSS variable sets injected into the SVG `<defs>`.

| Theme | Background | Node Fill | Node Stroke | Text | Accent |
|---|---|---|---|---|---|
| **Corporate** (default) | #fafafa | #fff / #EEF4FB | #CBD5E1 / #2E75B6 | #111827 | #2E75B6 |
| **Clean/Minimal** | #ffffff | #f9fafb | #e5e7eb | #374151 | #6b7280 |
| **Blueprint** | #0f172a | #1e293b | #3b82f6 | #e2e8f0 | #60a5fa |
| **Colorful** | #F5F2ED | #E8E4F3 / #DCEEE0 | #A89BC9 / #7FB892 | #4A3F7A | #D89B7A |
| **Gold/Amber** | #000000 | #fef3c7 | #daa520 | #5c4620 | #d97706 |

See [references/themes.html](references/themes.html) for the full variable list and live theme preview.

### Step 4 — SVG Generation Rules

**Universal rules (apply to all layouts):**

- **viewBox** should be auto-calculated to fit content with 40px padding on all sides
- **Default canvas sizes:**
  - 1100×720 for swimlane/LR
  - 800×900 for top-down
  - 1000×700 for network/DFD
  - 1200×900 for sequence diagrams
- **Font:** `ui-sans-serif, system-ui, -apple-system, sans-serif`
- **Arrow markers** defined in `<defs>` — use `id="arrow"` (solid) and `id="arrowDashed"` (tool call / return)
- **All colours** via CSS variables (never hardcoded hex in shape attributes)
- **Every node has:**
  - Bold title (14px, font-weight:700)
  - Optional subtitle (11px, muted)
- **Node minimum sizes:**
  - 140×60 for simple boxes
  - 160×80 for agent/service nodes
  - 90px height for icon nodes
- **Include a legend** at the bottom-left showing arrow types used
- **Stroke width:** 1.5–2px for primary elements, 1–1.5px for secondary
- **Rounded corners:** 6–8px for modern appearance
- **Text alignment:** center for titles, left-aligned for longer labels
- **Spacing:** 60–100px between nodes, 40–60px for lane separators

### Step 5 — Output Delivery

#### Interactive HTML Artifact

1. Wrap the SVG inside the HTML interactive shell (see [references/html-template.html](references/html-template.html))
2. Include:
   - **Theme switcher** (Corporate / Clean / Blueprint / Colorful)
   - **"Download SVG" button** that triggers `<a download>` with the SVG blob
   - **Optional:** node click → highlight + info panel
3. Render with artifact widget or write to `/mnt/user-data/outputs/<diagram-name>.html`

#### SVG File Output

If user prefers standalone SVG, generate the raw SVG file without HTML wrapper.

**Ask if not specified:** "Would you like this as a downloadable SVG file, or as an interactive in-chat diagram with theme controls and a download button?"

### Step 6 — Iteration

After delivery, invite the user to refine:
- "Want to switch the theme, rename any nodes, or add/remove a lane?"
- Apply changes to the existing SVG/HTML — do not regenerate from scratch unless the structure changes fundamentally
- For SVG edits, use surgical text replacement to update labels, colours, or paths

---

## Reference Files

- [layouts.html](references/layouts.html) — Visual templates for each layout type with coordinate systems
- [themes.html](references/themes.html) — Live theme previews and CSS variable mappings
- [html-template.html](references/html-template.html) — Interactive HTML shell with theme switcher and download button
