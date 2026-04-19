# Data

Datasets, benchmark results, execution traces, and sample logs for AXIOM-Ω.

## Purpose

This folder contains:

- **Sample Datasets** — Example logs, execution traces, and system metrics
- **Benchmark Results** — Performance metrics and validation results
- **Test Data** — Injected anomalies and expected fixes for PoC validation
- **Encoding Examples** — How variables are mapped to ℝⁿ
- **Integral Bounds** — Baseline [L,U] computations for reference functions

## Data Structure

### Input Data

```
data/
├── logs/                      Sample structured logs (JSON)
├── execution_traces/          Function call traces with args/returns
├── system_metrics/            CPU, memory, latency time series
└── codebase_ast/              Parsed AST examples
```

### Processing Outputs

```
data/
├── state_vectors/             Encoded s(t) ∈ ℝⁿ vectors
├── phi_matrices/              Jacobian Φ(s,t) snapshots
├── integral_bounds/           Computed [L,U] per function
├── fix_proposals/             Generated code patches
└── verification_results/      Agent scores and decisions
```

### Benchmarks

```
data/
├── benchmarks/
│   ├── latency_profile.json   Timing per layer (L1–L6)
│   ├── accuracy_rates.json    PoC success metrics (7/10 target)
│   └── swarm_consensus.json   Agent voting patterns
```

## Data Specs

### Log Format

Structured JSON logs with minimum fields:

```json
{
  "timestamp": "2026-04-18T12:34:56.789Z",
  "level": "ERROR",
  "message": "Database connection timeout",
  "request_id": "req_abc123",
  "function": "fetchUserData",
  "duration_ms": 5000,
  "error_code": 504
}
```

### State Vector Encoding

Every log event is encoded as a real number vector:

```
vᵢ = hash(token) | timestamp_unix | error_code | duration_ms | ...

s(t) = [v₁(t), v₂(t), ..., vₙ(t)]ᵀ ∈ ℝⁿ
```

All values are L2-normalized before entering Φ-space.

### Φ-Matrix Format

Jacobian matrix per time window:

```
Φ(s,t) = [
  ∂f₁/∂v₁  ∂f₁/∂v₂  ...  ∂f₁/∂vₙ  ] L1: observ...
  ∂f₂/∂v₁  ∂f₂/∂v₂  ...  ∂f₂/∂vₙ  ]
  ...
  ∂fₘ/∂v₁  ∂fₘ/∂v₂  ...  ∂fₘ/∂vₙ  ]
]
```

Stored as JSON with function names and numerical arrays.

### Integral Bounds

Per-function bounds from Monte Carlo estimation:

```json
{
  "function": "processPayment",
  "lower_bound": -2.45,
  "upper_bound": 3.82,
  "samples": 10000,
  "timestamp": "2026-04-18T12:34:56.789Z",
  "current_output": 4.12,
  "anomaly_detected": true
}
```

## PoC Testing Data

For the proof-of-concept, 10 real anomalies are injected:

```
data/
├── poc_anomalies/
│   ├── anomaly_001/
│   │   ├── injected_error.log         Original error
│   │   ├── expected_fix.patch         Ground truth patch
│   │   ├── generated_fixes/           Proposed patches
│   │   └── agent_verdicts.json        Verification results
│   ├── anomaly_002/
│   │   ...
│   └── anomaly_010/
```

**Success Criteria:**
- ≥ 7/10 patches pass all agent checks ✓
- 0 false positives (broken patches gated through) ✓

## Vector Database Schema

For storing vectors (Qdrant / Pinecone):

```yaml
Collection: "axiom-omega-vectors"

Fields:
  - id: string                    # Unique identifier
  - timestamp: timestamp          # When vector was created
  - vector: float[]               # ℝⁿ vector (embeddings)
  - vector_type: enum             # "state", "phi", "solution"
  - function_name: string         # Associated function
  - bounds_lower: float           # Integral bound [L,U]
  - bounds_upper: float
  - metadata: object              # Custom fields
```

## Sample Queries

### Fetch State Vectors

```sql
SELECT * FROM vectors 
WHERE vector_type = 'state' 
  AND timestamp BETWEEN '2026-04-18T00:00' AND '2026-04-18T12:00'
ORDER BY timestamp DESC
```

### Find Anomalous Functions

```sql
SELECT function_name, COUNT(*) as violations
FROM vectors 
WHERE vector_type = 'state' 
  AND (data_value < bounds_lower OR data_value > bounds_upper)
GROUP BY function_name
ORDER BY violations DESC
```

## File Formats

- **JSON** — Logs, vectors, metadata
- **CSV** — Time series, metrics
- **Parquet** — Large datasets (efficient columnar format)
- **HDF5** — Scientific data (matrices, tensors)
- **SQLite** — Local database for small datasets

## Contributing

When adding data:

1. Use descriptive folder names (e.g., `system_metrics_2026_04_18/`)
2. Include a `README.md` in each subfolder describing contents
3. Document the schema/format
4. Anonymize sensitive data (remove real IPs, credentials, PII)
5. Add to version control or link to external storage

## Related Resources

### From Documentation

See [../docs/AXIOM_omega_system_documentation.html](../docs/AXIOM_omega_system_documentation.html#data) for:
- Data requirements (inputs/outputs)
- Encoding specifications
- Vector database requirements

### Tools

- **Vector DBs:** Qdrant, Pinecone, Weaviate
- **Time Series:** InfluxDB, Prometheus
- **Data Processing:** Pandas, NumPy, DuckDB

---

**Status:** Sample data phase  
**Version:** v0.1 (PoC)