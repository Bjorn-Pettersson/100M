# 100M — Algorithmic Emergency Settlement Design

> *Exploring algorithmic design methodology for emergency settlement design.*

With over 101 million people forcibly displaced worldwide (UNHCR, 2022), the need for faster, better-coordinated emergency settlement planning has never been more urgent. This project investigates how the **Wave Function Collapse (WFC) algorithm** can accelerate and improve the generation of emergency settlement layouts — tested against the Azraq refugee settlement in Jordan as a real-world case study.

Produced as part of a Master's in Architecture — Computation in Architecture at the Royal Danish Academy (2021–2022).

---

## Research Question

> *How can the Wave Function Collapse algorithm help speed up, and/or improve the planning of emergency settlements by fast generation of design proposals?*

---

## Repository Contents

| File | Description |
|------|-------------|
| `*.gh` | Grasshopper script — the generative design framework built in Rhino/Grasshopper with custom Python components |
| `*.3dm` | Rhino 3D model — base geometry and site model that the Grasshopper script reads and modifies |

---

## How It Works

The framework is implemented as a Grasshopper definition with a set of custom Python scripting components. The workflow proceeds in the following stages:

### 1. Prepare — Build the Graph
A site boundary and grid (e.g. 100×100 m tiles) are fed into a custom `graph` component. It reads plot lines and corner geometry to produce a node/edge/curve graph that represents the settlement site as a spatial network.

### 2. Input Design Goals
The designer specifies:
- **Function ratios** — the proportional mix of tile types (shelter, communal, service, green space, etc.)
- **Adjacency rules** — how tile types are allowed to relate to one another (e.g. communal tiles must border residential clusters)
- **Probability ratios** — relative likelihood of each tile type being placed

### 3. Design with the Algorithm
Three custom Grasshopper components drive the generative process:

- **`graph` component** — constructs the spatial graph from site geometry
- **`wfc` component** (`in_WFC`, `step`, `step10`, `step_all`, `out_nodes`) — runs the Wave Function Collapse algorithm, collapsing the probability space of the grid step by step until every tile has an assigned function
- **`RESET` component** — allows the designer to uncollapse and re-randomise specific tiles, enabling manual iteration within the algorithmically generated layout

### 4. Apply Humanitarian Standards as Rules
Design principles drawn from the **UNHCR Handbook for Emergencies** and **The Sphere Handbook** are encoded as computational rules:

- Fire corridors: 30 m wide, every 300 m
- Shelter spacing: minimum 2 m apart (ideally 1–3× shelter height)
- Flood avoidance: construction excluded from low-lying areas; these become green/recreational space
- Slope safety: steep terrain avoided; stable foundations required on slopes

These rules constrain the WFC during generation — the algorithm produces layouts that are already compliant with humanitarian standards.

### 5. Render Output
A final render component outputs the completed tile layout as geometry in Rhino, which can be visualised, evaluated, and exported.

---

## Workflow Summary

```
Prepare → Input design goals → Design with algorithm → Get output
   ↓              ↓                      ↓                  ↓
Build graph   Set ratios &        Run / reset WFC      Render in Rhino
              adjacency rules     step by step
```

---

## Case Study: Azraq Settlement, Jordan

The framework was applied to the Azraq settlement — one of the largest UNHCR-managed settlements, used here as a reference for scale, topology, and humanitarian standards. The site geometry (`.3dm`) is derived from Azraq's layout and is the base model the Grasshopper script modifies.

The algorithm generates a **social grid** — a hierarchical community structure at multiple scales — that creates a more varied, human-scale environment compared to the uniform grid of existing emergency settlements.

---

## Design Principles Encoded

The tile ruleset is grounded in four key areas derived from humanitarian standards research:

- **Safety** — fire corridors, flood zones, slope stability
- **Shelter typologies** — dense, medium, and low-density residential tiles
- **Community functions** — markets, schools, health facilities, mosques
- **Green & recreational space** — open areas, trees, transitional buffers

---

## Requirements

- **Rhino 3D** (version 6 or later recommended)
- **Grasshopper** (bundled with Rhino)
- **Python scripting** enabled in Grasshopper (GHPython component)

No additional Grasshopper plugins are required — all WFC logic is implemented in native Python components within the `.gh` file.

---

## Getting Started

1. Open the `.3dm` file in Rhino to load the site geometry.
2. Open the `.gh` file in Grasshopper.
3. Adjust the **function ratios** and **adjacency rules** inputs to reflect your design goals.
4. Use the `step_all` button on the `wfc` component to run the full WFC generation.
5. Use the `RESET` component to selectively uncollapse tiles and iterate on specific areas.
6. Review the rendered output geometry in the Rhino viewport.

---

## Research Context & Collaborators

This project sits at the intersection of two global challenges: the growing displacement crisis driven by climate change and conflict, and the potential for computational design methods to help the architecture industry respond faster and more effectively.

The WFC algorithm — borrowed from procedural generation in game design — is adapted here for rule-based, context-aware urban layout generation. The framework was developed collaboratively at the Royal Danish Academy.

**Collaborators:** Camilla Hedegaard Møller, Josep Cayuelas Mateu, Marja Edén

**Key references:** UNHCR Handbook for Emergencies (3rd ed.), The Sphere Handbook (2018 ed.), UNHCR displacement data (2022)

---

## Potential Extensions

- Integration of AI (LLM-based) rule generation to make the tool faster and easier to configure
- Grid generation tool to handle organic, non-orthogonal site boundaries
- Strategic planning layer incorporating temporality and exit strategies
- Community participation workflows enabled by faster design iteration
