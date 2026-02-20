# Draw.io Complete Examples with Companion .arch.json

Four complete, working .drawio XML examples using the FPGA color palette. Each can be saved as a `.drawio` file and opened directly in draw.io web or desktop.

## FPGA Color Palette Reference

| Role | Fill | Stroke | Extra |
|------|------|--------|-------|
| DSP / Compute | `#dbeafe` | `#1971c2` | - |
| Memory / Buffer | `#e0e7ff` | `#4f46e5` | `dashed=1` |
| Control / FSM | `#fef3c7` | `#d97706` | - |
| Data I/O port | `#d1fae5` | `#059669` | `shape=ellipse` |
| Control I/O port | `#fce7f3` | `#db2777` | `shape=ellipse` |
| Stage border | `#f8fafc` | `#94a3b8` | `dashed=1` |
| Accumulator | `#99e9f2` | `#0c8599` | - |
| Pipeline register | `#e9ecef` | `#868e96` | - |
| Data arrow | - | `#1971c2` | `strokeWidth=2` |
| Control arrow | - | `#e03131` | `dashed=1` |
| Legend box | `#f8f9fa` | `#495057` | - |
| Splitter | `#e9ecef` | `#495057` | - |

---

## Example 1: System Block Diagram (5-Module Topology)

A simplified FPGA system with 5 modules connected by hls::stream data and control signals. Input on the left, output on the right. M1 produces an async start_offset control signal that is duplicated (FP-SPLIT) to M2 and M4. M3 produces fine_offset to M4.

```xml
<mxfile>
  <diagram name="System Block Diagram" id="sys-block">
    <mxGraphModel dx="1400" dy="500" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="0" pageScale="1" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- Data I/O Ports -->
        <mxCell id="io-data-in" value="&lt;b&gt;data_in&lt;/b&gt;&lt;br&gt;complex_t" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="200" width="100" height="60" as="geometry"/>
        </mxCell>

        <mxCell id="io-data-out" value="&lt;b&gt;data_out&lt;/b&gt;&lt;br&gt;complex_t" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1280" y="200" width="100" height="60" as="geometry"/>
        </mxCell>

        <!-- Control output ports -->
        <mxCell id="io-coarse-freq" value="&lt;b&gt;coarseFreqOff&lt;/b&gt;&lt;br&gt;freq_t" style="ellipse;whiteSpace=wrap;html=1;fillColor=#fce7f3;strokeColor=#db2777;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1280" y="80" width="100" height="60" as="geometry"/>
        </mxCell>

        <mxCell id="io-fine-offset" value="&lt;b&gt;fineOffset&lt;/b&gt;&lt;br&gt;index_t" style="ellipse;whiteSpace=wrap;html=1;fillColor=#fce7f3;strokeColor=#db2777;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1280" y="340" width="100" height="60" as="geometry"/>
        </mxCell>

        <!-- Module 0: Prefilter -->
        <mxCell id="m0-prefilter" value="&lt;b&gt;M0: Prefilter&lt;/b&gt;&lt;br&gt;51-tap symmetric FIR&lt;br&gt;II=1" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=11;arcSize=12;" vertex="1" parent="1">
          <mxGeometry x="160" y="190" width="150" height="80" as="geometry"/>
        </mxCell>

        <!-- Module 1: Packet Detect -->
        <mxCell id="m1-packet-detect" value="&lt;b&gt;M1: Packet Detect&lt;/b&gt;&lt;br&gt;Detection + passthrough&lt;br&gt;ALL N samples + startOffset" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=11;arcSize=12;" vertex="1" parent="1">
          <mxGeometry x="360" y="190" width="170" height="80" as="geometry"/>
        </mxCell>

        <!-- Splitter: startOffset duplicator -->
        <mxCell id="split-offset" value="&lt;b&gt;SPLIT&lt;/b&gt;&lt;br&gt;startOffset" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#495057;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="580" y="310" width="80" height="50" as="geometry"/>
        </mxCell>

        <!-- Module 2: Coarse CFO -->
        <mxCell id="m2-coarse-cfo" value="&lt;b&gt;M2: Coarse CFO&lt;/b&gt;&lt;br&gt;FSM + DATAFLOW&lt;br&gt;EXTRACT/COMPUTE/APPLY" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=12;" vertex="1" parent="1">
          <mxGeometry x="700" y="150" width="170" height="80" as="geometry"/>
        </mxCell>

        <!-- Module 3: Fine Sync -->
        <mxCell id="m3-fine-sync" value="&lt;b&gt;M3: Fine Sync&lt;/b&gt;&lt;br&gt;3x hls::FIR IP&lt;br&gt;160-tap Karatsuba" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=11;arcSize=12;" vertex="1" parent="1">
          <mxGeometry x="920" y="150" width="160" height="80" as="geometry"/>
        </mxCell>

        <!-- Module 4: Fine CFO Apply -->
        <mxCell id="m4-fine-cfo" value="&lt;b&gt;M4: Fine CFO Apply&lt;/b&gt;&lt;br&gt;FSM + DATAFLOW&lt;br&gt;NCO correction" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=12;" vertex="1" parent="1">
          <mxGeometry x="1080" y="200" width="160" height="80" as="geometry"/>
        </mxCell>

        <!-- Data edges (blue, solid) -->
        <mxCell id="edge-in-m0" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="io-data-in" target="m0-prefilter" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-m0-m1" value="26105" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m0-prefilter" target="m1-packet-detect" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-m1-m2" value="26105" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m1-packet-detect" target="m2-coarse-cfo" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-m2-m3" value="640" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m2-coarse-cfo" target="m3-fine-sync" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-m1-m4-data" value="26105" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m1-packet-detect" target="m4-fine-cfo" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="550" y="400"/>
              <mxPoint x="1060" y="400"/>
              <mxPoint x="1060" y="240"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <mxCell id="edge-m4-out" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="m4-fine-cfo" target="io-data-out" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Control edges (red, dashed) -->
        <mxCell id="edge-m1-split" value="startOffset" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m1-packet-detect" target="split-offset" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-split-m2" value="to M2" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="split-offset" target="m2-coarse-cfo" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-split-m4" value="to M4" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="split-offset" target="m4-fine-cfo" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="620" y="430"/>
              <mxPoint x="1160" y="430"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <mxCell id="edge-m2-freq" value="coarseFreqOff" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m2-coarse-cfo" target="io-coarse-freq" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="785" y="100"/>
              <mxPoint x="1280" y="100"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <mxCell id="edge-m3-m4-ctrl" value="fineOffset" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m3-fine-sync" target="m4-fine-cfo" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="1000" y="280"/>
              <mxPoint x="1160" y="280"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <mxCell id="edge-m3-fineout" value="fineOffset" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="m3-fine-sync" target="io-fine-offset" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="1100" y="370"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- Legend -->
        <mxCell id="legend-box" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#495057;dashed=0;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="20" width="260" height="120" as="geometry"/>
        </mxCell>
        <mxCell id="legend-title" value="&lt;b&gt;Legend&lt;/b&gt;" style="text;html=1;align=left;verticalAlign=top;fontSize=11;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="30" y="25" width="80" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-data-line" value="" style="endArrow=block;endFill=1;strokeColor=#1971c2;strokeWidth=2;html=1;" edge="1" parent="1">
          <mxGeometry relative="1" as="geometry">
            <mxPoint x="30" y="60" as="sourcePoint"/>
            <mxPoint x="80" y="60" as="targetPoint"/>
          </mxGeometry>
        </mxCell>
        <mxCell id="legend-data-text" value="Data stream (hls::stream)" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="90" y="50" width="160" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-ctrl-line" value="" style="endArrow=block;endFill=1;strokeColor=#e03131;strokeWidth=1;dashed=1;html=1;" edge="1" parent="1">
          <mxGeometry relative="1" as="geometry">
            <mxPoint x="30" y="85" as="sourcePoint"/>
            <mxPoint x="80" y="85" as="targetPoint"/>
          </mxGeometry>
        </mxCell>
        <mxCell id="legend-ctrl-text" value="Control signal (async)" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="90" y="75" width="160" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-fsm-box" value="FSM" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="30" y="100" width="50" height="25" as="geometry"/>
        </mxCell>
        <mxCell id="legend-fsm-text" value="Module with FSM + DATAFLOW" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="90" y="102" width="180" height="20" as="geometry"/>
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Companion `.arch.json`:

```json
{
  "title": "wlanSync - System Block Diagram",
  "type": "system_block",
  "phase": 1,
  "version": "5.0.2",
  "notes": [
    "5-module WLAN synchronization pipeline",
    "M1 write-through + startOffset duplicator fans to M2 + M4"
  ],
  "nodes": {
    "module0_prefilter": {
      "name": "M0: Prefilter",
      "desc": "51-tap symmetric pre-add FIR (FP-SYMCOEFF)",
      "csr": 1.0
    },
    "module1_packet_detect": {
      "name": "M1: Packet Detect",
      "desc": "L-STF detection + write-through ALL samples + startOffset",
      "csr": 1.0
    },
    "module2_coarse_cfo": {
      "name": "M2: Coarse CFO",
      "desc": "Coarse CFO estimation + correction (FSM + DATAFLOW)",
      "csr": 4.0,
      "fsm": true,
      "dataflow": true
    },
    "module3_fine_sync": {
      "name": "M3: Fine Sync",
      "desc": "Fine timing via L-LTF cross-correlation (3x hls::FIR IP, SP=16)",
      "csr": 1.0
    },
    "module4_fine_cfo_apply": {
      "name": "M4: Fine CFO Apply",
      "desc": "Fine CFO estimation + correction (FSM + DATAFLOW)",
      "csr": 162.7,
      "fsm": true,
      "dataflow": true
    }
  },
  "edges": [
    {"from": "INPUT", "to": "module0_prefilter", "type": "data", "label": "data_in (complex_t, 26155 samples)"},
    {"from": "module0_prefilter", "to": "module1_packet_detect", "type": "data", "label": "26105 samples"},
    {"from": "module1_packet_detect", "to": "module2_coarse_cfo", "type": "data", "label": "26105 samples"},
    {"from": "module1_packet_detect", "to": "module4_fine_cfo_apply", "type": "data", "label": "26105 samples"},
    {"from": "module1_packet_detect", "to": "startOffset_splitter", "type": "ctrl", "label": "startOffset (index_t)"},
    {"from": "startOffset_splitter", "to": "module2_coarse_cfo", "type": "ctrl", "label": "startOffset_to_m2 (async)"},
    {"from": "startOffset_splitter", "to": "module4_fine_cfo_apply", "type": "ctrl", "label": "startOffset_to_m4 (async)"},
    {"from": "module2_coarse_cfo", "to": "module3_fine_sync", "type": "data", "label": "640 samples"},
    {"from": "module2_coarse_cfo", "to": "OUTPUT", "type": "ctrl", "label": "coarseFreqOff (freq_t)"},
    {"from": "module3_fine_sync", "to": "module4_fine_cfo_apply", "type": "ctrl", "label": "fineOffset (async, index_t)"},
    {"from": "module3_fine_sync", "to": "OUTPUT", "type": "ctrl", "label": "fineOffset (index_t)"},
    {"from": "module4_fine_cfo_apply", "to": "OUTPUT", "type": "data", "label": "data_out (complex_t)"}
  ],
  "control_signals": {
    "startOffset": {
      "producer": "module1_packet_detect",
      "consumers": ["module2_coarse_cfo", "module4_fine_cfo_apply"],
      "async": true,
      "type": "index_t",
      "handling_pattern": "FSM_POLL"
    },
    "fineOffset": {
      "producer": "module3_fine_sync",
      "consumers": ["module4_fine_cfo_apply"],
      "async": true,
      "type": "index_t",
      "handling_pattern": "FSM_POLL"
    },
    "coarseFreqOff": {
      "producer": "module2_coarse_cfo",
      "consumers": ["OUTPUT"],
      "async": false,
      "type": "freq_t",
      "handling_pattern": "INLINE_OUTPUT"
    }
  },
  "patterns": {
    "module0_prefilter": ["FP-SYMCOEFF", "FP-PRAGMA"],
    "module1_packet_detect": ["FP-DETECTION", "FP-NOBREAK"],
    "module2_coarse_cfo": ["FP-ASYNC", "FP-SEPARATION"],
    "module3_fine_sync": ["FP-CORR"],
    "module4_fine_cfo_apply": ["FP-ASYNC", "FP-SEPARATION"]
  }
}
```

---

## Example 2: DATAFLOW Decomposition (3-Stage EXTRACT/COMPUTE/APPLY)

Internal DATAFLOW architecture of a coarse CFO estimation module. Three dashed stage borders contain the EXTRACT (FSM + circular buffer), COMPUTE (autocorrelation + atan2), and APPLY (NCO correction) stages. Internal hls::stream FIFOs connect stages. A bypass passthrough stream routes data from EXTRACT directly to APPLY, skipping COMPUTE.

```xml
<mxfile>
  <diagram name="DATAFLOW Decomposition" id="dataflow-3stage">
    <mxGraphModel dx="1200" dy="500" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="0" pageScale="1" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- Title -->
        <mxCell id="title" value="&lt;b&gt;module2_coarse_cfo - Internal DATAFLOW&lt;/b&gt;&lt;br&gt;#pragma HLS DATAFLOW" style="text;html=1;align=left;verticalAlign=top;fontSize=13;fontStyle=0;" vertex="1" parent="1">
          <mxGeometry x="20" y="10" width="400" height="40" as="geometry"/>
        </mxCell>

        <!-- I/O Ports -->
        <mxCell id="io-data-in" value="&lt;b&gt;data_in&lt;/b&gt;&lt;br&gt;complex_t&lt;br&gt;26105 samples" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="140" width="100" height="70" as="geometry"/>
        </mxCell>

        <mxCell id="io-ctrl-offset" value="&lt;b&gt;startOffset_in&lt;/b&gt;&lt;br&gt;index_t&lt;br&gt;(async)" style="ellipse;whiteSpace=wrap;html=1;fillColor=#fce7f3;strokeColor=#db2777;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="60" width="100" height="70" as="geometry"/>
        </mxCell>

        <mxCell id="io-data-out" value="&lt;b&gt;data_out&lt;/b&gt;&lt;br&gt;complex_t&lt;br&gt;640 samples" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1080" y="140" width="100" height="70" as="geometry"/>
        </mxCell>

        <mxCell id="io-freq-out" value="&lt;b&gt;freq_out&lt;/b&gt;&lt;br&gt;freq_t" style="ellipse;whiteSpace=wrap;html=1;fillColor=#fce7f3;strokeColor=#db2777;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1080" y="60" width="100" height="70" as="geometry"/>
        </mxCell>

        <!-- Stage 1: EXTRACT border -->
        <mxCell id="stage-extract-border" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8fafc;strokeColor=#94a3b8;dashed=1;strokeWidth=2;" vertex="1" parent="1">
          <mxGeometry x="150" y="50" width="280" height="400" as="geometry"/>
        </mxCell>
        <mxCell id="stage-extract-label" value="&lt;b&gt;EXTRACT&lt;/b&gt;&lt;br&gt;II=1" style="text;html=1;align=center;verticalAlign=top;fontSize=11;fontColor=#94a3b8;" vertex="1" parent="1">
          <mxGeometry x="240" y="55" width="100" height="30" as="geometry"/>
        </mxCell>

        <!-- Stage 2: COMPUTE border -->
        <mxCell id="stage-compute-border" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8fafc;strokeColor=#94a3b8;dashed=1;strokeWidth=2;" vertex="1" parent="1">
          <mxGeometry x="470" y="50" width="260" height="200" as="geometry"/>
        </mxCell>
        <mxCell id="stage-compute-label" value="&lt;b&gt;COMPUTE&lt;/b&gt;&lt;br&gt;II=1" style="text;html=1;align=center;verticalAlign=top;fontSize=11;fontColor=#94a3b8;" vertex="1" parent="1">
          <mxGeometry x="550" y="55" width="100" height="30" as="geometry"/>
        </mxCell>

        <!-- Stage 3: APPLY border -->
        <mxCell id="stage-apply-border" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8fafc;strokeColor=#94a3b8;dashed=1;strokeWidth=2;" vertex="1" parent="1">
          <mxGeometry x="770" y="50" width="280" height="400" as="geometry"/>
        </mxCell>
        <mxCell id="stage-apply-label" value="&lt;b&gt;APPLY&lt;/b&gt;&lt;br&gt;II=1" style="text;html=1;align=center;verticalAlign=top;fontSize=11;fontColor=#94a3b8;" vertex="1" parent="1">
          <mxGeometry x="860" y="55" width="100" height="30" as="geometry"/>
        </mxCell>

        <!-- EXTRACT internals -->
        <mxCell id="extract-fsm" value="&lt;b&gt;FSM&lt;/b&gt;&lt;br&gt;WAIT_CTRL&lt;br&gt;WAIT_DATA&lt;br&gt;EXTRACT&lt;br&gt;DONE" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=10;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="180" y="100" width="110" height="100" as="geometry"/>
        </mxCell>

        <mxCell id="extract-circbuf" value="&lt;b&gt;circ_buf[2048]&lt;/b&gt;&lt;br&gt;complex_t&lt;br&gt;BRAM" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;dashed=1;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="310" y="100" width="100" height="80" as="geometry"/>
        </mxCell>

        <!-- COMPUTE internals -->
        <mxCell id="compute-autocorr" value="&lt;b&gt;autocorrelation&lt;/b&gt;&lt;br&gt;160 L-STF samples&lt;br&gt;16-tap shift reg" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=10;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="490" y="100" width="130" height="70" as="geometry"/>
        </mxCell>

        <mxCell id="compute-atan2" value="&lt;b&gt;atan2&lt;/b&gt;&lt;br&gt;CORDIC" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=10;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="510" y="190" width="100" height="50" as="geometry"/>
        </mxCell>

        <!-- APPLY internals -->
        <mxCell id="apply-nco" value="&lt;b&gt;NCO LUT&lt;/b&gt;&lt;br&gt;1024-entry&lt;br&gt;quarter-wave" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;dashed=1;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="800" y="100" width="110" height="70" as="geometry"/>
        </mxCell>

        <mxCell id="apply-mul" value="&lt;b&gt;Complex MUL&lt;/b&gt;&lt;br&gt;data * exp(-j*phi)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=10;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="920" y="100" width="110" height="70" as="geometry"/>
        </mxCell>

        <!-- Internal FIFOs (drawn as labeled arrows) -->
        <mxCell id="fifo-lstf" value="&lt;b&gt;lstf_stream&lt;/b&gt;&lt;br&gt;160 samples&lt;br&gt;depth=160" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="extract-circbuf" target="compute-autocorr" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="fifo-passthrough" value="&lt;b&gt;passthrough_stream&lt;/b&gt;&lt;br&gt;640 samples, depth=680" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="extract-circbuf" target="apply-mul" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="360" y="370"/>
              <mxPoint x="975" y="370"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <mxCell id="edge-autocorr-atan" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="compute-autocorr" target="compute-atan2" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="fifo-cfo" value="&lt;b&gt;cfo_stream&lt;/b&gt;&lt;br&gt;scalar freq_t&lt;br&gt;depth=4" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;fontSize=9;fontColor=#e03131;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="compute-atan2" target="apply-nco" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="750" y="215"/>
              <mxPoint x="750" y="135"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- External I/O connections -->
        <mxCell id="edge-datain-fsm" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="io-data-in" target="extract-fsm" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-ctrl-fsm" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;html=1;" edge="1" source="io-ctrl-offset" target="extract-fsm" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-fsm-circbuf" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="extract-fsm" target="extract-circbuf" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-nco-mul" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="apply-nco" target="apply-mul" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-mul-dataout" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="apply-mul" target="io-data-out" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-atan-freqout" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#e03131;strokeWidth=1;dashed=1;endArrow=block;endFill=1;html=1;" edge="1" source="compute-atan2" target="io-freq-out" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="560" y="270"/>
              <mxPoint x="1070" y="270"/>
              <mxPoint x="1070" y="95"/>
            </Array>
          </mxGeometry>
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Companion `.arch.json`:

```json
{
  "title": "module2_coarse_cfo - DATAFLOW Decomposition",
  "type": "dataflow",
  "phase": 5,
  "version": "5.0.2",
  "notes": [
    "3-stage: EXTRACT(FSM+circ_buf[2048]) -> COMPUTE(autocorr+atan2) -> APPLY(NCO correction)",
    "EXTRACT uses FP-ASYNC: blocking data read, non-blocking startOffset poll"
  ],
  "nodes": {
    "extract_with_offset": {
      "name": "extract_with_offset",
      "desc": "EXTRACT: FSM + circ_buf[2048], polls startOffset, outputs L-STF(160) + passthrough(640)"
    },
    "compute_cfo": {
      "name": "compute_cfo",
      "desc": "COMPUTE: autocorrelation on 160 L-STF samples, CORDIC atan2 for CFO"
    },
    "apply_correction": {
      "name": "apply_correction",
      "desc": "APPLY: NCO-based quarter-wave frequency correction on 640 passthrough samples"
    }
  },
  "stages": [
    {"name": "extract_with_offset", "role": "EXTRACT", "ii": 1},
    {"name": "compute_cfo", "role": "COMPUTE", "ii": 1},
    {"name": "apply_correction", "role": "APPLY", "ii": 1}
  ],
  "edges": [
    {"from": "extract_with_offset", "to": "compute_cfo", "type": "data", "label": "lstf_stream (160 samples, depth=160)"},
    {"from": "extract_with_offset", "to": "apply_correction", "type": "data", "label": "passthrough_stream (640 samples, depth=680)"},
    {"from": "compute_cfo", "to": "apply_correction", "type": "ctrl", "label": "cfo_stream (scalar freq_t, depth=4)"}
  ],
  "control_inputs": {
    "startOffset_in": {
      "type": "index_t",
      "async": true,
      "handling": "FSM_POLL",
      "estimated_arrival_cycles": 200,
      "producer": "module1_packet_detect"
    }
  }
}
```

---

## Example 3: FSM State Diagram (4-State Machine)

A 4-state FSM for an async control consumer module. States flow left-to-right: WAIT_CTRL (initial), WAIT_DATA, EXTRACT, DONE (terminal). Each state has a self-loop with its guard condition and a forward transition. The WAIT_CTRL state polls the control input non-blockingly; WAIT_DATA waits for the data stream position to reach the extraction offset; EXTRACT reads W samples from a circular buffer; DONE consumes remaining input.

```xml
<mxfile>
  <diagram name="FSM State Diagram" id="fsm-4state">
    <mxGraphModel dx="1000" dy="500" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="0" pageScale="1" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- Title -->
        <mxCell id="title" value="&lt;b&gt;FSM State Diagram - Async Control Consumer&lt;/b&gt;&lt;br&gt;FP-ASYNC pattern: blocking data read + non-blocking control poll" style="text;html=1;align=left;verticalAlign=top;fontSize=12;" vertex="1" parent="1">
          <mxGeometry x="20" y="10" width="500" height="40" as="geometry"/>
        </mxCell>

        <!-- Initial marker (small filled circle) -->
        <mxCell id="fsm-init" value="" style="ellipse;whiteSpace=wrap;html=1;fillColor=#000000;strokeColor=#000000;aspect=fixed;" vertex="1" parent="1">
          <mxGeometry x="40" y="195" width="20" height="20" as="geometry"/>
        </mxCell>

        <!-- State: WAIT_CTRL -->
        <mxCell id="state-wait-ctrl" value="&lt;b&gt;WAIT_CTRL&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;blocking data_in.read()&lt;br&gt;non-blocking ctrl poll&lt;br&gt;write to circ_buf&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=15;" vertex="1" parent="1">
          <mxGeometry x="90" y="160" width="160" height="90" as="geometry"/>
        </mxCell>

        <!-- State: WAIT_DATA -->
        <mxCell id="state-wait-data" value="&lt;b&gt;WAIT_DATA&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;continue filling circ_buf&lt;br&gt;until n &amp;gt;= offset + W - 1&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=15;" vertex="1" parent="1">
          <mxGeometry x="330" y="160" width="160" height="90" as="geometry"/>
        </mxCell>

        <!-- State: EXTRACT -->
        <mxCell id="state-extract" value="&lt;b&gt;EXTRACT&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;read W samples&lt;br&gt;from circ_buf&lt;br&gt;write to output stream&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=15;" vertex="1" parent="1">
          <mxGeometry x="570" y="160" width="160" height="90" as="geometry"/>
        </mxCell>

        <!-- State: DONE -->
        <mxCell id="state-done" value="&lt;b&gt;DONE&lt;/b&gt;&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;consume remaining&lt;br&gt;data_in samples&lt;br&gt;(write nothing)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=11;arcSize=15;" vertex="1" parent="1">
          <mxGeometry x="810" y="160" width="160" height="90" as="geometry"/>
        </mxCell>

        <!-- Terminal marker (double circle) -->
        <mxCell id="fsm-terminal" value="" style="ellipse;whiteSpace=wrap;html=1;fillColor=none;strokeColor=#000000;aspect=fixed;strokeWidth=3;" vertex="1" parent="1">
          <mxGeometry x="870" y="290" width="30" height="30" as="geometry"/>
        </mxCell>

        <!-- Initial transition -->
        <mxCell id="edge-init" value="reset" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#495057;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="fsm-init" target="state-wait-ctrl" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Self-loop: WAIT_CTRL -->
        <mxCell id="self-wait-ctrl" value="!ctrl_in.empty()&lt;br&gt;false" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#d97706;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;fontColor=#d97706;labelBackgroundColor=#FFFFFF;html=1;exitX=0.25;exitY=0;exitDx=0;exitDy=0;entryX=0.75;entryY=0;entryDx=0;entryDy=0;" edge="1" source="state-wait-ctrl" target="state-wait-ctrl" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="130" y="110"/>
              <mxPoint x="210" y="110"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- Transition: WAIT_CTRL -> WAIT_DATA -->
        <mxCell id="edge-wc-wd" value="ctrl_received" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#495057;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="state-wait-ctrl" target="state-wait-data" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Self-loop: WAIT_DATA -->
        <mxCell id="self-wait-data" value="n &lt; offset&lt;br&gt;+ W - 1" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#d97706;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;fontColor=#d97706;labelBackgroundColor=#FFFFFF;html=1;exitX=0.25;exitY=0;exitDx=0;exitDy=0;entryX=0.75;entryY=0;entryDx=0;entryDy=0;" edge="1" source="state-wait-data" target="state-wait-data" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="370" y="110"/>
              <mxPoint x="450" y="110"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- Transition: WAIT_DATA -> EXTRACT -->
        <mxCell id="edge-wd-ex" value="n &gt;= offset + W - 1" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#495057;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="state-wait-data" target="state-extract" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Self-loop: EXTRACT -->
        <mxCell id="self-extract" value="extracted&lt;br&gt;&lt; W" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#d97706;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;fontColor=#d97706;labelBackgroundColor=#FFFFFF;html=1;exitX=0.25;exitY=0;exitDx=0;exitDy=0;entryX=0.75;entryY=0;entryDx=0;entryDy=0;" edge="1" source="state-extract" target="state-extract" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="610" y="110"/>
              <mxPoint x="690" y="110"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- Transition: EXTRACT -> DONE -->
        <mxCell id="edge-ex-done" value="extracted == W" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#495057;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="state-extract" target="state-done" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Self-loop: DONE -->
        <mxCell id="self-done" value="n &lt;&lt;br&gt;num_samples" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#d97706;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;fontColor=#d97706;labelBackgroundColor=#FFFFFF;html=1;exitX=0.25;exitY=0;exitDx=0;exitDy=0;entryX=0.75;entryY=0;entryDx=0;entryDy=0;" edge="1" source="state-done" target="state-done" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="850" y="110"/>
              <mxPoint x="930" y="110"/>
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- Transition: DONE -> terminal -->
        <mxCell id="edge-done-term" value="n == num_samples" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#495057;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="state-done" target="fsm-terminal" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Legend -->
        <mxCell id="legend-box" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#495057;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="370" width="260" height="100" as="geometry"/>
        </mxCell>
        <mxCell id="legend-title" value="&lt;b&gt;Legend&lt;/b&gt;" style="text;html=1;align=left;verticalAlign=top;fontSize=11;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="30" y="375" width="80" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-state-box" value="STATE" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fef3c7;strokeColor=#d97706;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="30" y="400" width="60" height="25" as="geometry"/>
        </mxCell>
        <mxCell id="legend-state-text" value="FSM state (enum + switch)" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="100" y="402" width="170" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-self-text" value="Self-loop (guard condition)" style="text;html=1;align=left;fontSize=9;fontColor=#d97706;" vertex="1" parent="1">
          <mxGeometry x="100" y="427" width="170" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-trans-text" value="Forward transition" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="100" y="447" width="170" height="20" as="geometry"/>
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Companion `.arch.json`:

```json
{
  "title": "module2_coarse_cfo - FSM State Diagram",
  "type": "fsm_states",
  "phase": 5,
  "version": "5.0.2",
  "notes": [
    "FP-ASYNC pattern: blocking data read + non-blocking startOffset poll",
    "circ_buf[2048] stores data for replay at extraction offset"
  ],
  "nodes": {
    "state-wait-ctrl": {
      "name": "WAIT_CTRL",
      "desc": "Blocking data_in.read() into circ_buf, non-blocking startOffset_in poll, II=1"
    },
    "state-wait-data": {
      "name": "WAIT_DATA",
      "desc": "Continue filling circ_buf until n >= start_off + W - 1"
    },
    "state-extract": {
      "name": "EXTRACT",
      "desc": "Read W samples from circ_buf -> output stream"
    },
    "state-done": {
      "name": "DONE",
      "desc": "Consume remaining data_in samples (write nothing downstream)"
    },
    "buf-circ": {
      "name": "circ_buf[2048]",
      "desc": "Circular buffer, complex_t, BRAM, BUF_MASK=2047"
    }
  },
  "states": [
    {
      "name": "WAIT_CTRL",
      "transitions": [
        {"to": "WAIT_CTRL", "condition": "!ctrl_in.empty() == false (self-loop: keep polling)"},
        {"to": "WAIT_DATA", "condition": "ctrl_received == true"}
      ]
    },
    {
      "name": "WAIT_DATA",
      "transitions": [
        {"to": "WAIT_DATA", "condition": "n < offset + W - 1 (self-loop)"},
        {"to": "EXTRACT", "condition": "n >= offset + W - 1"}
      ]
    },
    {
      "name": "EXTRACT",
      "transitions": [
        {"to": "EXTRACT", "condition": "extracted < W (self-loop)"},
        {"to": "DONE", "condition": "extracted == W"}
      ]
    },
    {
      "name": "DONE",
      "transitions": [
        {"to": "DONE", "condition": "n < num_samples (self-loop: consume remaining)"},
        {"to": "TERMINAL", "condition": "n == num_samples"}
      ]
    }
  ],
  "edges": [
    {"from": "WAIT_CTRL", "to": "WAIT_CTRL", "type": "flow", "label": "!ctrl_in.empty() false (self-loop)"},
    {"from": "WAIT_CTRL", "to": "WAIT_DATA", "type": "flow", "label": "ctrl_received = true"},
    {"from": "WAIT_DATA", "to": "WAIT_DATA", "type": "flow", "label": "n < offset + W - 1 (self-loop)"},
    {"from": "WAIT_DATA", "to": "EXTRACT", "type": "flow", "label": "n >= offset + W - 1"},
    {"from": "EXTRACT", "to": "EXTRACT", "type": "flow", "label": "extracted < W (self-loop)"},
    {"from": "EXTRACT", "to": "DONE", "type": "flow", "label": "extracted == W"},
    {"from": "DONE", "to": "DONE", "type": "flow", "label": "n < num_samples (consume remaining)"}
  ]
}
```

---

## Example 4: RTL Micro-Architecture (FIR Filter Datapath)

A detailed hardware datapath for a 51-tap symmetric pre-add FIR filter. Shows the complete pipeline from input port through shift register, pre-addition (exploiting coefficient symmetry to halve DSP usage), multiplication in DSP48E1 blocks, tree accumulation, and truncation to output. Pipeline registers separate stages. This demonstrates that draw.io handles RTL microarchitecture diagrams in the same XML format as all other diagram types.

```xml
<mxfile>
  <diagram name="RTL Micro-Architecture" id="rtl-microarch">
    <mxGraphModel dx="1200" dy="600" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="0" pageScale="1" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>

        <!-- Title -->
        <mxCell id="title" value="&lt;b&gt;module0_prefilter - RTL Micro-Architecture&lt;/b&gt;&lt;br&gt;51-tap Symmetric Pre-Add FIR, II=1, Dual Channel (I/Q)" style="text;html=1;align=left;verticalAlign=top;fontSize=13;" vertex="1" parent="1">
          <mxGeometry x="20" y="10" width="500" height="40" as="geometry"/>
        </mxCell>

        <!-- Input Port -->
        <mxCell id="io-data-in" value="&lt;b&gt;data_in&lt;/b&gt;&lt;br&gt;complex_t&lt;br&gt;16b I + 16b Q" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="20" y="220" width="110" height="70" as="geometry"/>
        </mxCell>

        <!-- Pipeline Register: Input -->
        <mxCell id="preg-in" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=8;" vertex="1" parent="1">
          <mxGeometry x="155" y="195" width="8" height="120" as="geometry"/>
        </mxCell>

        <!-- Shift Register block -->
        <mxCell id="mem-shift-reg" value="&lt;b&gt;SHIFT REGISTER&lt;/b&gt;&lt;br&gt;51 x data_t&lt;br&gt;PARTITION complete&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;51 FFs per channel&lt;br&gt;sr[0] = newest&lt;br&gt;sr[50] = oldest&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;dashed=1;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="185" y="190" width="140" height="130" as="geometry"/>
        </mxCell>

        <!-- Pipeline Register: Post-shift -->
        <mxCell id="preg-shift" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=8;" vertex="1" parent="1">
          <mxGeometry x="345" y="195" width="8" height="120" as="geometry"/>
        </mxCell>

        <!-- Pre-Add block -->
        <mxCell id="dsp-preadd" value="&lt;b&gt;PRE-ADD&lt;/b&gt;&lt;br&gt;sr[k] + sr[50-k]&lt;br&gt;k = 0..24&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;25 pairs + 1 center&lt;br&gt;= 26 unique values&lt;br&gt;17-bit output&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="375" y="190" width="140" height="130" as="geometry"/>
        </mxCell>

        <!-- Coefficient ROM -->
        <mxCell id="mem-coeffs" value="&lt;b&gt;COEFFS[26]&lt;/b&gt;&lt;br&gt;const ROM&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;26 unique symmetric&lt;br&gt;coefficients (coeff_t)&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;dashed=1;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="555" y="80" width="130" height="80" as="geometry"/>
        </mxCell>

        <!-- Pipeline Register: Post-preadd -->
        <mxCell id="preg-preadd" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=8;" vertex="1" parent="1">
          <mxGeometry x="535" y="195" width="8" height="120" as="geometry"/>
        </mxCell>

        <!-- DSP Multiply block -->
        <mxCell id="dsp-mul" value="&lt;b&gt;DSP48E1 MUL&lt;/b&gt;&lt;br&gt;26 x (18b x 16b)&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;26 DSP per channel&lt;br&gt;52 DSP total (I+Q)&lt;br&gt;34-bit product&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="565" y="190" width="140" height="130" as="geometry"/>
        </mxCell>

        <!-- Pipeline Register: Post-mul -->
        <mxCell id="preg-mul" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=8;" vertex="1" parent="1">
          <mxGeometry x="725" y="195" width="8" height="120" as="geometry"/>
        </mxCell>

        <!-- Accumulator block -->
        <mxCell id="acc-tree" value="&lt;b&gt;ACCUMULATE&lt;/b&gt;&lt;br&gt;Tree reduction&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;26-term adder tree&lt;br&gt;accum_t: ap_fixed&amp;lt;36,12&amp;gt;&lt;br&gt;5-level log2(26) tree&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#99e9f2;strokeColor=#0c8599;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="755" y="190" width="140" height="130" as="geometry"/>
        </mxCell>

        <!-- Pipeline Register: Post-acc -->
        <mxCell id="preg-acc" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=8;" vertex="1" parent="1">
          <mxGeometry x="915" y="195" width="8" height="120" as="geometry"/>
        </mxCell>

        <!-- Truncation block -->
        <mxCell id="trunc" value="&lt;b&gt;TRUNCATE&lt;/b&gt;&lt;br&gt;36b -&gt; 16b&lt;br&gt;&lt;font style=&quot;font-size:9px&quot;&gt;accum_t -&gt; data_t&lt;br&gt;saturate + round&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=10;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="945" y="210" width="120" height="90" as="geometry"/>
        </mxCell>

        <!-- Output Port -->
        <mxCell id="io-data-out" value="&lt;b&gt;data_out&lt;/b&gt;&lt;br&gt;complex_t&lt;br&gt;16b I + 16b Q" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="1090" y="220" width="110" height="70" as="geometry"/>
        </mxCell>

        <!-- Critical path annotation -->
        <mxCell id="critical-path-label" value="&lt;font color=&quot;#e03131&quot;&gt;&lt;b&gt;Critical Path:&lt;/b&gt; SHIFT -&gt; PRE-ADD(17b) -&gt; MUL(34b) -&gt; ACC(36b) -&gt; TRUNC(16b)&lt;/font&gt;" style="text;html=1;align=left;verticalAlign=top;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="185" y="350" width="500" height="25" as="geometry"/>
        </mxCell>

        <!-- Data edges (blue, solid, thick) -->
        <mxCell id="edge-in-preg" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="io-data-in" target="preg-in" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preg-sr" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="preg-in" target="mem-shift-reg" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-sr-preg2" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="mem-shift-reg" target="preg-shift" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preg2-preadd" value="16b pairs" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="preg-shift" target="dsp-preadd" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preadd-preg3" value="17b" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="dsp-preadd" target="preg-preadd" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preg3-mul" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="preg-preadd" target="dsp-mul" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-coeff-mul" value="coeff_t" style="edgeStyle=orthogonalEdgeStyle;rounded=1;strokeColor=#4f46e5;strokeWidth=1;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="mem-coeffs" target="dsp-mul" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-mul-preg4" value="34b" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="dsp-mul" target="preg-mul" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preg4-acc" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="preg-mul" target="acc-tree" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-acc-preg5" value="36b" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="acc-tree" target="preg-acc" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-preg5-trunc" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;html=1;" edge="1" source="preg-acc" target="trunc" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <mxCell id="edge-trunc-out" value="16b" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1971c2;strokeWidth=2;endArrow=block;endFill=1;fontSize=9;labelBackgroundColor=#FFFFFF;html=1;" edge="1" source="trunc" target="io-data-out" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>

        <!-- Resource Summary Box -->
        <mxCell id="resource-box" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#495057;dashed=0;" vertex="1" parent="1">
          <mxGeometry x="20" y="400" width="320" height="160" as="geometry"/>
        </mxCell>
        <mxCell id="resource-title" value="&lt;b&gt;Resource Summary&lt;/b&gt;" style="text;html=1;align=left;verticalAlign=top;fontSize=11;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="30" y="405" width="200" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="resource-dsp" value="DSP48E1: 52 (26/channel x 2 channels)" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="430" width="280" height="18" as="geometry"/>
        </mxCell>
        <mxCell id="resource-bram" value="BRAM: 0 (shift regs use FFs via PARTITION)" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="450" width="280" height="18" as="geometry"/>
        </mxCell>
        <mxCell id="resource-ff" value="FF: ~1632 (51 x 16b x 2 channels)" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="470" width="280" height="18" as="geometry"/>
        </mxCell>
        <mxCell id="resource-ii" value="II: 1 (fully pipelined, no stalls)" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="490" width="280" height="18" as="geometry"/>
        </mxCell>
        <mxCell id="resource-latency" value="Latency: 51 + pipeline_depth cycles" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="510" width="280" height="18" as="geometry"/>
        </mxCell>
        <mxCell id="resource-sym" value="Symmetry: h[k]==h[50-k], 25 pre-add pairs" style="text;html=1;align=left;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="35" y="530" width="280" height="18" as="geometry"/>
        </mxCell>

        <!-- Legend -->
        <mxCell id="legend-box" value="" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8f9fa;strokeColor=#495057;" vertex="1" parent="1">
          <mxGeometry x="380" y="400" width="280" height="160" as="geometry"/>
        </mxCell>
        <mxCell id="legend-title" value="&lt;b&gt;Legend&lt;/b&gt;" style="text;html=1;align=left;verticalAlign=top;fontSize=11;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="390" y="405" width="80" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-dsp-box" value="DSP" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dbeafe;strokeColor=#1971c2;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="390" y="430" width="50" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-dsp-label" value="Compute / DSP48E1" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="450" y="430" width="180" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-mem-box" value="MEM" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e0e7ff;strokeColor=#4f46e5;dashed=1;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="390" y="455" width="50" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-mem-label" value="Memory / Buffer (dashed)" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="450" y="455" width="180" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-acc-box" value="ACC" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#99e9f2;strokeColor=#0c8599;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="390" y="480" width="50" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-acc-label" value="Accumulator / Reduction" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="450" y="480" width="180" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-preg-box" value="" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#e9ecef;strokeColor=#868e96;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="400" y="505" width="8" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-preg-label" value="Pipeline register" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="450" y="505" width="180" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-io-box" value="I/O" style="ellipse;whiteSpace=wrap;html=1;fillColor=#d1fae5;strokeColor=#059669;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="390" y="530" width="50" height="20" as="geometry"/>
        </mxCell>
        <mxCell id="legend-io-label" value="Stream port (data_t)" style="text;html=1;align=left;fontSize=9;" vertex="1" parent="1">
          <mxGeometry x="450" y="530" width="180" height="20" as="geometry"/>
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

Companion `.arch.json`:

```json
{
  "title": "module0_prefilter - Micro-Architecture",
  "type": "module_microarchitecture",
  "phase": 6,
  "version": "5.0.3",
  "module_pattern": "Case_A",
  "notes": [
    "51-tap symmetric pre-add FIR, II=1, ARRAY_PARTITION complete on shift registers",
    "Critical path: SHIFT -> PRE-ADD(17b) -> MUL(34b) -> ACC(36b) -> TRUNC(16b)"
  ],
  "nodes": {
    "io-data-in": {
      "name": "data_in",
      "desc": "Input: complex_t (2x ap_fixed<16,4>)"
    },
    "io-data-out": {
      "name": "data_out",
      "desc": "Output: complex_t, num_filtered samples"
    },
    "mem-shift-reg": {
      "name": "SHIFT REG",
      "desc": "51 x data_t shift register, PARTITION complete (51 FFs per channel)"
    },
    "dsp-preadd": {
      "name": "PRE-ADD",
      "desc": "sr[k]+sr[50-k] for k=0..24, plus center tap. 26 unique 17-bit values"
    },
    "mem-coeffs": {
      "name": "COEFFS[26]",
      "desc": "26 unique symmetric coefficients, const ROM (coeff_t)"
    },
    "dsp-mul": {
      "name": "DSP48E1 MUL",
      "desc": "26 multiplies per channel (preadd*coeff), 1 DSP48E1 each. 52 total"
    },
    "acc-tree": {
      "name": "ACCUMULATE",
      "desc": "26-term tree reduction, accum_t (ap_fixed<36,12>), 5-level adder tree"
    },
    "trunc": {
      "name": "TRUNCATE",
      "desc": "accum_t(36b) -> data_t(16b), saturate + round"
    }
  },
  "edges": [
    {"from": "io-data-in", "to": "mem-shift-reg", "type": "data", "label": "complex_t stream"},
    {"from": "mem-shift-reg", "to": "dsp-preadd", "type": "data", "label": "16b symmetric pairs"},
    {"from": "dsp-preadd", "to": "dsp-mul", "type": "data", "label": "17b pre-added values"},
    {"from": "mem-coeffs", "to": "dsp-mul", "type": "data", "label": "coefficient values"},
    {"from": "dsp-mul", "to": "acc-tree", "type": "data", "label": "34b products"},
    {"from": "acc-tree", "to": "trunc", "type": "data", "label": "36b accumulated sum"},
    {"from": "trunc", "to": "io-data-out", "type": "data", "label": "16b truncated output"}
  ],
  "resources": {
    "dsp": 52,
    "bram": 0,
    "lut": 528,
    "ff": 1632
  },
  "hardware_params": {
    "module0_prefilter": {
      "fir_taps": 51,
      "fir_impl": "symmetric_preadd",
      "symmetric": true,
      "coefficient_files": ["filter_coeffs.txt"]
    }
  }
}
```

---

## Structural Notes

**Consistent patterns across all 4 examples:**

1. Every XML starts with `<mxfile>` wrapper, has `<mxCell id="0"/>` and `<mxCell id="1" parent="0"/>`.
2. All content cells use `parent="1"`.
3. All styles include `html=1;` for HTML rendering in labels.
4. Edge labels use `labelBackgroundColor=#FFFFFF` for readability over crossing lines.
5. Data arrows: solid blue (#1971c2), strokeWidth=2, endArrow=block.
6. Control arrows: dashed red (#e03131), strokeWidth=1, dashed=1.
7. Self-loop arrows use explicit exit/entry points (exitX/exitY, entryX/entryY) with waypoints.
8. Pipeline registers are thin gray rectangles (8px wide) between stages.
9. IDs follow the pattern: `{category}-{name}` (e.g., `io-data-in`, `m0-prefilter`, `dsp-mul`, `edge-m0-m1`).
10. Each `.arch.json` includes: title, type, phase, version, nodes, edges (matching the schema).
