---
name: drawio
description: Create diagrams using Draw.io XML format. Use when the user asks to create diagrams, flowcharts, architecture diagrams, RTL micro-architecture, FSM state diagrams, DATAFLOW decomposition, system topology, wireframes, or any visual drawings. Generates `.drawio` XML files. Preferred over excalidraw for FPGA/HLS diagrams due to simpler format, auto-centered text, and uniform XML for all diagram types including RTL microarch.
---

# Draw.io Diagram Creation

Generate `.drawio` files as XML using the Write tool. No scripts or executables needed.

## Key Advantages Over Excalidraw

| Feature | Draw.io | Excalidraw |
|---------|---------|------------|
| Text in shapes | `value` attribute, auto-centered | Separate text element + containerId + centering math |
| Tokens per element | ~30-50 | ~80-120 |
| RTL microarch format | Same XML as all other diagrams | Requires separate hand-crafted SVG |
| Arrow routing | Built-in orthogonal routing styles | Manual point arrays |
| Binding system | None needed | Bidirectional binding bugs common |
| Interactive editing | Desktop + VSCode extension + web | Web app only |

## File Wrapper

Every `.drawio` file uses this XML structure:

```xml
<mxfile host="Claude" modified="2026-01-01T00:00:00.000Z" agent="Claude Code" version="1.0">
  <diagram id="diagram-1" name="Page-1">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1" tooltips="1"
                  connect="1" arrows="1" fold="1" page="1" pageScale="1"
                  pageWidth="1200" pageHeight="800" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- All diagram elements go here as children of cell "1" -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

- `mxCell id="0"` - Root cell (always required, never modify)
- `mxCell id="1" parent="0"` - Default layer (always required, parent for all content cells)
- All content cells use `parent="1"`
- `dx`/`dy` control scroll offset; `pageWidth`/`pageHeight` control canvas size
- Set `pageWidth`/`pageHeight` to fit content (1200x800 default, increase for complex diagrams)

## Cell Types

### Vertex (Shape)

```xml
<mxCell id="node-1" value="Module Name" style="rounded=1;fillColor=#dbeafe;strokeColor=#1971c2;fontStyle=1;fontSize=12;html=1;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="160" height="60" as="geometry"/>
</mxCell>
```

- `id` - Unique identifier (use descriptive names: `dsp-mul`, `bram-hist`, `io-data-in`)
- `value` - Text displayed inside the shape. Supports HTML: `<b>`, `<br>`, `<font color='#xxx'>`
- `style` - Semicolon-separated style properties (see Style Reference below)
- `vertex="1"` - Marks this as a shape (not an edge)
- `parent="1"` - Parent is the default layer
- `mxGeometry` - Position (x,y) and size (width, height)

### Edge (Connection)

```xml
<mxCell id="edge-1" value="data_t" style="endArrow=classic;strokeColor=#1971c2;strokeWidth=2;html=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" parent="1" source="node-1" target="node-2">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

- `source`/`target` - IDs of connected shapes
- `value` - Edge label text (optional)
- `edge="1"` - Marks this as a connection
- `exitX`/`exitY` - Exit point on source (0-1 normalized). 1,0.5 = right center
- `entryX`/`entryY` - Entry point on target (0-1 normalized). 0,0.5 = left center
- Set `relative="1"` on mxGeometry for edges

### Edge with Waypoints

For non-straight routing (L-shaped, around obstacles):

```xml
<mxCell id="edge-2" value="" style="endArrow=classic;strokeColor=#1971c2;strokeWidth=2;html=1;edgeStyle=orthogonalEdgeStyle;" edge="1" parent="1" source="node-1" target="node-3">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="300" y="200"/>
      <mxPoint x="300" y="400"/>
    </Array>
  </mxGeometry>
</mxCell>
```

- `edgeStyle=orthogonalEdgeStyle` - Auto-routes with right-angle bends
- `Array as="points"` - Explicit waypoints for manual routing
- Orthogonal routing handles obstacle avoidance automatically

### Standalone Text (Label)

```xml
<mxCell id="label-1" value="Phase 1: Input" style="text;fontSize=14;fontStyle=1;fillColor=none;strokeColor=none;html=1;" vertex="1" parent="1">
  <mxGeometry x="100" y="50" width="120" height="30" as="geometry"/>
</mxCell>
```

- `fillColor=none;strokeColor=none` - Invisible container (text only)
- Use for titles, annotations, phase labels, bit-width annotations

## Style Reference

Style strings are semicolon-separated key=value pairs. Always include `html=1;` to enable HTML labels.

### Shape Styles

| Property | Values | Description |
|----------|--------|-------------|
| `rounded` | `0` (sharp) / `1` (rounded) | Corner style |
| `fillColor` | `#hex` | Background fill |
| `strokeColor` | `#hex` | Border color |
| `strokeWidth` | number | Border thickness (default 1) |
| `dashed` | `0` / `1` | Dashed border |
| `dashPattern` | `string` | e.g., `8 4` for custom dash |
| `fontStyle` | `0`/`1`/`2`/`3` | 0=normal, 1=bold, 2=italic, 3=bold+italic |
| `fontSize` | number | Text size in points |
| `fontColor` | `#hex` | Text color |
| `fontFamily` | name | e.g., `Courier New` for monospace |
| `html` | `1` | Enable HTML in value (ALWAYS include) |
| `whiteSpace` | `wrap` | Enable text wrapping |
| `align` | `left`/`center`/`right` | Horizontal text alignment |
| `verticalAlign` | `top`/`middle`/`bottom` | Vertical text alignment |
| `shape` | `ellipse`/`rhombus`/`cylinder`/`hexagon` | Non-rectangle shapes |
| `opacity` | 0-100 | Element transparency |

### Edge Styles

| Property | Values | Description |
|----------|--------|-------------|
| `endArrow` | `classic`/`block`/`open`/`none` | Arrowhead at target |
| `startArrow` | `classic`/`block`/`open`/`none` | Arrowhead at source |
| `strokeColor` | `#hex` | Line color |
| `strokeWidth` | number | Line thickness |
| `dashed` | `0` / `1` | Dashed line |
| `dashPattern` | `string` | Custom dash pattern |
| `edgeStyle` | `orthogonalEdgeStyle`/`elbowEdgeStyle`/`entityRelationEdgeStyle` | Routing algorithm |
| `curved` | `0` / `1` | Curved routing |
| `exitX`/`exitY` | 0-1 | Exit point on source shape |
| `entryX`/`entryY` | 0-1 | Entry point on target shape |
| `labelPosition` | `left`/`center`/`right` | Label horizontal position |
| `verticalLabelPosition` | `top`/`middle`/`bottom` | Label vertical position |

### Connection Points (exitX/exitY and entryX/entryY)

```
(0,0)-----(0.5,0)-----(1,0)
  |                      |
(0,0.5)              (1,0.5)
  |                      |
(0,1)-----(0.5,1)-----(1,1)
```

For ellipses, same normalized coordinates apply to the ellipse bounding box.

## FPGA Component Styles

### Semantic Color Palette

| Component | Fill | Stroke | Text Color | Use For |
|-----------|------|--------|------------|---------|
| DSP / Computation | `#dbeafe` | `#1971c2` | `#1e3a5f` | DSP48, multipliers, adders, accumulators |
| Memory / Storage | `#e0e7ff` | `#4f46e5` | `#3730a3` | BRAM, ROM, shift registers, circular buffers |
| Control / FSM | `#fef3c7` | `#d97706` | `#92400e` | FSM blocks, CORDIC, controllers |
| FIR IP | `#fef9c3` | `#ca8a04` | `#854d0e` | hls::FIR instances |
| Data I/O | `#d1fae5` | `#059669` | `#065f46` | Data streams (hls::stream) |
| Control I/O | `#fce7f3` | `#db2777` | `#9d174d` | Async control ports (index_t, scalar) |
| DATAFLOW Stage | `#f8fafc` | `#94a3b8` | `#475569` | Stage boundaries |
| Accumulator | `#99e9f2` | `#0c8599` | `#0c8599` | Running accumulators |
| Pipeline Register | `#e9ecef` | `#868e96` | `#495057` | Pipeline stage boundaries |

### Complete FPGA Style Strings

**DSP48 Block:**
```
rounded=0;fillColor=#dbeafe;strokeColor=#1971c2;fontColor=#1e3a5f;fontStyle=1;fontSize=11;html=1;whiteSpace=wrap;
```

**BRAM / Memory:**
```
rounded=1;fillColor=#e0e7ff;strokeColor=#4f46e5;fontColor=#3730a3;fontStyle=1;fontSize=11;html=1;whiteSpace=wrap;dashed=1;
```

**Control / FSM:**
```
rounded=1;fillColor=#fef3c7;strokeColor=#d97706;fontColor=#92400e;fontStyle=1;fontSize=11;html=1;whiteSpace=wrap;
```

**FIR IP:**
```
rounded=0;fillColor=#fef9c3;strokeColor=#ca8a04;fontColor=#854d0e;fontStyle=1;fontSize=11;html=1;whiteSpace=wrap;
```

**Data I/O Port (Ellipse):**
```
shape=ellipse;fillColor=#d1fae5;strokeColor=#059669;fontColor=#065f46;fontSize=10;html=1;
```

**Control I/O Port (Ellipse):**
```
shape=ellipse;fillColor=#fce7f3;strokeColor=#db2777;fontColor=#9d174d;fontSize=10;html=1;
```

**DATAFLOW Stage Border:**
```
rounded=1;fillColor=#f8fafc;strokeColor=#94a3b8;dashed=1;dashPattern=8 4;strokeWidth=1.5;fontStyle=1;fontSize=13;fontColor=#475569;html=1;verticalAlign=top;align=center;
```

**Accumulator:**
```
rounded=0;fillColor=#99e9f2;strokeColor=#0c8599;fontColor=#0c8599;fontStyle=1;fontSize=11;html=1;whiteSpace=wrap;
```

**Pipeline Register (thin tall rect):**
```
rounded=0;fillColor=#e9ecef;strokeColor=#868e96;fontColor=#495057;fontSize=9;html=1;
```

**FIFO Indicator:**
```
rounded=1;fillColor=#eff6ff;strokeColor=#1971c2;dashed=1;dashPattern=4 2;fontSize=10;fontColor=#1971c2;fontStyle=2;html=1;
```

**Resource Summary / Legend Box:**
```
rounded=1;fillColor=#f9fafb;strokeColor=#d1d5db;fontSize=11;fontColor=#4b5563;html=1;align=left;verticalAlign=top;whiteSpace=wrap;
```

### Arrow Styles

**Data Bus (solid blue):**
```
endArrow=classic;strokeColor=#1971c2;strokeWidth=2;html=1;
```

**Control Signal (dashed red):**
```
endArrow=classic;strokeColor=#e03131;strokeWidth=1.5;dashed=1;dashPattern=6 3;html=1;
```

**Memory Access (dashed indigo):**
```
endArrow=classic;strokeColor=#4f46e5;strokeWidth=1.5;dashed=1;dashPattern=4 2;html=1;
```

**Feedback Path (dotted gray):**
```
endArrow=classic;strokeColor=#868e96;strokeWidth=1;dashed=1;dashPattern=2 2;html=1;
```

**Coefficient Feed (dashed blue, thin):**
```
endArrow=classic;strokeColor=#1971c2;strokeWidth=1.5;dashed=1;dashPattern=4 2;html=1;
```

## HLS Diagram Types

### 1. System Block Diagram (Phase 1, 7)

Shows module topology with data/control streams and FIFO depths.

**Layout**: Left-to-right data flow. Modules as rounded rectangles. Splitters as small diamonds. I/O ports as ellipses at edges.

**Canvas**: `pageWidth="1400" pageHeight="600"`

**Required elements**:
- Input/output port ellipses (data I/O style)
- Module blocks with name + brief description
- Data stream arrows (solid blue) with type labels
- Control signal arrows (dashed red) with type labels
- Splitter nodes for fan-out
- FIFO depth annotations on Phase 7 integration diagrams
- Resource summary box (Phase 6+ with DSP/BRAM/LUT/FF)
- Legend box

### 2. DATAFLOW Decomposition (Phase 5)

Shows internal DATAFLOW stages (EXTRACT/COMPUTE/APPLY or TMCS 4-stage).

**Layout**: Left-to-right stages in dashed borders. Internal FIFOs between stages.

**Canvas**: `pageWidth="1200" pageHeight="600"`

**Required elements**:
- Stage boundary rectangles (DATAFLOW stage style, dashed)
- Function blocks inside each stage
- Internal FIFO streams between stages (with depth labels)
- Data I/O ports at diagram edges
- Control I/O ports if FSM+DATAFLOW
- Bypass/passthrough streams (below main flow)
- Stage labels: "EXTRACT", "COMPUTE", "APPLY" (or TMCS stages)

### 3. FSM State Diagram (Phase 5)

Shows finite state machine transitions.

**Layout**: States arranged in a flow: typically left-to-right or top-to-bottom for the main path, with loops shown as self-referencing edges.

**Canvas**: `pageWidth="1000" pageHeight="600"`

**Required elements**:
- State boxes as rounded rectangles (control/FSM style)
- Initial state indicator (extra bold border or filled arrow from a dot)
- Transition arrows with condition labels
- Self-loop transitions (edge with same source and target, use waypoints to create visible loop)
- Entry/exit actions as italic text inside state boxes

**Self-loop pattern:**
```xml
<mxCell id="self-loop-1" value="!ctrl.empty()" style="endArrow=classic;strokeColor=#d97706;strokeWidth=1.5;html=1;curved=1;" edge="1" parent="1" source="state-wait" target="state-wait">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="200" y="50"/>
    </Array>
  </mxGeometry>
</mxCell>
```

### 4. RTL Micro-Architecture (Phase 5, 6)

Detailed datapath diagram showing FPGA primitives, pipeline stages, and data flow with bit-width annotations.

**Layout**: Left-to-right data flow. I/O ports at edges. Pipeline stages separated by pipeline register columns. Storage row below main datapath.

**Canvas**: `pageWidth="1400" pageHeight="700"` (increase for complex modules)

**Required elements**:
- I/O port ellipses (data + control)
- DSP48 blocks for computation
- BRAM/shift register blocks for storage
- Pipeline register columns (thin tall rectangles)
- Accumulator blocks with feedback arrows
- Comparator diamonds for detection logic
- MUX diamonds for selection logic
- Bit-width annotations on data bus arrows (e.g., "18b", "32b cplx")
- Phase boundary labels
- Resource summary box with DSP/BRAM/LUT/FF counts
- Legend box with color key
- DATAFLOW stage borders (if applicable)

## HTML Labels in Shapes

Draw.io supports rich HTML in the `value` attribute. Always include `html=1;` in style.

### Multi-Line Labels

```xml
value="<b>DSP: Multiply</b><br>26 x (18b x 16b)<br><font color='#6b7280' style='font-size:9px'>26 DSP48E1</font>"
```

### Bold Title + Details

```xml
value="<b>hls::FIR (80 taps)</b><br>SP=4 | 10 DSP48E1"
```

### Monospace Technical Labels

```xml
value="<font face='Courier New'>circ_buf[16384]</font><br>BRAM, no partition"
```

### Subscript/Superscript

```xml
value="x[n] = r<sub>n</sub> + j*q<sub>n</sub>"
```

## Architecture Summary JSON (.arch.json)

Every diagram has a companion `.arch.json` file (~500-1500 tokens) for machine consumption. The schema is format-independent (same for excalidraw and draw.io diagrams).

Schema: `.claude/schemas/arch_diagram_schema.json`

**Key fields by diagram type:**

| Type | Required Fields | Optional Fields |
|------|----------------|-----------------|
| `system_block` | title, type, phase, version, nodes, edges | resources, patterns, hardware_params, control_signals |
| `module_architecture` | title, type, phase, version, nodes, edges | module_pattern, patterns_applied, control_inputs |
| `fsm_states` | title, type, phase, version, states | nodes |
| `dataflow` | title, type, phase, version, stages, edges | nodes |
| `system_integration` | title, type, phase, version, nodes, edges | fifo_depths, system_resources, latency_budget, per_module_resources |

## Construction Checklist

When generating a draw.io diagram:

1. **Start with the wrapper** - Copy the mxfile/diagram/mxGraphModel/root skeleton
2. **Set canvas size** - Adjust pageWidth/pageHeight based on diagram complexity
3. **Place I/O ports first** - Ellipses at left/right edges
4. **Add main blocks** - Position on a 10px grid, left-to-right data flow
5. **Add stage borders** - Dashed rectangles behind groups of blocks (use lower z-order by placing them earlier in XML)
6. **Connect with edges** - Data bus (solid blue), control (dashed red), memory (dashed indigo)
7. **Add labels** - Title, subtitle, bit-width annotations, phase labels
8. **Add resource summary** - Bottom-left box with DSP/BRAM/LUT/FF
9. **Add legend** - Bottom-right box with arrow/block color key
10. **Generate .arch.json** - Companion file with graph structure

### Z-Order

Elements are rendered in document order. Place background elements (stage borders) BEFORE foreground elements (blocks, arrows) in the XML.

### ID Convention

Use descriptive IDs for readability:
- Shapes: `io-data-in`, `dsp-mul`, `bram-hist`, `stage-extract`, `ctrl-fsm`
- Edges: `edge-data-in-to-sr`, `edge-ctrl-offset`, `edge-feedback`
- Labels: `label-title`, `label-phase1`, `label-bitwidth-18b`

### Grid Alignment

- Position all elements on 10px grid (x, y, width, height all multiples of 10)
- Minimum shape size: 80x40
- Standard spacing: 40-60px between shapes, 20-30px within groups
- I/O port ellipses: 70x30 minimum

## Rendering to PNG

Always generate a `.png` alongside the `.drawio` file so humans can review without opening an editor.

### Primary: draw.io Desktop CLI (AppImage)

Fastest and most deterministic. Renders headless via Electron + xvfb-run:

```bash
# One-time install (no sudo needed):
curl -sL -o /tmp/drawio.AppImage \
  "https://github.com/jgraph/drawio-desktop/releases/download/v29.5.1/drawio-x86_64-29.5.1.AppImage"
chmod +x /tmp/drawio.AppImage

# Render single diagram (2x scale for crisp output):
xvfb-run -a /tmp/drawio.AppImage --no-sandbox --export --format png --scale 2 \
  -o output.png input.drawio

# Batch render all .drawio files in a directory:
find path/to/diagrams -name "*.drawio" | while read f; do
  outpng="${f%.drawio}.png"
  xvfb-run -a /tmp/drawio.AppImage --no-sandbox --export --format png --scale 2 \
    -o "$outpng" "$f"
done
```

Options:
- `--scale 2`: 2x resolution (recommended for readability)
- `--format png|svg|pdf|jpg`: Output format
- `--no-sandbox`: Required for non-root execution
- `xvfb-run -a`: Virtual framebuffer (no display needed), auto-select server number

GPU errors in output (`Exiting GPU process due to errors during initialization`) are harmless
and do not affect rendering quality.

### Fallback: Playwright MCP

Use when the AppImage is unavailable. Injects XML into draw.io web editor via the
"Edit Diagram" dialog:

```
1. browser_navigate: https://app.diagrams.net/?splash=0&ui=atlas
2. browser_wait_for: time=5
3. Open Extras > Edit Diagram dialog
4. Inject XML via browser_evaluate using native HTMLTextAreaElement setter:
   () => {
     const xml = atob("<base64-encoded mxGraphModel XML>");
     const ta = document.querySelectorAll('textarea');
     const setter = Object.getOwnPropertyDescriptor(
       window.HTMLTextAreaElement.prototype, 'value'
     ).set;
     setter.call(ta[ta.length-1], xml);
     ta[ta.length-1].dispatchEvent(new Event('input', { bubbles: true }));
     return 'OK';
   }
5. Click OK button
6. browser_press_key: key=Control+Shift+H (fit to window)
7. browser_take_screenshot: filename=diagram.png
```

**Key detail**: The textarea value MUST be set using the native HTMLTextAreaElement
prototype setter, NOT direct assignment. Draw.io's React editor only detects
changes through the native setter + dispatched events.

**Browser sizing**: Use `browser_resize(1920, 1080)` before rendering for adequate
viewport. Default viewport is too small for complex diagrams.

## Comparison with Excalidraw

| Criterion | Draw.io | Excalidraw |
|-----------|---------|------------|
| Format | XML (~5 attrs/cell) | JSON (~50 props/element) |
| Tokens/element | ~30-50 | ~80-120 |
| Text centering | Automatic (value attr) | Manual formula (containerId + x,y calc) |
| Binding system | None needed | Bidirectional (boundElements + containerId) |
| RTL microarch | Same XML format | Separate SVG format required |
| Arrow routing | Built-in orthogonal | Manual point arrays |
| Hand-drawn look | No (always formal) | Yes (roughness > 0) |
| Editing options | Desktop + VSCode + web | Web only |
| CLI rendering | draw.io desktop or Playwright | excalirender |
| Common bugs | Style string typos | Binding mismatches, centering errors |

**When to use Draw.io**: FPGA/HLS architecture diagrams, formal technical documentation, any diagram that will be edited by others in VSCode or desktop tools.

**When to use Excalidraw**: Brainstorming, informal whiteboarding, diagrams that benefit from hand-drawn aesthetic.

## References

For detailed specifications, see:
- `references/element-reference.md` - Complete mxCell property reference
- `references/best-practices.md` - Layout guidelines, FPGA conventions, common mistakes
- `references/fpga-shapes.md` - Complete XML snippets for every FPGA component
- `references/examples.md` - 4 complete working diagrams (system, DATAFLOW, FSM, RTL microarch)
- `references/comparison.md` - Detailed empirical comparison with Excalidraw
- `templates/fpga-library.xml` - Importable custom shape library
