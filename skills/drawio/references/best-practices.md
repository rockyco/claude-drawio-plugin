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

### Edge Quality Rules

Six rules that prevent the most common visual defects in generated diagrams.
Rules 1-5 are correctness rules (violations look broken). Rule 6 is a layout
optimization pass. For the concise checklist, see the main SKILL.md. This
section provides detailed explanations and XML examples.

#### Rule 1: Face Points Only (Corners Forbidden)

Edge endpoints (`exitX/exitY`, `entryX/entryY`) must be ON a face: exactly
one coordinate at 0 or 1, the other anywhere in [0,1]. Default to midpoints
(0.5). When 2+ edges share the same face, distribute using fractional
positions (0.25, 0.5, 0.75). Corner values (both coordinates at 0 or 1)
create a diagonal stub in orthogonal routing because the router draws a
straight line from the corner to the first orthogonal waypoint, producing a
45-degree segment that looks broken.

```xml
<!-- GOOD: exits right midpoint (default) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;exitX=1;exitY=0.5;exitDx=0;exitDy=0;
               entryX=0;entryY=0.5;entryDx=0;entryDy=0;strokeColor=#1971c2;strokeWidth=2;
               endArrow=classic;html=1;"
        edge="1" source="m1" target="m2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- GOOD: exits left face at 75% down (distributed to avoid overlap) -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;exitX=0;exitY=0.75;exitDx=0;exitDy=0;
               entryX=0;entryY=0.5;entryDx=0;entryDy=0;strokeColor=#e03131;strokeWidth=1;
               dashed=1;endArrow=classic;html=1;"
        edge="1" source="fsm" target="offset-fifo" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- BAD: exits bottom-right corner - creates diagonal stub -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;exitX=1;exitY=1;exitDx=0;exitDy=0;
               entryX=0;entryY=1;entryDx=0;entryDy=0;strokeColor=#1971c2;strokeWidth=2;
               endArrow=classic;html=1;"
        edge="1" source="m1" target="m2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Valid connection points (one coordinate at 0 or 1):
- `(0, 0.5)` - left center (default)
- `(0.5, 0)` - top center (default)
- `(1, 0.5)` - right center (default)
- `(0.5, 1)` - bottom center (default)
- `(0, 0.25)`, `(0, 0.75)` - left face, distributed
- `(0.25, 0)`, `(0.75, 0)` - top face, distributed
- `(1, 0.25)`, `(1, 0.75)` - right face, distributed
- `(0.25, 1)`, `(0.75, 1)` - bottom face, distributed

#### Rule 2: No Arrow Overlap

See Section 9 for the full 30px minimum clearance rule and audit checklist.

#### Rule 3: Minimum 30px Last-Segment Length

The final segment of an L-shaped or elbow arrow (the segment entering the
target shape) must be >= 30px long. When this segment is too short (< 30px),
the arrowhead visually overlaps the perpendicular segment before the bend,
creating an ambiguous integral-sign or T-junction appearance.

```
Problem (final segment ~10px):     Fixed (final segment 40px):

    source                             source
       |                                  |
       +---->target                       +-------+
                                                  |
                                                  v
                                               target
```

**Fix**: Add a waypoint that pushes the bend further back from the target.
The waypoint should be at least 30px from the target's entry edge.

```xml
<!-- GOOD: waypoint at x=250, target entry at x=300. Last segment = 50px -->
<mxCell style="edgeStyle=orthogonalEdgeStyle;entryX=0;entryY=0.5;entryDx=0;entryDy=0;
               strokeColor=#1971c2;strokeWidth=2;endArrow=classic;html=1;"
        edge="1" source="src" target="dst" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="250" y="200"/>
      <mxPoint x="250" y="350"/>
    </Array>
  </mxGeometry>
</mxCell>
```

#### Rule 4: Arrow Lines Invisible Under Text Boxes

When an edge passes behind a standalone text cell (label, annotation, title),
the text cell must have an opaque background (`fillColor=#FFFFFF`) so the
arrow line is hidden underneath. Text cells with `fillColor=none` are
transparent and let arrow lines bleed through, creating visual clutter.

```xml
<!-- GOOD: opaque white background hides arrow line underneath -->
<mxCell id="label-phase" value="Phase 1: Input"
        style="text;html=1;fontSize=14;fontStyle=1;fillColor=#FFFFFF;strokeColor=none;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="50" width="120" height="30" as="geometry"/>
</mxCell>

<!-- BAD: transparent background lets arrows bleed through -->
<mxCell id="label-phase" value="Phase 1: Input"
        style="text;html=1;fontSize=14;fontStyle=1;fillColor=none;strokeColor=none;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="50" width="120" height="30" as="geometry"/>
</mxCell>
```

For edge labels (`value` on an edge cell), set `labelBackgroundColor=#FFFFFF`
(or the containing stage fill color) in the edge style to prevent the label
text from overlapping the edge line itself.

#### Rule 5: Dashed Box Titles Center-Aligned

DATAFLOW stage border labels and any title text inside dashed containers must
use `align=center;` in their style. The DATAFLOW Stage Border style already
includes `align=center;`, but standalone title text cells placed inside stage
borders are often generated with `align=left;` by default, causing off-center
titles.

```xml
<!-- GOOD: center-aligned title inside a dashed stage border -->
<mxCell value="EXTRACT Stage"
        style="text;html=1;fontSize=13;fontStyle=1;fontColor=#475569;align=center;"
        vertex="1" parent="stage-extract">
  <mxGeometry x="0" y="5" width="280" height="25" as="geometry"/>
</mxCell>

<!-- BAD: left-aligned title looks off-center in a centered container -->
<mxCell value="EXTRACT Stage"
        style="text;html=1;fontSize=13;fontStyle=1;fontColor=#475569;align=left;"
        vertex="1" parent="stage-extract">
  <mxGeometry x="10" y="5" width="280" height="25" as="geometry"/>
</mxCell>
```

#### Rule 6: Layout Improvement Pass

A post-generation audit that optimizes layout quality. Run after initial
placement and edge routing. Six techniques, applied in order:

##### T1: Connection Point Distribution

**Problem**: Multiple edges auto-route to the same face midpoint, overlapping.

**Rule**: When 2+ edges connect to the same shape, distribute them across
different faces. When forced to share a face, use distributed positions
(0.25, 0.5, 0.75) not all at 0.5.

```xml
<!-- BAD: data_in and fineOffset_in both enter FSM at left midpoint -->
<mxCell id="edge-in-fsm" source="io-data-in" target="extract-fsm"
        style="edgeStyle=orthogonalEdgeStyle;entryX=0;entryY=0.5;..." edge="1" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
<mxCell id="edge-ctrl-fsm" source="io-fineOffset-in" target="extract-fsm"
        style="edgeStyle=orthogonalEdgeStyle;entryX=0;entryY=0.5;..." edge="1" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- GOOD: ctrl stays on left face (default), data enters from bottom -->
<mxCell id="edge-in-fsm" source="io-data-in" target="extract-fsm"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;
               endArrow=block;endFill=1;html=1;
               entryX=0.129;entryY=1.008;entryDx=0;entryDy=0;entryPerimeter=0;"
        edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="180" y="320"/>
      <mxPoint x="180" y="300"/>
      <mxPoint x="218" y="300"/>
    </Array>
  </mxGeometry>
</mxCell>
<mxCell id="edge-ctrl-fsm" source="io-fineOffset-in" target="extract-fsm"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;..." edge="1" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

##### T2: Route-Around via Alternative Face

**Problem**: Direct path from shape A to shape B crosses through shape C.

**Rule**: Choose an alternative entry/exit face that allows an obstacle-free
route, even if it means more waypoints.

```
BEFORE: circ_buf --right--> [crosses through FSM] --left--> lltf_stream
AFTER:  circ_buf --right(midpoint)--> waypoints route below FSM --> lltf_stream(bottom)
```

```xml
<!-- GOOD: exit right midpoint, route around obstacle, enter FIFO from bottom -->
<mxCell id="edge-circ-lltf" source="extract-circbuf" target="fifo-lltf"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;
               endArrow=block;endFill=1;html=1;
               exitX=1;exitY=0.5;exitDx=0;exitDy=0;
               entryX=0.5;entryY=1;entryDx=0;entryDy=0;"
        edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="370" y="350"/>
      <mxPoint x="370" y="300"/>
      <mxPoint x="420" y="300"/>
    </Array>
  </mxGeometry>
</mxCell>
```

##### T3: Explicit Waypoints for Route Determinism

**Problem**: Auto-routing with 0-1 waypoints produces ambiguous L-shapes in
crowded areas. Different renderers may choose different paths.

**Rule**: Add enough waypoints to make every segment explicit. The auto-router
should only handle the first/last few pixels from exit/entry points to the
nearest waypoint.

```xml
<!-- BAD: 1 waypoint, auto-router decides the remaining path -->
<mxCell id="edge-circ-pass" source="extract-circbuf" target="fifo-passthrough"
        style="edgeStyle=orthogonalEdgeStyle;..." edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="270" y="430"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- GOOD: 3 waypoints, every segment is fully specified -->
<mxCell id="edge-circ-pass" source="extract-circbuf" target="fifo-passthrough"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;
               endArrow=block;endFill=1;html=1;
               exitX=0.5;exitY=1;exitDx=0;exitDy=0;
               entryX=0;entryY=0.5;entryDx=0;entryDy=0;"
        edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="270" y="430"/>
      <mxPoint x="330" y="430"/>
      <mxPoint x="330" y="455"/>
    </Array>
  </mxGeometry>
</mxCell>
```

##### T4: Detached Edges for Internal Routing

**Problem**: Two shapes are vertically stacked with intervening shapes, and
connected routing (source/target) cannot find a clean path.

**Rule**: Use detached edges with explicit `sourcePoint`/`targetPoint`
coordinates instead of `source`/`target` attributes. This gives full manual
control over the path.

```xml
<!-- GOOD: NCO -> rotation with phase_acc between them -->
<mxCell id="edge-nco-rotation" value="sin/cos"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#4f46e5;
               strokeWidth=1;dashed=1;endArrow=block;endFill=1;html=1;
               fontSize=9;fontColor=#4f46e5;labelBackgroundColor=#FFFFFF;"
        edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="1240" y="175"/>
      <mxPoint x="1240" y="430"/>
    </Array>
    <mxPoint x="1160" y="175" as="sourcePoint"/>
    <mxPoint x="1180" y="430" as="targetPoint"/>
  </mxGeometry>
</mxCell>
```

**Trade-off**: Detached edges do not auto-update when shapes are moved. Use
only when connected routing consistently fails.

##### T5: Route Simplification (Outside-Route Pattern)

**Problem**: Cross-stage edges use multi-waypoint Z-shapes routing through
stage content areas, crossing internal shapes.

**Rule**: For edges crossing multiple stages, route OUTSIDE stage content
areas (above or below all blocks). This avoids crossing through internal
shapes and produces simpler routes with fewer waypoints.

```
BEFORE (3 waypoints, routes through stages):
  cordic --> [670,380] --> [940,380] --> [940,280] --> freq_out

AFTER (2 waypoints, routes below all stage content):
  cordic --> [670,350] --> [1410,350] --> freq_out
```

```xml
<!-- GOOD: route below all stage content, past APPLY stage boundary -->
<mxCell id="edge-cordic-freqout" value="fineCfoFreq"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;
               strokeWidth=1;dashed=1;endArrow=block;endFill=1;html=1;
               fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;"
        source="compute-cordic" target="io-freq-out" edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="670" y="350"/>
      <mxPoint x="1410" y="350"/>
    </Array>
  </mxGeometry>
</mxCell>
```

##### T6: Container Tightening

**Problem**: Stage borders have excess whitespace below content, wasting
vertical space and pushing the legend too far down.

**Rule**: After placing all content, set container height to
`max_child_bottom_y - container_y + 30px` (padding). Adjust legend and
resource box positions to match.

```
BEFORE: EXTRACT border height=620, APPLY border height=620
        Legend at y=770

AFTER:  EXTRACT border height=570, APPLY border height=570  (-50px)
        Legend at y=680                                       (-90px)
```

Formula: `container_height = (max_y_of_children + child_height) - container_y + 30`

##### T7: Center Alignment for Vertical Connections

**Problem**: Shapes connected by top-to-bottom arrows have different widths
but the same left-x, causing "vertical" arrows to angle.

**Rule**: When shapes are connected by vertical arrows (exitX=0.5 to
entryX=0.5), all connected shapes must share the same center-x. Compute
each shape's x-position from its width: `shape.x = center_x - width / 2`.

```
BEFORE (left-aligned, angled arrows):

  x=910  [NCO LUT    w=130]     center=975
           |  (angled)
  x=910  [Phase Acc   w=120]    center=970
           |  (angled)
  x=910  [Complex MUL w=145]    center=982.5

AFTER (center-aligned, straight arrows):

  x=910  [NCO LUT    w=130]     center=975
           |  (straight)
  x=915  [Phase Acc   w=120]    center=975
           |  (straight)
  x=903  [Complex MUL w=145]    center=975.5
```

```xml
<!-- Center-aligned at center_x=975 -->
<mxCell id="apply-nco" style="...">
  <mxGeometry x="910" y="130" width="130" height="65" as="geometry"/>
  <!-- center = 910 + 65 = 975 -->
</mxCell>
<mxCell id="apply-phase-acc" style="...">
  <mxGeometry x="915" y="225" width="120" height="50" as="geometry"/>
  <!-- center = 915 + 60 = 975 -->
</mxCell>
<mxCell id="apply-cmul" style="...">
  <mxGeometry x="903" y="330" width="145" height="65" as="geometry"/>
  <!-- center = 903 + 72.5 = 975.5 (sub-pixel, visually identical) -->
</mxCell>
```

**Waypoint update**: When repositioning a shape for center alignment, update
all edge waypoints that target that shape's center. For example, if a
passthrough edge's waypoint was `x=982` (old center of Complex MUL), update
to `x=975` (new center).

##### T8: Label-Edge Clearance

**Problem**: FIFO label boxes or annotation labels overlap with edge routing
paths, causing edges to visually cross through the label text.

**Rule**: After placing all edges, verify that no annotation label overlaps
an edge path segment. Labels must have >= 15px clearance from the nearest
edge segment (both horizontal and vertical).

**Z-order fix**: In addition to clearance, place annotation labels AFTER all
edges in the XML document order so they render on top (Layer 4). This
ensures that even if an edge passes near a label, the label's background
covers the edge line underneath.

```
BEFORE: passthrough_stream label at y=432-462, edge at y=445
        -> edge crosses through label text

AFTER:  passthrough_stream label at y=400-430, edge at y=445
        -> 15px clearance, label above edge
```

```xml
<!-- XML document order matters for z-order -->

<!-- Layer 1-2: Stage borders, then component shapes -->
<mxCell id="stage-extract-border" ... vertex="1" parent="1"/>
<mxCell id="extract-fsm" ... vertex="1" parent="1"/>

<!-- Layer 3: All edges -->
<mxCell id="edge-passthrough" ... edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="245" y="445"/>
      <mxPoint x="975" y="445"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- Layer 4 (AFTER edges): Annotation labels render on top -->
<mxCell id="fifo-passthrough-label" value="passthrough_stream"
        style="rounded=1;fillColor=#eff6ff;strokeColor=#1971c2;..."
        vertex="1" parent="1">
  <mxGeometry x="540" y="400" width="155" height="30" as="geometry"/>
  <!-- y=400, 45px ABOVE edge at y=445 -->
</mxCell>

<!-- Layer 4: Legend box (also after edges) -->
<mxCell id="legend-box" ... vertex="1" parent="1"/>
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
| Z-order: FIFO labels before edges | Edges cover FIFO label boxes | 4-layer order: borders, shapes, edges, annotations (T8) |
| Edge without `source`/`target` | Disconnected floating arrow | Set `source` and `target` to valid cell IDs |
| Overlapping text labels | Unreadable text clutter | Offset labels by 15-20px, use `labelPosition` |
| Arrow labels on top of shapes | Label obscured by fill | Use `edgeLabel` or offset the label geometry |
| Missing `as="geometry"` | Geometry not applied | `<mxGeometry ... as="geometry"/>` is required |
| Container without `container=1` | Child cells not grouped | Add `container=1;` to parent style |
| Text outside visible canvas | Content clipped or invisible | Keep all elements within positive x,y coordinates |
| Corner exit/entry on edges | Diagonal stub in orthogonal routing | Use face points (one coord at 0/1). Distribute (0.25, 0.75) when sharing |
| Short last segment (< 30px) | Arrowhead overlaps previous segment | Add waypoint to push bend >= 30px from target entry |
| Text label with `fillColor=none` over edge path | Arrow line bleeds through label | Set `fillColor=#FFFFFF` on text cell |
| Multiple edges at same face midpoint | Overlapping edges at connection point | Distribute across faces; use 0.25/0.75 when sharing (T1) |
| Edge crosses through unrelated shape | Arrow visually pierces a block | Reroute via alternative face (T2) or use detached edge (T4) |
| Auto-routing in crowded area | Different renderers choose different paths | Add explicit waypoints for every segment (T3) |
| Cross-stage edge through content | Arrow weaves through internal blocks | Route outside stage content areas (T5) |
| Container with 50px+ excess whitespace | Wasted vertical space, legend too far | Tighten to content + 30px padding (T6) |
| Vertical shapes with same left-x but different widths | Angled "vertical" arrows between them | Center-align: `x = center - width/2` (T7) |
| FIFO label boxes defined before edges in XML | Edges render on top, covering FIFO labels | Move FIFO labels to Layer 4 (after all edges) (T8) |
| Annotation label overlapping edge route | Edge line crosses through label text | Reposition label with >= 15px clearance from edge (T8) |

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

After placing all edges, run this audit in two phases:

**Phase A: Correctness (Rules 1-5)**

1. **Vertical corridors**: List all waypoint x-coordinates. Flag any duplicates
   where the corresponding y-ranges overlap or are within 30px.
2. **Horizontal corridors**: List all waypoint y-coordinates. Flag any
   duplicates where the corresponding x-ranges overlap or are within 30px.
3. **Fan-in/fan-out**: For edges sharing a source or target, verify distinct
   entry/exit corridors with >= 30px between parallel segments.
4. **Near-miss check**: Edges within 15px of each other visually merge at
   normal zoom levels. Treat anything < 30px as an overlap.
5. **Corner connections**: Verify no edge has BOTH exitX/exitY (or
   entryX/entryY) at 0 or 1. Face points (one coordinate at 0/1) are valid.
6. **Last-segment length**: For each L-shaped or elbow edge, measure the
   distance from the last bend to the target entry point. Must be >= 30px.
7. **Label backgrounds**: Check all standalone text cells that overlap with
   edge paths. Each must have `fillColor=#FFFFFF` (not `fillColor=none`).

**Phase B: Layout Optimization (Rule 6)**

8. **Edge-shape crossthrough**: Trace each edge path. If any segment passes
   through a shape it does not connect to, reroute via T2 (alternative face)
   or T4 (detached edge).
9. **Connection point distribution**: Find shapes with 2+ edges on the same
   face. Apply T1 - distribute across faces or use fractional positions.
10. **Route determinism**: In crowded areas, verify each edge has enough
    waypoints for a fully deterministic path (T3).
11. **Cross-stage simplification**: Check edges spanning 2+ stages. Apply T5
    - route outside stage content areas where possible.
12. **Container tightening**: Verify container heights match content with
    30px padding (T6). Move legend/resource boxes if containers were shrunk.
13. **Vertical arrow alignment**: For shapes connected by top-to-bottom
    arrows (exitX=0.5, entryX=0.5), verify all share the same center-x.
    Formula: `shape.x = center_x - width / 2` (T7).
14. **Label-edge clearance**: Verify no FIFO label or annotation box
    overlaps an edge path. Min 15px clearance. Confirm labels are placed
    AFTER edges in XML for correct z-order (T8).

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
