# Draw.io mxCell Element Reference

Complete property reference for constructing Draw.io (.drawio) XML diagrams programmatically.

---

## 1. mxCell Overview

Two fundamental cell types: **vertex** (shapes) and **edge** (connections). Every cell requires:

| Attribute | Required | Description |
|-----------|----------|-------------|
| `id`      | Yes      | Unique identifier. `"0"` and `"1"` are reserved. |
| `parent`  | Yes      | Parent cell ID. Usually `"1"` (default layer). |
| `vertex`  | Vertex   | Set to `"1"` to declare a shape. |
| `edge`    | Edge     | Set to `"1"` to declare a connection. |

```xml
<mxfile>
  <diagram name="Page-1" id="page1">
    <mxGraphModel>
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- user cells here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## 2. Vertex Properties

| Attribute | Type   | Description |
|-----------|--------|-------------|
| `value`   | String | Display text. Supports HTML: `&lt;b&gt;`, `&lt;i&gt;`, `&lt;br&gt;`. |
| `style`   | String | Semicolon-separated style properties (see Section 6). |
| `vertex`  | `"1"`  | Marks this cell as a shape. |
| `parent`  | String | `"1"` for default layer; group ID for grouped children. |

```xml
<mxCell id="node1" value="Hello World" style="rounded=1;whiteSpace=wrap;html=1;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="50" width="120" height="60" as="geometry"/>
</mxCell>
```

---

## 3. mxGeometry for Vertices

Position and size on canvas. Top-left origin, Y increases downward.

| Attribute | Type   | Description |
|-----------|--------|-------------|
| `x`       | Number | Left edge X position (pixels). |
| `y`       | Number | Top edge Y position (pixels). |
| `width`   | Number | Width in pixels. |
| `height`  | Number | Height in pixels. |
| `as`      | String | Always `"geometry"`. |

---

## 4. Edge Properties

| Attribute | Type   | Description |
|-----------|--------|-------------|
| `edge`    | `"1"`  | Marks this cell as a connection. |
| `source`  | String | ID of the source vertex. |
| `target`  | String | ID of the target vertex. |
| `value`   | String | Edge label text. |
| `style`   | String | Edge style string (routing, arrows, color). |

```xml
<mxCell id="edge1" value="data_out" style="edgeStyle=orthogonalEdgeStyle;"
        edge="1" source="node1" target="node2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Floating edge (no source/target) - use `mxPoint` with `as="sourcePoint"` / `as="targetPoint"`:

```xml
<mxCell id="edge2" style="edgeStyle=orthogonalEdgeStyle;" edge="1" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="100" y="200" as="sourcePoint"/>
    <mxPoint x="300" y="200" as="targetPoint"/>
  </mxGeometry>
</mxCell>
```

---

## 5. mxGeometry for Edges

Edge geometry uses `relative="1"` and optional waypoints.

| Child Element | `as` value      | Description |
|---------------|-----------------|-------------|
| `mxPoint`     | `"sourcePoint"` | Override source connection point. |
| `mxPoint`     | `"targetPoint"` | Override target connection point. |
| `mxPoint`     | `"offset"`      | Label offset from midpoint. |
| `Array`       | `"points"`      | Waypoint list for manual routing. |

```xml
<mxGeometry relative="1" as="geometry">
  <Array as="points">
    <mxPoint x="300" y="100"/>
    <mxPoint x="300" y="250"/>
  </Array>
  <mxPoint x="0" y="-10" as="offset"/>
</mxGeometry>
```

---

## 6. Style String Syntax

Semicolon-separated `key=value` pairs. Always end with trailing semicolon.

```
rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;
```

Common vertex style keys:

| Key          | Values                    | Description |
|--------------|---------------------------|-------------|
| `rounded`    | `0`, `1`                  | Rounded corners. |
| `shadow`     | `0`, `1`                  | Drop shadow. |
| `html`       | `1`                       | Enable HTML in value. |
| `whiteSpace` | `wrap`                    | Wrap text inside shape. |
| `aspect`     | `fixed`                   | Lock aspect ratio. |
| `arcSize`    | Number                    | Corner radius. |
| `rotation`   | Degrees                   | Rotate shape. |
| `direction`  | `south`, `west`, `north`  | Rotate orientation. |

---

## 7. Shape Types

Set shape by placing the keyword in the style string. Default is rectangle.

| Shape         | Style Prefix               | Notes |
|---------------|----------------------------|-------|
| Rectangle     | _(default)_                | `rounded=1;` for rounded corners. |
| Ellipse       | `ellipse;`                 | `aspect=fixed;` for circle. |
| Rhombus       | `rhombus;`                 | Diamond / decision. |
| Cylinder      | `shape=cylinder3;`         | Database / storage. |
| Hexagon       | `shape=hexagon;`           | |
| Triangle      | `triangle;`                | Points right. `direction=south;` for down. |
| Parallelogram | `shape=parallelogram;`     | |
| Trapezoid     | `shape=trapezoid;`         | |
| Cloud         | `ellipse;shape=cloud;`     | |
| Process       | `shape=process;`           | Rectangle with side bars. |
| Document      | `shape=document;`          | Wavy bottom edge. |

```xml
<mxCell id="c1" value="Start" style="ellipse;aspect=fixed;whiteSpace=wrap;html=1;"
        vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="60" height="60" as="geometry"/>
</mxCell>
```

---

## 8. Edge Routing Styles

| Style            | Style Value                              | Behavior |
|------------------|------------------------------------------|----------|
| Orthogonal       | `edgeStyle=orthogonalEdgeStyle;`         | Right-angle segments. |
| Elbow            | `edgeStyle=elbowEdgeStyle;`              | Single elbow bend. |
| Entity Relation  | `edgeStyle=entityRelationEdgeStyle;`     | Perpendicular endpoints. |
| Curved           | `curved=1;`                              | Bezier curves. |
| Isometric        | `edgeStyle=isometricEdgeStyle;`          | 30-degree isometric. |
| Straight         | _(omit edgeStyle)_                       | Direct line. |

Modifiers: `rounded=0|1`, `jettySize=auto|px`, `jumpStyle=arc|gap|sharp`, `jumpSize=N`.

Arrow styles:

| Key          | Values | Description |
|--------------|--------|-------------|
| `endArrow`   | `classic`, `block`, `open`, `oval`, `diamond`, `none` | Target arrow. |
| `startArrow` | _(same)_ | Source arrow. |
| `endFill`    | `0`, `1` | Fill target arrow. |
| `startFill`  | `0`, `1` | Fill source arrow. |
| `endSize`    | Number | Arrow size (px). |

---

## 9. Connection Points

Control where edges attach using normalized 0-1 coordinates on the bounding box.

**Exit** (source): `exitX`, `exitY`, `exitDx`, `exitDy`
**Entry** (target): `entryX`, `entryY`, `entryDx`, `entryDy`

| Position      | X   | Y   |
|---------------|-----|-----|
| Top center    | 0.5 | 0   |
| Bottom center | 0.5 | 1   |
| Left center   | 0   | 0.5 |
| Right center  | 1   | 0.5 |

```xml
<mxCell id="e2"
        style="exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;edgeStyle=orthogonalEdgeStyle;"
        edge="1" source="A" target="B" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

Omit exit/entry properties for automatic perimeter-based connection.

---

## 10. Font Properties

| Key                      | Values                    | Description |
|--------------------------|---------------------------|-------------|
| `fontSize`               | Number (default `12`)     | Font size in points. |
| `fontStyle`              | Bitmask                   | 0=normal, 1=bold, 2=italic, 3=bold+italic, 4=underline. |
| `fontFamily`             | String                    | `Helvetica`, `Times New Roman`, `Courier New`. |
| `fontColor`              | Hex                       | Text color. |
| `labelBackgroundColor`   | Hex or `none`             | Background behind label text. |
| `labelBorderColor`       | Hex or `none`             | Border around label. |
| `labelPosition`          | `left`, `center`, `right` | Horizontal label anchor relative to shape. |
| `verticalLabelPosition`  | `top`, `middle`, `bottom` | Vertical label anchor relative to shape. |

---

## 11. Fill and Stroke

| Key                 | Values                       | Description |
|---------------------|------------------------------|-------------|
| `fillColor`         | Hex or `none`                | Shape fill. |
| `strokeColor`       | Hex or `none`                | Border color. |
| `strokeWidth`       | Number (default `1`)         | Border thickness. |
| `dashed`            | `0`, `1`                     | Dashed border. |
| `dashPattern`       | `"8 8"`, `"12 4"`            | Dash pattern. |
| `opacity`           | 0-100                        | Overall opacity. |
| `fillOpacity`       | 0-100                        | Fill-only opacity. |
| `gradientColor`     | Hex                          | Second gradient color. |
| `gradientDirection` | `south`, `east`, `north`, `west` | Gradient direction. |

Default color palettes:

| Color  | Fill      | Stroke    |
|--------|-----------|-----------|
| Blue   | `#dae8fc` | `#6c8ebf` |
| Green  | `#d5e8d4` | `#82b366` |
| Orange | `#ffe6cc` | `#d6b656` |
| Red    | `#f8cecc` | `#b85450` |
| Purple | `#e1d5e7` | `#9673a6` |
| Yellow | `#fff2cc` | `#d6b656` |
| Gray   | `#f5f5f5` | `#666666` |

---

## 12. Text Alignment

| Key              | Values                          | Description |
|------------------|---------------------------------|-------------|
| `align`          | `left`, `center`, `right`       | Horizontal text alignment. |
| `verticalAlign`  | `top`, `middle`, `bottom`       | Vertical text alignment. |
| `whiteSpace`     | `wrap`                          | Enable text wrapping. |
| `overflow`        | `hidden`, `fill`, `visible`, `width` | Overflow behavior. |
| `spacing`        | Number                          | Uniform padding (px). |
| `spacingTop`     | Number                          | Top padding. |
| `spacingBottom`  | Number                          | Bottom padding. |
| `spacingLeft`    | Number                          | Left padding. |
| `spacingRight`   | Number                          | Right padding. |
| `horizontal`     | `0`, `1`                        | `0` for vertical text. |

---

## 13. Grouping

A group is a vertex with `container=1`. Children use `parent="group-id"` and their coordinates are relative to the group's top-left corner.

| Style Key     | Value | Description |
|---------------|-------|-------------|
| `container`   | `1`   | Makes vertex a group container. |
| `collapsible` | `0`   | Disable collapse. `1` to allow folding. |
| `childLayout` | `stackLayout`, `tableLayout` | Auto child arrangement. |

```xml
<!-- Group container -->
<mxCell id="group1" value="Module A"
        style="container=1;collapsible=0;rounded=1;whiteSpace=wrap;html=1;"
        vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="300" height="200" as="geometry"/>
</mxCell>

<!-- Child (coordinates relative to group) -->
<mxCell id="child1" value="Sub-block 1"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;"
        vertex="1" parent="group1">
  <mxGeometry x="20" y="40" width="120" height="50" as="geometry"/>
</mxCell>

<!-- Edge between children inside group -->
<mxCell id="innerEdge" style="edgeStyle=orthogonalEdgeStyle;"
        edge="1" source="child1" target="child2" parent="group1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

---

## 14. Layers

`mxCell id="0"` is the root. `mxCell id="1" parent="0"` is the default layer. Additional layers are cells with `parent="0"`. Layers render in document order (later = on top).

```xml
<root>
  <mxCell id="0"/>
  <mxCell id="layer_bg" value="Background" parent="0"/>
  <mxCell id="layer_fg" value="Foreground" parent="0"/>

  <mxCell id="bg_rect" value="BG" style="fillColor=#f5f5f5;"
          vertex="1" parent="layer_bg">
    <mxGeometry x="10" y="10" width="800" height="500" as="geometry"/>
  </mxCell>

  <mxCell id="fg_box" value="FG" style="fillColor=#dae8fc;"
          vertex="1" parent="layer_fg">
    <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
  </mxCell>
</root>
```

Layer attributes: `visible="0|1"`, `locked="0|1"`, `value="Layer Name"`.

---

## Quick Reference: Complete Vertex

```xml
<mxCell id="full_vertex"
        value="&lt;b&gt;Module&lt;/b&gt;&lt;br&gt;packet_detect"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=12;fontStyle=0;fontColor=#333333;align=center;verticalAlign=middle;shadow=1;strokeWidth=2;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="80" width="160" height="80" as="geometry"/>
</mxCell>
```

## Quick Reference: Complete Edge

```xml
<mxCell id="full_edge" value="hls::stream"
        style="edgeStyle=orthogonalEdgeStyle;rounded=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;strokeColor=#6c8ebf;strokeWidth=2;fontSize=10;labelBackgroundColor=#FFFFFF;endArrow=block;endFill=1;"
        edge="1" source="node_a" target="node_b" parent="1">
  <mxGeometry relative="1" as="geometry">
    <mxPoint x="0" y="-10" as="offset"/>
  </mxGeometry>
</mxCell>
```
