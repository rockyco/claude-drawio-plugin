# Draw.io vs Excalidraw: Comprehensive Comparison

Empirical comparison for FPGA/HLS architecture diagram generation.

## Format Comparison

### Token Cost per Element

Measured on equivalent elements (rounded rectangle with bold title + 2 detail lines):

**Excalidraw** (~105 tokens):
```json
{
  "type": "rectangle", "id": "rect-1",
  "x": 100, "y": 100, "width": 160, "height": 60,
  "angle": 0, "strokeColor": "#1971c2", "backgroundColor": "#dbeafe",
  "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
  "roughness": 0, "opacity": 100, "seed": 1001,
  "version": 1, "versionNonce": 1001, "isDeleted": false,
  "groupIds": [], "boundElements": [{"id": "text-1", "type": "text"}],
  "link": null, "locked": false, "roundness": {"type": 3}
}
// + separate text element (~65 tokens):
{
  "type": "text", "id": "text-1",
  "x": 112, "y": 113, "width": 136, "height": 34,
  "text": "DSP: Multiply\n26 DSP48E1", "fontSize": 14,
  "fontFamily": 2, "textAlign": "center", "verticalAlign": "middle",
  "angle": 0, "strokeColor": "#1e3a5f", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
  "roughness": 0, "opacity": 100, "seed": 1002,
  "version": 1, "versionNonce": 1002, "isDeleted": false,
  "groupIds": [], "boundElements": null,
  "link": null, "locked": false,
  "containerId": "rect-1", "originalText": "DSP: Multiply\n26 DSP48E1"
}
```
Total: ~170 tokens for shape + text

**Draw.io** (~40 tokens):
```xml
<mxCell id="dsp-mul" value="<b>DSP: Multiply</b><br>26 DSP48E1"
  style="rounded=0;fillColor=#dbeafe;strokeColor=#1971c2;fontStyle=1;fontSize=11;html=1;"
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="160" height="60" as="geometry"/>
</mxCell>
```
Total: ~40 tokens for shape + text (single element)

**Ratio: Draw.io uses ~24% of Excalidraw's tokens per element** (4.25x more efficient).

### Arrow Token Cost

**Excalidraw arrow** (~120 tokens): Requires id, type, points array, startBinding, endBinding, startArrowhead, endArrowhead, plus all standard properties (seed, version, roughness, opacity, etc.)

**Draw.io arrow** (~35 tokens):
```xml
<mxCell id="edge-1" value="data_t" style="endArrow=classic;strokeColor=#1971c2;strokeWidth=2;html=1;"
  edge="1" parent="1" source="dsp-mul" target="acc-1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

**Ratio: Draw.io uses ~29% of Excalidraw's tokens per arrow** (3.4x more efficient).

### Full Diagram Token Cost (System Block Diagram, 5 modules)

| Component | Count | Excalidraw | Draw.io |
|-----------|-------|-----------|---------|
| Module blocks | 5 | 5 x 170 = 850 | 5 x 40 = 200 |
| I/O ports | 4 | 4 x 140 = 560 | 4 x 35 = 140 |
| Arrows | 8 | 8 x 120 = 960 | 8 x 35 = 280 |
| Labels | 6 | 6 x 65 = 390 | 6 x 25 = 150 |
| Splitter | 1 | 1 x 100 = 100 | 1 x 30 = 30 |
| Legend box | 1 | 1 x 300 = 300 | 1 x 80 = 80 |
| **Total** | **25** | **~3,160** | **~880** |
| Wrapper overhead | - | ~150 | ~250 |
| **Grand total** | - | **~3,310** | **~1,130** |

**Draw.io is ~3x more token-efficient** for a typical system diagram.

---

## Feature Comparison Matrix

| Criterion | Draw.io | Excalidraw | Winner |
|-----------|---------|------------|--------|
| **Token efficiency** | ~30-50/element | ~80-120/element | Draw.io |
| **Text centering** | Automatic (value attr) | Manual formula required | Draw.io |
| **Binding system** | None needed | Bidirectional (error-prone) | Draw.io |
| **RTL microarch** | Same XML format | Separate hand-crafted SVG | Draw.io |
| **Arrow routing** | Built-in orthogonal style | Manual point arrays | Draw.io |
| **Format readability** | XML (structured, verbose) | JSON (flat, verbose) | Tie |
| **Hand-drawn aesthetic** | Not available | Yes (roughness > 0) | Excalidraw |
| **Interactive editing** | Desktop + VSCode + web | Web only | Draw.io |
| **CLI rendering** | AppImage + xvfb-run (3s/diagram, 2x scale) | excalirender (installed) | Tie |
| **AI generation errors** | Style string typos | Binding mismatches, centering bugs | Draw.io |
| **Common AI bug severity** | Low (style typo = visual only) | High (binding bug = missing text) | Draw.io |
| **File size** | ~3-8 KB typical | ~8-15 KB typical | Draw.io |
| **Existing ecosystem** | Mature, widely adopted | Growing, popular for whiteboarding | Tie |
| **VSCode integration** | hediet.vscode-drawio extension | None built-in | Draw.io |
| **Version control diff** | XML diffs readable | JSON diffs verbose | Draw.io |

---

## Error Rate Analysis

### Excalidraw Common AI Generation Errors

1. **Bidirectional binding mismatch** (HIGH severity) - Shape's `boundElements` doesn't list text element, or text's `containerId` points to wrong shape. Result: text appears at wrong position or disappears.
2. **Text centering formula error** (MEDIUM severity) - CLI rendering uses literal x,y coordinates while web app auto-centers. Incorrect character width estimate causes text overflow or misalignment.
3. **Seed/version duplication** (LOW severity) - Two elements share a seed value. Usually harmless but can cause rendering artifacts.
4. **Arrow binding to deleted element** (HIGH severity) - Arrow's startBinding/endBinding references a removed element. Arrow renders incorrectly.
5. **Missing roughness/opacity** (LOW severity) - Omitted properties default differently across renderers.

### Draw.io Common AI Generation Errors

1. **Missing `html=1;` in style** (MEDIUM severity) - HTML tags in value rendered as literal text. Easy to spot and fix.
2. **Missing `parent="1"`** (HIGH severity) - Cell not attached to layer, invisible. Easy to diagnose.
3. **Style string typo** (LOW severity) - Misspelled property name ignored silently. Visual only.
4. **Missing root cells** (HIGH severity) - No id=0/id=1 cells. Entire diagram fails to load.
5. **Edge without source/target** (LOW severity) - Disconnected arrow. Renders but not connected.

**Draw.io errors are generally easier to diagnose and fix** because:
- No binding system means no binding bugs
- HTML in value is self-contained (no separate text elements)
- Missing properties are silently ignored rather than causing layout breaks
- Style string errors affect appearance only, not structure

---

## Format-Specific Advantages

### Draw.io Unique Advantages

1. **Single format for all diagram types** - System topology, DATAFLOW, FSM, and RTL microarch all use the same XML format. Excalidraw requires hand-crafted SVG for RTL microarch due to text centering limitations in the CLI renderer.

2. **VSCode extension** - The hediet.vscode-drawio extension provides in-editor diagram editing with real-time preview. No need to switch to a web app.

3. **Built-in orthogonal routing** - `edgeStyle=orthogonalEdgeStyle` automatically routes arrows with right-angle bends around obstacles. Excalidraw requires manual point array calculation.

4. **Auto-centered text** - Text in the `value` attribute is automatically centered within the shape. No centering formula, no character width estimation, no containerId binding.

5. **HTML rich text** - Bold, italic, color, font size, line breaks all in a single `value` attribute. Excalidraw requires separate text elements for each style variation.

6. **Container grouping** - `container=1` style creates collapsible groups. Children inherit the parent's coordinate space.

### Excalidraw Unique Advantages

1. **Hand-drawn aesthetic** - `roughness > 0` creates an informal, whiteboard-like appearance. Draw.io always renders formally.

2. **excalirender CLI** - Already installed and working for PNG generation without browser dependencies.

3. **Simpler mental model** - JSON properties are flat key-value pairs. No XML nesting or attribute syntax.

4. **Web-first collaboration** - excalidraw.com supports real-time collaboration. Draw.io web app is single-user.

---

## Recommendation

### Use Draw.io When:
- Generating FPGA/HLS architecture diagrams (all 4 types)
- Token budget is constrained (3-4x savings)
- Diagrams will be edited by others in VSCode or desktop tools
- RTL micro-architecture diagrams are needed (avoids dual-format)
- Formal technical documentation appearance is desired
- Minimizing AI generation errors is a priority

### Use Excalidraw When:
- Informal brainstorming or whiteboarding aesthetic is desired
- Real-time web collaboration is needed
- The diagram is simple enough that binding bugs are unlikely
- The existing excalirender pipeline is preferred for PNG generation

### For MATLAB2HLS Framework:
**Draw.io is the recommended format** for all FPGA/HLS diagrams due to:
- 3-4x token efficiency per diagram
- Elimination of dual-format complexity (no separate SVG for microarch)
- Lower error rate in AI generation
- Better editing workflow with VSCode extension
- Same semantic .arch.json companion regardless of visual format

---

## Empirical Validation: wlanSync Full Pipeline (17 diagrams)

All 17 wlanSync project diagrams were generated in draw.io XML and rendered to PNG
via the AppImage CLI (`xvfb-run -a /tmp/drawio.AppImage --no-sandbox --export --format png --scale 2`).

### Rendering Results

| Diagram | Type | Resolution | Size (KB) |
|---------|------|-----------|-----------|
| system_block_diagram | System topology | 2925x1203 | 263 |
| system_integration | System integration | 2705x1413 | 285 |
| M0 architecture | FIR architecture | 2303x1172 | 251 |
| M0 microarch | RTL datapath | 2485x1224 | 228 |
| M1 architecture | Detection arch | 1683x993 | 193 |
| M1 microarch | RTL datapath | 2645x999 | 255 |
| M2 architecture | FSM+DATAFLOW arch | 2605x924 | 265 |
| M2 dataflow | 4-stage TMCS | 2605x924 | 231 |
| M2 FSM | 4-state FSM | 1963x943 | 197 |
| M2 microarch | RTL datapath | 2905x1303 | 351 |
| M3 architecture | FIR IP arch | 2563x1463 | 187 |
| M3 dataflow | 3-FIR pipeline | 2905x1743 | 221 |
| M3 microarch | RTL datapath | 3005x1643 | 252 |
| M4 architecture | FSM+DATAFLOW arch | 2865x1623 | 282 |
| M4 dataflow | 5-stage DATAFLOW | 2905x1763 | 286 |
| M4 FSM | 5-state FSM | 2705x1423 | 181 |
| M4 microarch | RTL datapath | 3045x1923 | 375 |

**Total render time**: ~50 seconds for all 17 diagrams (~3 seconds each).
**All 17 rendered successfully** with no failures. GPU errors in output are harmless.

### Key Findings

1. **Uniform format validated**: All 4 diagram types (system, DATAFLOW, FSM, RTL microarch) rendered from the same XML format. No dual-format workaround needed.
2. **RTL microarch quality**: The 5 microarch diagrams (which required hand-crafted SVG with excalidraw) rendered with comparable quality from draw.io XML.
3. **CLI rendering reliability**: 17/17 success rate. The AppImage approach is deterministic and headless.
4. **Resolution**: 2x scale produces ~2500-3000px wide images, suitable for documentation and presentations.
