# calc_re

An experiment in AI-assisted reverse engineering: reconstructing a Windows calculator binary as readable, working Rust on Linux.

Instead of translating assembly instruction by instruction, the goal is to infer the original program's behavior, likely abstractions, and developer intent.

The proposed iterative workflow combines:

- **Specification inference:** expand incomplete requirements into testable hypotheses.
- **Semantic inference:** recover meaningful functions, types, and state from binary evidence.
- **Documentation inference:** continuously improve human-readable documentation and diagrams.
- **Validation:** compare the Rust implementation against the original, preserving evidence, tests, and lessons between iterations.

The project follows a **Luna-first, budget-aware** approach: optimize for verified progress per euro, not the fewest iterations.

**Status:** Research and design phase. No working reconstruction yet.

See [PLAN.md](PLAN.md) for the full design.
