# Draw.io Best Practices for FPGA/HLS Diagrams

Layout guidelines and FPGA conventions for Draw.io (mxGraph XML) diagrams.
Adapted from the Excalidraw best-practices for the MATLAB2HLS framework.

---

## 1. Layout Principles

### Grid Alignment

- Use a 10px grid. Position all element x, y, width, and height values on
  multiples of 10.
- Consistent alignment prevents visual drift when diagrams are edited manually
  or regenerated programmatically.

```xml
<!-- CORRECT: coordinates on 10px grid -->
<mxCell style="..." vertex="1" parent="1">
  <mxGeometry x="80" y="150" width="200" height="120" as="geometry"/>
</mxCell>

<!-- WRONG: off-grid coordinates -->
<mxCell style="..." vertex="1" parent="1">
  <mxGeometry x="83" y="147" width="195" height="117" as="geometry"/>
</mxCell>
```

### Spacing

| Context | Spacing |
|---------|---------|
| Between shapes (horizontal) | 40-60px |
| Between shapes (vertical) | 40-60px |
| Within groups (internal padding) | 20-30px |
| Between pipeline stages | 60-80px |
| Edge labels from arrow midpoint | 10-15px offset |

### Visual Hierarchy

- **Size communicates importance**: top-level modules are larger rectangles
  (200x120), internal sub-blocks are smaller (140x80), annotations are compact.
- **Color communicates function**: see Section 2.
- **Grouping communicates ownership**: use container cells (swimlanes or
  rounded rectangles with `container=1`) to show module boundaries.

### Flow Direction

| Diagram Type | Primary Flow |
|--------------|-------------|
| System topology / datapath | Left to right |
| Module hierarchy / decomposition | Top to bottom |
| FSM state transitions | Top to bottom with loops |
| DATAFLOW stages | Left to right |

---

## 2. Color Usage

### The 60-30-10 Rule

- **60%** - Neutral backgrounds (white, light gray). Canvas and containers.
- **30%** - Semantic fills (blue, amber, green). Functional blocks.
- **10%** - Accent strokes and highlights. Critical paths, warnings.

### Semantic Color Palette

Use these fill/stroke pairs consistently across all FPGA diagrams:

| Meaning | Fill | Stroke | Usage |
|---------|------|--------|-------|
| DSP / Computation | `#dbeafe` | `#1971c2` | Multipliers, accumulators, ALU blocks |
| Memory / Storage | `#e0e7ff` | `#4f46e5` | BRAM, registers, FIFOs, circular buffers |
| Control / FSM | `#fef3c7` | `#d97706` | State machines, control logic, MUX select |
| FIR IP | `#fef9c3` | `#ca8a04` | hls::FIR instances, filter blocks |
| Data I/O | `#d1fae5` | `#059669` | Stream inputs/outputs, port interfaces |
| Control I/O | `#fce7f3` | `#db2777` | Async control signals, detect flags |
| Accumulator | `#99e9f2` | `#0c8599` | Running sums, energy metrics |
| Pipeline Register | `#e9ecef` | `#868e96` | Stage boundaries, delay elements |

### Style String Examples

```
<!-- DSP block -->
rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;strokeWidth=2;

<!-- Memory block -->
rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;strokeWidth=2;

<!-- Control/FSM block -->
rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;strokeWidth=2;

<!-- Data I/O ellipse -->
ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;strokeWidth=2;
```

---

## 3. Typography

### Font Sizes

| Element | Font Size | Weight |
|---------|-----------|--------|
| Diagram title | 16-20px | Bold |
| Subtitle / description | 12px | Italic, gray |
| Block labels | 11-12px | Bold |
| Port / signal labels | 10px | Regular |
| Annotations / bit-widths | 9-10px | Regular |
| Legend entries | 10px | Regular |

### HTML Labels

Draw.io supports HTML inside `value` attributes when `html=1` is in the style.
Use this for rich formatting:

```xml
<!-- Bold title with subtitle -->
<mxCell value="&lt;b&gt;Module 1&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px;color:#868e96&quot;&gt;packet_detect&lt;/font&gt;"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;"
        vertex="1" parent="1">
  <mxGeometry x="80" y="150" width="200" height="80" as="geometry"/>
</mxCell>

<!-- Monospace code label -->
<mxCell value="&lt;font face=&quot;Courier New&quot;&gt;hls::stream&amp;lt;data_t&amp;gt;&lt;/font&gt;"
        style="text;html=1;align=center;verticalAlign=middle;"
        vertex="1" parent="1">
  <mxGeometry x="300" y="180" width="140" height="20" as="geometry"/>
</mxCell>
```

### HTML Entity Reference

| Character | Entity |
|-----------|--------|
| `<` | `&lt;` |
| `>` | `&gt;` |
| `&` | `&amp;` |
| `"` (inside attribute) | `&quot;` |
| Line break | `&lt;br&gt;` |

### Monospace for Code

Use `Courier New` for:
- Type names (`data_t`, `complex_t`, `ap_fixed<18,4>`)
- Stream names (`data_in`, `ctrl_out`)
- Pragma annotations (`II=1`, `PIPELINE`)
- Bit-width labels (`[17:0]`, `18b`)

---

## 4. Arrow Conventions for Hardware

### Signal Types

| Signal Type | Style Properties | Visual |
|-------------|-----------------|--------|
| Data bus | `strokeColor=#1971c2;strokeWidth=2;` | Solid blue, medium weight |
| Control signal | `strokeColor=#e03131;strokeWidth=1.5;dashed=1;dashPattern=8 4;` | Dashed red |
| Memory access | `strokeColor=#4f46e5;strokeWidth=1.5;dashed=1;dashPattern=8 4;` | Dashed indigo |
| Feedback path | `strokeColor=#868e96;strokeWidth=1;dashed=1;dashPattern=4 4;` | Dotted gray |
| Coefficient feed | `strokeColor=#1971c2;strokeWidth=1.5;dashed=1;dashPattern=8 4;` | Dashed blue, thin |

### Arrow Endpoint Styles

| Context | Source End | Target End |
|---------|-----------|------------|
| Data flow | None (`startFill=0`) | Filled arrow (`endArrow=block;endFill=1`) |
| Bidirectional | Open arrow | Open arrow |
| Control broadcast | None | Open arrow (`endFill=0`) |
| hls::stream | None | Filled classic (`endArrow=classic;endFill=1`) |

### Edge Routing

- Use `edgeStyle=orthogonalEdgeStyle;` for clean right-angle routing in
  block diagrams.
- Use `curved=1;` for FSM state transition arcs.
- Avoid diagonal edges in block diagrams - they suggest unstructured wiring.
- Add waypoints for edges that would otherwise cross unrelated blocks.

```xml
<!-- Data bus edge -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;strokeColor=#1971c2;strokeWidth=2;
               endArrow=block;endFill=1;html=1;"
        edge="1" source="m1" target="m2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- Control signal edge -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;strokeColor=#e03131;strokeWidth=1.5;
               dashed=1;dashPattern=8 4;endArrow=classic;endFill=0;html=1;"
        edge="1" source="ctrl_src" target="ctrl_dst" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Bit-Width Annotations

Place bit-width labels on edges using `mxCell` with `style="text;..."`:

```xml
<mxCell value="32b" style="text;html=1;fontSize=9;align=center;fontColor=#868e96;"
        vertex="1" parent="1">
  <mxGeometry x="250" y="165" width="30" height="15" as="geometry"/>
</mxCell>
```

Alternatively, use the edge's `value` attribute for inline labels:

```xml
<mxCell value="&lt;font style=&quot;font-size:9px&quot;&gt;32b&lt;/font&gt;"
        style="edgeStyle=orthogonalEdgeStyle;strokeColor=#1971c2;strokeWidth=2;
               endArrow=block;endFill=1;html=1;fontSize=9;"
        edge="1" source="m1" target="m2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

---

## 5. FPGA Micro-Architecture Layout

### Spatial Organization

```
+-----------------------------------------------------------------------+
|  CONTROL LOGIC (FSM, MUX select, enables)          [amber fill]       |
+-----------------------------------------------------------------------+
|                                                                       |
|  [I/O]  -->  [STAGE 1]  | REG |  [STAGE 2]  | REG |  [STAGE 3]  --> [I/O]
|  port        compute     pipe     compute     pipe     compute        port
|  (ellipse)   (rect)      (thin)   (rect)      (thin)  (rect)         (ellipse)
|                                                                       |
+-----------------------------------------------------------------------+
|  STORAGE (BRAM, shift regs, circular buffers)      [indigo fill]      |
+-----------------------------------------------------------------------+
```

### Layout Rules

1. **Data flow left-to-right**: Input ports on the left edge, output ports on
   the right edge. Main datapath runs horizontally through the center.

2. **I/O ports as ellipses**: Use `ellipse` shape with Data I/O colors
   (`#d1fae5` / `#059669`) for stream ports. Control I/O uses
   (`#fce7f3` / `#db2777`).

3. **Pipeline stage separators**: Thin vertical rectangles (width=10,
   height=full stage) with Pipeline Register colors (`#e9ecef` / `#868e96`).
   Label with `REG` or cycle count.

4. **Storage row below datapath**: BRAM blocks, shift registers, and circular
   buffers sit below the main datapath with vertical dashed arrows connecting
   to their consumers.

5. **Control logic above or below**: FSM blocks, multiplexer select logic,
   and enable signals above the datapath (preferred) or below if space
   requires. Use amber fill (`#fef3c7` / `#d97706`).

6. **Bit-width on every inter-stage arrow**: Annotate data width on arrows
   between pipeline stages. Use 9px gray text.

### Container Cells for Stages

Use `container=1` and `collapsible=0` for stage grouping:

```xml
<mxCell value="EXTRACT Stage" style="rounded=1;whiteSpace=wrap;html=1;
               container=1;collapsible=0;fillColor=#f8f9fa;strokeColor=#868e96;
               strokeWidth=1;dashed=1;verticalAlign=top;fontStyle=1;fontSize=11;"
        vertex="1" parent="1">
  <mxGeometry x="80" y="100" width="280" height="250" as="geometry"/>
</mxCell>
```

---

## 6. Title and Subtitle Convention

### Title Block

Place the title as a standalone text cell at the top of the diagram:

```xml
<!-- Title -->
<mxCell value="&lt;b&gt;Module 1: Packet Detect - Micro-Architecture&lt;/b&gt;"
        style="text;html=1;fontSize=18;fontStyle=1;align=left;verticalAlign=top;
               strokeColor=none;fillColor=none;"
        vertex="1" parent="1">
  <mxGeometry x="20" y="20" width="500" height="30" as="geometry"/>
</mxCell>

<!-- Subtitle -->
<mxCell value="&lt;i&gt;&lt;font color=&quot;#868e96&quot;&gt;Phase 6 - Fixed-Point, II=1, xc7z020&lt;/font&gt;&lt;/i&gt;"
        style="text;html=1;fontSize=12;fontStyle=2;align=left;verticalAlign=top;
               strokeColor=none;fillColor=none;"
        vertex="1" parent="1">
  <mxGeometry x="20" y="50" width="400" height="20" as="geometry"/>
</mxCell>
```

### Resource Summary Box

Place at the bottom-right corner of the diagram:

```xml
<mxCell value="&lt;b&gt;Resources (CSYNTH)&lt;/b&gt;&lt;br&gt;DSP: 11 / 176 (6.3%)&lt;br&gt;BRAM: 2 / 140 (1.4%)&lt;br&gt;LUT: 528 / 26600 (2.0%)&lt;br&gt;FF: 688 / 53200 (1.3%)&lt;br&gt;II: 1&lt;br&gt;Latency: 26105 cycles"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#868e96;
               strokeWidth=1;fontSize=10;align=left;verticalAlign=top;
               spacingLeft=10;spacingTop=5;"
        vertex="1" parent="1">
  <mxGeometry x="600" y="400" width="180" height="130" as="geometry"/>
</mxCell>
```

### Legend Box

Place at the bottom-left, showing color/line-style meanings:

```xml
<mxCell value="&lt;b&gt;Legend&lt;/b&gt;&lt;br&gt;&lt;font color=&quot;#1971c2&quot;&gt;--- Data bus (solid blue)&lt;/font&gt;&lt;br&gt;&lt;font color=&quot;#e03131&quot;&gt;- - Control (dashed red)&lt;/font&gt;&lt;br&gt;&lt;font color=&quot;#4f46e5&quot;&gt;- - Memory (dashed indigo)&lt;/font&gt;"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#dee2e6;
               strokeWidth=1;fontSize=10;align=left;verticalAlign=top;
               spacingLeft=10;spacingTop=5;"
        vertex="1" parent="1">
  <mxGeometry x="20" y="400" width="240" height="100" as="geometry"/>
</mxCell>
```

---

## 7. Common Mistakes to Avoid

### XML Structure Errors

| Mistake | Symptom | Fix |
|---------|---------|-----|
| Missing `html=1;` in style | HTML tags render as literal text | Add `html=1;` to every cell with HTML values |
| Missing `parent="1"` | Cell not visible in diagram | Every user cell must have `parent="1"` (or a container ID) |
| Compressed/minified XML | Unreadable, impossible to diff | Pretty-print with 2-space indentation |
| Missing `vertex="1"` | Shape not rendered | All shape cells need `vertex="1"` |
| Missing `edge="1"` | Arrow not rendered | All edge cells need `edge="1"` |
| Style not ending with `;` | Last property silently ignored | Always terminate style strings with `;` |
| Duplicate cell IDs | Unpredictable rendering, lost elements | Use unique IDs - prefix with module/stage name |
| Missing root cells | Diagram fails to load | Always include cells with `id="0"` and `id="1"` |

### Root Cell Template

Every Draw.io XML must start with these two root cells:

```xml
<mxGraphModel>
  <root>
    <mxCell id="0"/>
    <mxCell id="1" parent="0"/>
    <!-- All diagram cells go here with parent="1" -->
  </root>
</mxGraphModel>
```

### Layout Errors

| Mistake | Symptom | Fix |
|---------|---------|-----|
| Z-order: blocks before containers | Blocks hidden behind containers | Place container cells BEFORE their children in XML |
| Edge without `source`/`target` | Disconnected floating arrow | Set `source` and `target` to valid cell IDs |
| Overlapping text labels | Unreadable text clutter | Offset labels by 15-20px, use `labelPosition` |
| Arrow labels on top of shapes | Label obscured by fill | Use `edgeLabel` or offset the label geometry |
| Missing `as="geometry"` | Geometry not applied | `<mxGeometry ... as="geometry"/>` is required |
| Container without `container=1` | Child cells not grouped | Add `container=1;` to parent style |
| Text outside visible canvas | Content clipped or invisible | Keep all elements within positive x,y coordinates |

### Style String Hygiene

```
CORRECT:  rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;
WRONG:    rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2
                                                                                ^-- missing trailing semicolon

CORRECT:  fontSize=12;fontStyle=1;
WRONG:    font-size=12;font-style=1;
                                   ^-- CSS syntax, not mxGraph syntax
```

### Entity Encoding in Values

When the `value` attribute contains HTML, all special characters must be
XML-entity-encoded at the attribute level:

```xml
<!-- CORRECT: double-encoded for attribute context -->
value="&lt;b&gt;hls::stream&amp;lt;data_t&amp;gt;&lt;/b&gt;"

<!-- WRONG: single-encoded breaks XML parsing -->
value="<b>hls::stream&lt;data_t&gt;</b>"
```

The outer `&lt;` / `&gt;` encode the HTML `<b>` tags for the XML attribute.
The inner `&amp;lt;` / `&amp;gt;` encode the literal `<` / `>` characters
that should appear in the rendered text.

---

## 8. Diagram-Type Patterns

### System Topology

Layout for Phase 1 / Phase 7 system block diagrams showing module interconnections.

- **Modules as rounded rectangles** (200x120), arranged left-to-right in
  processing order.
- **Splitter nodes** as small diamonds (40x40) where scalar control fans out.
- **FIFO connections** annotated with depth on the edge label.
- **Input/output ports** as ellipses at the left and right edges.
- **Vertical stacking** for parallel branches: if Module A feeds both Module B
  and Module C, stack B and C vertically with a splitter between A and the pair.
- Container rectangle around the entire system, labeled with algorithm name.

```
[in] --> [M0: Prep] --> [M1: Detect] --data--> [M2: Correct]
                              |                      |
                              +--ctrl(splitter)------+---> [M3: Sync]
                                                              |
                                                          [M4: Extract] --> [out]
```

### DATAFLOW Decomposition

Layout for modules with internal DATAFLOW (CSR > 2).

- **Three or four stage columns** left-to-right: EXTRACT, COMPUTE, APPLY
  (optionally split APPLY into BUFFER_FORWARD + APPLY_CORRECTION).
- Each stage is a **dashed container** with the stage name as header.
- Internal FIFOs between stages shown as thin rounded rectangles with
  indigo fill, labeled with depth.
- FSM state machine inside EXTRACT stage if applicable.
- Circular buffer shown as a cylinder shape inside EXTRACT.

### FSM State Diagram

Layout for modules with `@FSM: YES`.

- **States as rounded rectangles** with amber fill, arranged top-to-bottom
  or in a grid if many states.
- **Transitions as curved edges** with labels showing the condition.
- **Self-loops** as curved edges returning to the same state, label offset
  above or to the side.
- **Initial state** indicated by a small filled circle with an arrow to the
  first state.
- **Terminal state** (DONE) at the bottom with double-border styling
  (`strokeWidth=3`).
- State names in bold, transition conditions in regular weight, actions in
  italic below the condition.

```xml
<!-- FSM state -->
<mxCell value="&lt;b&gt;CHECK&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;poll ctrl_in&lt;/font&gt;"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;
               strokeWidth=2;arcSize=20;"
        vertex="1" parent="1">
  <mxGeometry x="200" y="150" width="140" height="60" as="geometry"/>
</mxCell>
```

### RTL Micro-Architecture

Layout for Phase 6 detailed pipeline views.

- Follow the spatial organization from Section 5.
- **One column per pipeline stage**, separated by register elements.
- **Multipliers** shown as trapezoids or labeled rectangles with DSP fill.
- **Adders** shown as circles with `+` label.
- **Multiplexers** shown as trapezoids with control arrow from above.
- **Registers** shown as thin rectangles (10x60) with gray fill.
- **BRAM** shown as cylinders with indigo fill, capacity annotated.
- **Bit-widths on every wire**: `18b`, `36b`, `[17:0]` format.
- **Clock domain boundaries** (if any) shown as dashed vertical lines
  spanning the full diagram height.

---

## 9. Edge Overlap Prevention

### Minimum Clearance Rule

Parallel edge segments on the same axis must be >= 30px apart. When two edges
share a vertical corridor (same x-coordinate) or horizontal corridor (same
y-coordinate), offset one edge's waypoints by at least 30px.

```
OVERLAP (15px gap):           FIXED (30px gap):
  x=960 |  | x=960             x=930 |     | x=960
        |  |                         |     |
        |  |                         |     |
```

### Shared-Target Fan-In Rule

When multiple edges converge on the same target shape, assign distinct entry
corridors. Each edge must approach from a unique x (for vertical entry) or
unique y (for horizontal entry), with >= 30px between corridors.

```xml
<!-- BAD: Two edges both route through x=960 to reach the same target -->
<mxPoint x="960" y="455" />  <!-- edge A waypoint -->
<mxPoint x="960" y="470" />  <!-- edge B waypoint -->

<!-- GOOD: Offset edge A to x=930 for 30px clearance -->
<mxPoint x="930" y="455" />  <!-- edge A waypoint -->
<mxPoint x="960" y="470" />  <!-- edge B waypoint -->
```

### Shared-Source Fan-Out Rule

Same principle for multiple edges leaving one source. Each exit path must use
a distinct corridor with >= 30px separation between parallel segments.

### Post-Placement Audit Checklist

After placing all edges, run this mental audit:

1. **Vertical corridors**: List all waypoint x-coordinates. Flag any duplicates
   where the corresponding y-ranges overlap or are within 30px.
2. **Horizontal corridors**: List all waypoint y-coordinates. Flag any
   duplicates where the corresponding x-ranges overlap or are within 30px.
3. **Fan-in/fan-out**: For edges sharing a source or target, verify distinct
   entry/exit corridors with >= 30px between parallel segments.
4. **Near-miss check**: Edges within 15px of each other visually merge at
   normal zoom levels. Treat anything < 30px as an overlap.

---

## Appendix: Minimal Working Template

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile>
  <diagram name="Module Architecture">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1" page="1"
                  pageScale="1" pageWidth="1200" pageHeight="800">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- Title -->
        <mxCell id="title" value="&lt;b&gt;Module Name - Architecture&lt;/b&gt;"
                style="text;html=1;fontSize=18;fontStyle=1;align=left;
                       verticalAlign=top;strokeColor=none;fillColor=none;"
                vertex="1" parent="1">
          <mxGeometry x="20" y="20" width="400" height="30" as="geometry"/>
        </mxCell>

        <!-- Input port -->
        <mxCell id="in_port" value="data_in"
                style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;
                       strokeColor=#059669;strokeWidth=2;fontSize=11;"
                vertex="1" parent="1">
          <mxGeometry x="20" y="160" width="100" height="60" as="geometry"/>
        </mxCell>

        <!-- Processing block -->
        <mxCell id="proc" value="&lt;b&gt;Compute&lt;/b&gt;&lt;br&gt;II=1"
                style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;
                       strokeColor=#1971c2;strokeWidth=2;fontSize=11;"
                vertex="1" parent="1">
          <mxGeometry x="200" y="140" width="200" height="100" as="geometry"/>
        </mxCell>

        <!-- Output port -->
        <mxCell id="out_port" value="data_out"
                style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;
                       strokeColor=#059669;strokeWidth=2;fontSize=11;"
                vertex="1" parent="1">
          <mxGeometry x="480" y="160" width="100" height="60" as="geometry"/>
        </mxCell>

        <!-- Data bus: in -> proc -->
        <mxCell style="edgeStyle=orthogonalEdgeStyle;strokeColor=#1971c2;
                       strokeWidth=2;endArrow=block;endFill=1;html=1;"
                edge="1" source="in_port" target="proc" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Data bus: proc -> out -->
        <mxCell style="edgeStyle=orthogonalEdgeStyle;strokeColor=#1971c2;
                       strokeWidth=2;endArrow=block;endFill=1;html=1;"
                edge="1" source="proc" target="out_port" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## Appendix: Style Quick Reference

### Shape Styles

```
Rectangle:          rounded=1;whiteSpace=wrap;html=1;
Ellipse:            ellipse;whiteSpace=wrap;html=1;
Diamond:            rhombus;whiteSpace=wrap;html=1;
Cylinder:           shape=cylinder3;whiteSpace=wrap;html=1;size=10;
Hexagon:            shape=hexagon;whiteSpace=wrap;html=1;size=0.15;
Parallelogram:      shape=parallelogram;whiteSpace=wrap;html=1;size=0.15;
Container:          rounded=1;whiteSpace=wrap;html=1;container=1;collapsible=0;
Text only:          text;html=1;strokeColor=none;fillColor=none;
```

### Edge Styles

```
Orthogonal:         edgeStyle=orthogonalEdgeStyle;
Curved:             curved=1;
Straight:           edgeStyle=none;
Elbow:              edgeStyle=elbowEdgeStyle;
```

### Font Styles (fontStyle bitmask)

```
Regular:            fontStyle=0;
Bold:               fontStyle=1;
Italic:             fontStyle=2;
Bold + Italic:      fontStyle=3;
Underline:          fontStyle=4;
```

### Common Properties

```
strokeWidth=N;      Line thickness (1, 1.5, 2, 3)
dashed=1;           Dashed line
dashPattern=8 4;    Dash pattern (8px dash, 4px gap)
opacity=N;          0-100 transparency
arcSize=N;          Corner radius for rounded rectangles (10-20 typical)
spacingTop=N;       Internal padding top
spacingLeft=N;      Internal padding left
align=left;         Text alignment (left, center, right)
verticalAlign=top;  Vertical text alignment (top, middle, bottom)
```
