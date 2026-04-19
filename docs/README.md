# Documentation

The primary documentation for AXIOM-Ω is an interactive web-based system design guide.

## Files

- **AXIOM_omega_system_documentation.html** — Main system design document with interactive tabs

  Open this file in any web browser to explore:
  - System overview and core concepts
  - Mathematical framework (Φ-space, integrals, algebra, embeddings)
  - Six-layer architecture and pipeline design
  - Proof-of-concept roadmap (Phases 0–3)
  - Data requirements and encoding schemes
  - Technical risks and constraints
  - Assessment scores and validation criteria

## How to View

1. **Local (Recommended):** Open `AXIOM_omega_system_documentation.html` in your browser
   ```bash
   # On macOS
   open AXIOM_omega_system_documentation.html
   
   # On Linux
   xdg-open AXIOM_omega_system_documentation.html
   
   # On Windows
   start AXIOM_omega_system_documentation.html
   ```

2. **Via File Explorer:** Double-click the HTML file to open in your default browser

3. **Via Terminal:** Use a local web server (optional)
   ```bash
   python3 -m http.server 8000
   # Then navigate to http://localhost:8000/docs/
   ```

## Document Structure

The HTML guide is organized into 8 interactive panels:

| Panel | Topic |
|-------|-------|
| **Overview** | What AXIOM-Ω is, five core ideas, system philosophy |
| **Math Framework** | Φ-space, integral bounding, algebra, embeddings, swarm consensus |
| **Architecture** | Six-layer design (L1–L6), documentation plane, vector databases |
| **Pipeline** | Step-by-step execution flow from anomaly to deployment decision |
| **PoC Plan** | Proof-of-concept phases, success criteria, timeline |
| **Data** | Input/output specifications, encoding schemes, vector storage |
| **Risks** | Technical risks, constraints, open mathematical questions |
| **Assessment** | Scores, complexity analysis, knowledge domains required |

## Key Concepts

- **Φ-Space:** High-dimensional function matrix representing system dependencies
- **Integral Bounds:** Triple integrals computing valid solution regions [L, U]
- **Ring Homomorphism:** Structure-preserving code transformations τ
- **ANN Embedding:** Neural network decoding from vector space to code
- **Swarm Decider:** Weighted consensus voting for deployment gates

## Next Steps

- **For Theory:** See [../papers/](../papers/) for research foundations
- **For Implementation:** See [../code/](../code/) for scaffolding and source
- **For Visuals:** See [../diagrams/](../diagrams/) for architecture diagrams
- **For Data:** See [../data/](../data/) for sample datasets and benchmarks

---

**Last Updated:** 2026-04-18  
**Version:** v0.1 (PoC)