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

**Provider style is auto-detected — don't ask about theme when a provider is named.** If the prompt clearly references AWS, NVIDIA, GCP, or Azure services (e.g. "RAG chatbot on AWS", "NeMo data flywheel", "Vertex AI pipeline", "AKS deployment"), skip the theme question and apply the matching provider's house style — see **Step 2b**. Only confirm when the prompt mixes providers (e.g. "ingest from S3 into Vertex AI") or when the user explicitly asks for a vendor-neutral look.

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

### Step 2b — Provider-Specific Visual Style

When the prompt names a cloud or platform provider, match that provider's reference-architecture house style instead of a generic theme. The goal is that the diagram looks like it came from the provider's own architecture catalog:

| Provider | Canonical reference catalog |
|---|---|
| AWS | [aws.amazon.com/architecture/reference-architecture-diagrams](https://aws.amazon.com/architecture/reference-architecture-diagrams/) |
| NVIDIA | [build.nvidia.com/blueprints](https://build.nvidia.com/blueprints) |
| GCP | [docs.cloud.google.com/architecture/blueprints/security-foundations](https://docs.cloud.google.com/architecture/blueprints/security-foundations) (and the broader [cloud.google.com/architecture](https://cloud.google.com/architecture) center) |
| Azure | [learn.microsoft.com/en-us/azure/architecture/browse](https://learn.microsoft.com/en-us/azure/architecture/browse/) |

**Auto-detection signals (use the first match):**

| Provider | Trigger words / services |
|---|---|
| **AWS** | AWS, S3, Lambda, EC2, VPC, IAM, CloudFront, RDS, DynamoDB, Bedrock, SageMaker, Transit Gateway, Direct Connect, EventBridge, SQS, SNS, Cognito, API Gateway, ECS/EKS/Fargate, Aurora, Redshift, Kinesis, Step Functions |
| **NVIDIA** | NVIDIA, NeMo, NIM, Triton, RAPIDS, Omniverse, BioNeMo, NVIDIA AI Enterprise, NVIDIA Blueprint, DGX, NeMo Microservices, NeMo Guardrails |
| **GCP** | GCP, Google Cloud, GCS, Cloud Run, BigQuery, Vertex AI, GKE, Cloud SQL, Pub/Sub, Cloud Functions, Spanner, Firestore, Dataflow, Dataproc, Cloud Build, Cloud Armor |
| **Azure** | Azure, AKS, Cosmos DB, Azure OpenAI, Azure Functions, Service Bus, Entra ID, App Service, Synapse, Event Grid, Logic Apps, Blob Storage, Front Door, API Management |

If the prompt is multi-provider, ask which provider's style to use, or fall back to the generic Theme System (Step 3) if the user wants vendor-neutral.

**Provider style files:**

| Provider | Reference | Background | Group conventions | Icon style |
|---|---|---|---|---|
| AWS | [references/styles/aws.html](references/styles/aws.html) | white (#fff) | thin black `AWS Region` outer; green-bordered VPCs; dashed teal Availability Zones; orange-dashed account/ASG | square service badges colored by category — purple networking, orange compute, red security, blue/purple database, green storage, teal ML |
| NVIDIA | [references/styles/nvidia.html](references/styles/nvidia.html) | black (#000) | thin white-bordered sections, white uppercase titles centered or top-left, no fill | green isometric service cubes (#76B900) + accent purple/orange/yellow for adapters/datastore/orchestrator; white arrows with white labels |
| GCP | [references/styles/gcp.html](references/styles/gcp.html) | white (#fff) | solid gray-bordered project (with Google Cloud chip top-left); dashed gray region/zone; light-blue VPC | 4-color Material badges using #4285F4 / #EA4335 / #FBBC04 / #34A853, white glyph |
| Azure | [references/styles/azure.html](references/styles/azure.html) | white (#fff) | solid Azure-blue (#0078D4) subscription; dashed blue resource group; solid blue VNet | Fluent multicolor geometric icons; Azure-blue accents, gray (#605E5C) connectors |

**What carries over from Step 2 / Step 4 regardless of provider:**

- Layout selection logic (cloud architecture, sequence, swimlane, etc.)
- Multi-flow arrow conventions when used (numbered primary, lettered secondary, dashed async)
- Step-marker circles/squares at segment midpoints
- Cross-cutting governance/observability rail at the bottom for full architectures
- Embedded SVG legend so the diagram survives download

**What the provider style overrides:**

- Page background, container fills/borders, fonts
- Service icon symbol set and color palette
- Step-marker color (AWS uses orange-amber square; NVIDIA uses green circle; GCP uses blue circle; Azure uses Azure-blue circle)
- The HTML shell's theme switcher is replaced with a non-interactive "Provider style: AWS" badge — do not offer Corporate/Blueprint/Gold theme switching when a provider style is in effect.

If no provider is detected, use the generic Theme System (Step 3).

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
- [themes.html](references/themes.html) — Live theme previews and CSS variable mappings (used when no provider style applies)
- [html-template.html](references/html-template.html) — Interactive HTML shell with theme switcher and download button
- [styles/aws.html](references/styles/aws.html) — AWS reference-architecture style (palette, container conventions, icon symbols, worked example)
- [styles/nvidia.html](references/styles/nvidia.html) — NVIDIA blueprints style (black background, green service cubes)
- [styles/gcp.html](references/styles/gcp.html) — Google Cloud architecture style (4-color Material badges)
- [styles/azure.html](references/styles/azure.html) — Microsoft Azure architecture style (Fluent multicolor icons, Azure-blue accents)
