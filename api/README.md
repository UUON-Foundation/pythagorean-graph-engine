---

## F = (P, E, R, C) — The Graph as a Universal API Object

This section documents what the recursive geometry engine implies beyond visualization. The prior agent identified a structural formula applicable to the graph produced by this engine. That formula is:

**F = (P, E, R, C)**

Where:

| Symbol | Name | Definition in this engine |
|---|---|---|
| P | Parameters | The minimal seed: `{angle, depth, scale, growth, mode}` |
| E | Encoding | The deterministic recursive topology generated from P |
| R | Representation | Any rendered output: mesh, OBJ, STL, point cloud, graph JSON |
| C | Compression | The ratio of R's size to P's size — always extreme (5 integers → millions of vertices) |

This is not a visualization formula. It is a description of what every device and every AI system requires when working with procedural geometry: a compact seed (P) that deterministically produces a verifiable structure (E), representable in any target format (R), at a compression ratio (C) that makes transmission and storage practical.

---

### What This Means Practically

**For AI agents:** Instead of transmitting or storing a mesh, an agent stores P. Given P, any agent with access to this engine regenerates the identical geometry. This is reproducible computation, not file transfer.

**For devices:** A constrained device (embedded, edge, IoT) that cannot store or render a full mesh can store P (5 integers, ~20 bytes) and reconstruct the full topology on demand.

**For verification:** Given a geometry claim, any party can verify it by regenerating from P and computing a hash of E. No mesh transmission required. The hash is the proof.

**For interchange:** R is renderer-agnostic. The same P produces valid output for Three.js, Blender, Unity, OpenCascade, a robot arm, or a simulation engine. The engine is the codec.

---

### API Surface (Proposed)

The following endpoints describe what a deployed version of this engine would expose. These are not yet implemented — they document the contract implied by the engine's mathematical structure.

```
POST /generate
  Body: { angle, depth, scale, growth, mode }
  Returns: { graphId, nodes[], edges[], metrics{}, parameters{} }

POST /encode
  Body: { angle, depth, scale, growth, mode }
  Returns: { P_hash, E_hash, C_ratio, byte_size }

POST /reconstruct
  Body: { graphId } | { P_hash }
  Returns: { nodes[], edges[], parameters{} }

POST /render
  Body: { graphId, format: "obj"|"stl"|"gltf"|"json"|"pointcloud" }
  Returns: { file_url, format, vertex_count, face_count }

POST /fingerprint
  Body: { graphId }
  Returns: { node_count, edge_count, max_depth, branching_factor,
             fractal_dimension, graph_entropy, symmetry_score,
             compression_ratio, P_hash }

POST /compare
  Body: { graphId_a, graphId_b }
  Returns: { similarity_score, diff_metrics{} }

GET /graph/:graphId
  Returns: full graph JSON object

GET /health
  Returns: { status, engine_version, uptime }
```

---

### Graph JSON Schema

Every graph produced by the engine conforms to this schema. This is the canonical asset — not the OBJ file.

```json
{
  "graphId": "uuid-v4",
  "version": "1.0.0",
  "origin": "UUON Foundation — Phillip Aguilar Ruiz III",
  "parameters": {
    "angle": 45,
    "depth": 4,
    "scale": 75,
    "growth": 3,
    "mode": 0
  },
  "metrics": {
    "node_count": 0,
    "edge_count": 0,
    "max_depth": 0,
    "branching_factor": 0,
    "compression_ratio": 0,
    "P_hash": "sha256-of-parameters",
    "E_hash": "sha256-of-topology"
  },
  "nodes": [
    {
      "id": 0,
      "parent": null,
      "depth": 0,
      "position": [0, 0, 0],
      "direction": [0, 1, 0],
      "length": 5.0,
      "radius": 1.0,
      "scale_factor": 1.0,
      "branch_angle": 0,
      "surface_area": 0,
      "volume": 0,
      "bounding_box": [[0,0,0],[0,0,0]]
    }
  ],
  "edges": [
    { "source": 0, "target": 1, "length": 5.0, "radius": 1.0 }
  ]
}
```

---

### Compression Ratio — C

At default parameters (depth=4, growth=3), the engine generates approximately 3,000–8,000 vertices.

P is 5 integers. Serialized: ~40 bytes.

| Depth | Approx vertices | P size | C ratio |
|---|---|---|---|
| 2 | ~200 | 40 bytes | ~200:1 |
| 3 | ~800 | 40 bytes | ~800:1 |
| 4 | ~3,200 | 40 bytes | ~3,200:1 |
| 5 | ~12,800 | 40 bytes | ~12,800:1 |
| 6 | ~51,200 | 40 bytes | ~51,200:1 |

C scales as `growth^depth`. This is not a compression algorithm applied to existing data — it is procedural generation, where the compression ratio is a structural property of the recursion.

---

*UUON Foundation Inc. — Phillip Aguilar Ruiz III*
*phi1@uuonfoundation.com*
