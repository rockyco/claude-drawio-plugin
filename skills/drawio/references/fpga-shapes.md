# FPGA Component Shapes for draw.io

Copy-paste reference for HLS architecture diagrams. Each snippet is a complete mxCell
element ready to drop into a `.drawio` XML file inside a `<root>` element.

All coordinates use `x`, `y` for position and `width`, `height` for size.
Adjust `x` and `y` to place components; sizes are defaults that can be overridden.

---

## 1. DSP48 Block

Computation unit: multiply, multiply-accumulate, pre-add.

```xml
<mxCell id="dsp_1" value="&lt;b&gt;DSP: Multiply x26&lt;/b&gt;&lt;br&gt;sum * coeff (16b)&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;26 DSP48E1 (18x16)&lt;/font&gt;" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="100" width="160" height="60" as="geometry" />
</mxCell>
```

## 2. BRAM Block

Block RAM storage - dual-port, auto-inferred by HLS.

```xml
<mxCell id="bram_1" value="&lt;b&gt;BRAM: coeff_buf&lt;/b&gt;&lt;br&gt;depth=160, 18b&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;1 RAMB18 (no partition)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#e0e7ff;strokeColor=#4f46e5;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="200" width="160" height="60" as="geometry" />
</mxCell>
```

## 3. Shift Register

FIR shift register with ARRAY_PARTITION complete.

```xml
<mxCell id="sr_1" value="&lt;b&gt;Shift Reg: sr[26]&lt;/b&gt;&lt;br&gt;PARTITION complete&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;26 FFs (no BRAM)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#e0e7ff;strokeColor=#4f46e5;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="300" width="160" height="50" as="geometry" />
</mxCell>
```

## 4. Circular Buffer

Circular BRAM buffer - history storage, no partition.

```xml
<mxCell id="circ_1" value="&lt;b&gt;Circ Buf: history[512]&lt;/b&gt;&lt;br&gt;write_idx &amp;amp; 0x1FF&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;1 RAMB18 (circular BRAM)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#e0e7ff;strokeColor=#4f46e5;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="400" width="160" height="60" as="geometry" />
</mxCell>
```

## 5. Pipeline Register

Thin register between pipeline stages - breaks timing paths.

```xml
<mxCell id="reg_1" value="REG" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=9;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="380" y="110" width="20" height="60" as="geometry" />
</mxCell>
```

## 6. MUX

Selection multiplexer - scheduling MUX or data MUX.

```xml
<mxCell id="mux_1" value="MUX" style="shape=rhombus;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=9;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="300" y="120" width="40" height="40" as="geometry" />
</mxCell>
```

## 7. Accumulator

Running accumulator with feedback path.

```xml
<mxCell id="acc_1" value="&lt;b&gt;Accumulator&lt;/b&gt;&lt;br&gt;ap_fixed&amp;lt;32,16&amp;gt;&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;feedback: acc += product&lt;/font&gt;" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#99e9f2;strokeColor=#0c8599;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="500" width="120" height="60" as="geometry" />
</mxCell>
```

## 8. Comparator

Threshold or detection comparator.

```xml
<mxCell id="cmp_1" value="&amp;gt; THR" style="shape=rhombus;whiteSpace=wrap;html=1;fillColor=#ffc9c9;strokeColor=#e03131;fontSize=10;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="350" y="510" width="50" height="50" as="geometry" />
</mxCell>
```

## 9. Truncation Block

Bit truncation or rounding between stages.

```xml
<mxCell id="trunc_1" value="TRUNC 32-&gt;18b" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=10;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="420" y="120" width="80" height="40" as="geometry" />
</mxCell>
```

## 10. Data I/O Port

hls::stream data input or output port.

```xml
<mxCell id="port_data_in" value="data_in&lt;br&gt;&lt;font style='font-size:8px'&gt;complex_t&lt;/font&gt;" style="shape=ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
  <mxGeometry x="40" y="125" width="70" height="30" as="geometry" />
</mxCell>
```

## 11. Control I/O Port

Async control signal port (index_t offset, bool detect).

```xml
<mxCell id="port_ctrl_in" value="ctrl_in&lt;br&gt;&lt;font style='font-size:8px'&gt;index_t&lt;/font&gt;" style="shape=ellipse;whiteSpace=wrap;html=1;fillColor=#fce7f3;strokeColor=#db2777;fontSize=10;" vertex="1" parent="1">
  <mxGeometry x="40" y="225" width="70" height="30" as="geometry" />
</mxCell>
```

## 12. FIR IP Block

hls::FIR IP instance with tap count and sample period.

```xml
<mxCell id="fir_ip_1" value="&lt;b&gt;hls::FIR IP&lt;/b&gt;&lt;br&gt;160 taps, SP=4&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;~14 DSP48E1 post-impl&lt;/font&gt;" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#fef9c3;strokeColor=#ca8a04;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="600" width="180" height="50" as="geometry" />
</mxCell>
```

## 13. CORDIC Block

CORDIC computation - atan2 or sincos.

```xml
<mxCell id="cordic_1" value="&lt;b&gt;CORDIC atan2&lt;/b&gt;&lt;br&gt;16-bit, 16 iterations&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;no DSP, ~200 LUT&lt;/font&gt;" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="680" width="140" height="50" as="geometry" />
</mxCell>
```

## 14. FSM / Controller Block

FSM control logic with state list.

```xml
<mxCell id="fsm_1" value="&lt;b&gt;FSM Controller&lt;/b&gt;&lt;br&gt;&lt;font style='font-size:10px'&gt;WAIT_DATA_READY&lt;br&gt;CHECK&lt;br&gt;EXTRACT&lt;br&gt;PASSTHROUGH&lt;br&gt;DONE&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;verticalAlign=top;" vertex="1" parent="1">
  <mxGeometry x="40" y="400" width="130" height="100" as="geometry" />
</mxCell>
```

## 15. DATAFLOW Stage Border

Dashed rectangle enclosing a DATAFLOW stage.

```xml
<mxCell id="df_extract" value="EXTRACT" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#f8fafc;strokeColor=#94a3b8;fontSize=13;fontStyle=1;verticalAlign=top;align=left;spacingLeft=10;spacingTop=5;" vertex="1" parent="1">
  <mxGeometry x="20" y="60" width="320" height="400" as="geometry" />
</mxCell>
```

## 16. Resource Summary Box

Bottom box showing resource utilization after CSYNTH or impl.

```xml
<mxCell id="resources" value="&lt;table style='font-size:10px;border-collapse:collapse;'&gt;&lt;tr&gt;&lt;td colspan='4' style='font-weight:bold;font-size:11px;padding-bottom:4px;'&gt;Resource Utilization (post-impl)&lt;/td&gt;&lt;/tr&gt;&lt;tr&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;DSP&lt;/b&gt;&lt;/td&gt;&lt;td&gt;25 / 176&lt;/td&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;BRAM&lt;/b&gt;&lt;/td&gt;&lt;td&gt;4 / 140&lt;/td&gt;&lt;/tr&gt;&lt;tr&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;LUT&lt;/b&gt;&lt;/td&gt;&lt;td&gt;2100 / 26600&lt;/td&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;FF&lt;/b&gt;&lt;/td&gt;&lt;td&gt;3200 / 53200&lt;/td&gt;&lt;/tr&gt;&lt;tr&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;WNS&lt;/b&gt;&lt;/td&gt;&lt;td&gt;+0.12 ns&lt;/td&gt;&lt;td style='padding:2px 8px;'&gt;&lt;b&gt;II&lt;/b&gt;&lt;/td&gt;&lt;td&gt;1&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f9fafb;strokeColor=#d1d5db;align=left;verticalAlign=top;spacingLeft=8;spacingTop=6;fontSize=10;" vertex="1" parent="1">
  <mxGeometry x="20" y="700" width="400" height="120" as="geometry" />
</mxCell>
```

## 17. Legend Box

Color key legend for the diagram.

```xml
<mxCell id="legend" value="&lt;b style='font-size:11px;'&gt;Legend&lt;/b&gt;&lt;br&gt;&lt;font style='font-size:10px;'&gt;&lt;font color='#1971c2'&gt;---&lt;/font&gt; Data bus (solid blue)&lt;br&gt;&lt;font color='#e03131'&gt;- - -&lt;/font&gt; Control signal (dashed red)&lt;br&gt;&lt;font color='#4f46e5'&gt;- - -&lt;/font&gt; Memory access (dashed indigo)&lt;br&gt;&lt;font color='#868e96'&gt;. . .&lt;/font&gt; Feedback (dotted gray)&lt;br&gt;&lt;br&gt;&lt;font color='#dbeafe'&gt;[__]&lt;/font&gt; DSP &amp;nbsp; &lt;font color='#e0e7ff'&gt;[__]&lt;/font&gt; BRAM&lt;br&gt;&lt;font color='#fef3c7'&gt;[__]&lt;/font&gt; IP/FSM &amp;nbsp; &lt;font color='#d1fae5'&gt;(o)&lt;/font&gt; Data port&lt;br&gt;&lt;font color='#fce7f3'&gt;(o)&lt;/font&gt; Ctrl port&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f9fafb;strokeColor=#d1d5db;align=left;verticalAlign=top;spacingLeft=8;spacingTop=6;fontSize=10;" vertex="1" parent="1">
  <mxGeometry x="460" y="700" width="260" height="120" as="geometry" />
</mxCell>
```

## 18. FIFO Indicator

Internal FIFO stream between DATAFLOW stages.

```xml
<mxCell id="fifo_1" value="stream d=160" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#eff6ff;strokeColor=#1971c2;fontSize=10;fontStyle=2;" vertex="1" parent="1">
  <mxGeometry x="340" y="250" width="120" height="28" as="geometry" />
</mxCell>
```

## 19. NCO LUT

NCO lookup table - sine/cosine stored in BRAM.

```xml
<mxCell id="nco_1" value="&lt;b&gt;NCO LUT&lt;/b&gt;&lt;br&gt;sin/cos[1024]&lt;br&gt;&lt;font color='#6b7280' style='font-size:9px'&gt;1 RAMB18 (quarter-wave)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;dashed=1;fillColor=#e0e7ff;strokeColor=#4f46e5;fontSize=11;" vertex="1" parent="1">
  <mxGeometry x="200" y="760" width="140" height="60" as="geometry" />
</mxCell>
```

## 20. Splitter Node

Fan-out splitter for scalar control duplication (FP-SPLIT).

```xml
<mxCell id="split_1" value="S" style="shape=rhombus;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=9;fontStyle=1;" vertex="1" parent="1">
  <mxGeometry x="300" y="235" width="30" height="30" as="geometry" />
</mxCell>
```

---

## Arrow Snippets

### Data Bus Arrow (solid blue)

Solid blue arrow for data streams between modules or stages.

```xml
<mxCell id="arrow_data_1" value="data_t x N" style="endArrow=block;endFill=1;html=1;strokeColor=#1971c2;strokeWidth=2;fontSize=10;fontColor=#1971c2;labelBackgroundColor=#ffffff;" edge="1" parent="1" source="port_data_in" target="dsp_1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Control Signal Arrow (dashed red)

Dashed red arrow for async control signals (detect, offset).

```xml
<mxCell id="arrow_ctrl_1" value="detect" style="endArrow=block;endFill=1;html=1;strokeColor=#e03131;strokeWidth=1;dashed=1;fontSize=10;fontColor=#e03131;labelBackgroundColor=#ffffff;" edge="1" parent="1" source="port_ctrl_in" target="fsm_1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Memory Access Arrow (dashed indigo)

Dashed indigo arrow for read/write access to BRAM or buffers.

```xml
<mxCell id="arrow_mem_1" value="rd" style="endArrow=block;endFill=1;html=1;strokeColor=#4f46e5;strokeWidth=1;dashed=1;fontSize=9;fontColor=#4f46e5;labelBackgroundColor=#ffffff;" edge="1" parent="1" source="dsp_1" target="bram_1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### Feedback Arrow (dotted gray)

Dotted gray arrow for accumulator feedback or loop-carried dependencies.

```xml
<mxCell id="arrow_fb_1" value="feedback" style="endArrow=block;endFill=1;html=1;strokeColor=#868e96;strokeWidth=1;dashed=1;dashPattern=2 4;fontSize=9;fontColor=#868e96;labelBackgroundColor=#ffffff;curved=1;" edge="1" parent="1" source="acc_1" target="acc_1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="180" y="560" />
      <mxPoint x="180" y="500" />
    </Array>
  </mxGeometry>
</mxCell>
```

### Orthogonal Routed Arrow (with waypoints)

Right-angle routed arrow using explicit waypoints for clean layout.

```xml
<mxCell id="arrow_orth_1" value="" style="endArrow=block;endFill=1;html=1;strokeColor=#1971c2;strokeWidth=2;edgeStyle=orthogonalEdgeStyle;rounded=1;" edge="1" parent="1" source="dsp_1" target="acc_1">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="280" y="180" />
      <mxPoint x="280" y="480" />
    </Array>
  </mxGeometry>
</mxCell>
```

---

## Minimal Skeleton

Wrap all components inside this skeleton to create a valid `.drawio` file:

```xml
<mxfile host="app.diagrams.net" type="device">
  <diagram name="Module Architecture" id="arch_1">
    <mxGraphModel dx="1200" dy="800" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1600" pageHeight="1200">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- paste component and arrow mxCells here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```
