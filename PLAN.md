# AI-Assisted Semantic Reverse Engineering

Version: 0.3  
Status: Experimental masterplan  
Target: Linux, Rust  
Initial case: Windows `calc.exe`

## 1. Goal

Reconstruct a compiled application as idiomatic, human-readable Rust, while simultaneously reconstructing its functional specifications, likely developer intent, architecture, tests, and friendly illustrated documentation. Everything evolves over multiple evidence-based iterations. Recreate observable behavior, not byte-identical machine code or supposedly exact original variable names.

Deliverables: native Linux executable; Rust sources; `spec_new.md`; inferred requirements; semantic/evidence graph; tests; user manual; architecture guide; diagrams and illustrations; iteration history; cost and confidence reports.

## 2. Core rules

- Separate **observed**, **logically derived**, **inferred**, **unknown**, and **refuted** claims.
- Immutable original artifacts remain the ultimate reference.
- Domain knowledge can suggest patterns immediately, including for Rust v0; plausible names and structs remain hypotheses until tested.
- An LLM output, generated documentation, or a second LLM agreeing with the first does **not** independently confirm an assertion.
- Preserve provenance: requirements -> evidence -> binary functions -> Rust symbols -> tests -> docs.
- Prefer behavioral fidelity over elegant but incompatible refactoring.
- Every iteration inherits accepted knowledge and the history of disproved ideas.

## 3. Inputs and baseline

Inputs may be incomplete: binary, screenshot, partial spec, observed input/output, UI resources, runtime traces, Ghidra disassembly/decompilation, API imports, and domain knowledge. Record binary SHA-256, architecture, dependencies, analysis-tool versions, runtime environment, and test scope. Preserve `original/calc.exe`, screenshots, and immutable `specs/spec_original.md`.

Example original spec: `The calculator has a button labeled 1.` Nothing else may be silently promoted to original evidence.

## 4. Specification Inference Engine

Expand incomplete specs using calculator conventions, dependency reasoning, screenshots, and binary observations. From button `1`, propose digit entry, display, digits `0`-`9`, arithmetic, equals and clear. These are **candidate** requirements, not deductions proving buttons exist. Record stable ID, status, confidence estimate, rationale, evidence references, dependencies, relevant Rust symbol, test cases, and history in `specs/spec_inferred.md` and `knowledge/requirements.json`.

Use top-down expectations to direct investigation; let newly found binary behavior expand the specs in return. Avoid circular confirmation.

## 5. Semantic Inference Engine

Use assembly, decompiled C, call graphs, data-flow, constants, shared offsets and event dispatch to reconstruct likely high-level concepts: meaningful names, parameters, structs, enums, state machines, modules and functions. Apply human software-engineering patterns from the **first** Rust iteration, not only after literal assembly translation.

Example low-level pattern `value = value * 10 + digit` suggests `Calculator::input_digit(digit)`, but the true number representation, overflow semantics and display handling must be checked. Preserve address/symbol traceability and alternative hypotheses. Compiler optimizations can erase original source structure; do not claim exact recovery without evidence.

## 6. Documentation Inference Engine

Generate continuously, not only at the end:

- `docs/user_guide.md`: examples, screenshots, button behavior, errors, usage.
- `docs/architecture.md`: modules, state, structs, call relationships, trade-offs.
- `docs/behavior.md`: input/output and edge cases.
- `docs/reconstruction.md`: evidence, assumptions, limitations.
- `docs/faq.md`, `docs/unknowns.md`.
- `docs/diagrams/*.mmd` or `.dot`: flow, sequence, state, architecture; render to SVG/PNG.
- `docs/images/`: distinguish original screenshots, reconstructions and purely illustrative images.

Documentation itself exposes missing explanations and contradictions. Emit `documentation_insights.json` containing open questions, rationale, references and next investigations. Feed these into iteration N+1 as **questions**, not proof. Verify that diagrams and claims match tests, specs and code. Generate an HTML site or PDF later from versioned sources if desired.

## 7. Shared knowledge and memory

Store a persistent graph linking requirements, hypotheses, functions, code, evidence, tests and documentation. Each node has source/provenance, uncertainty, status, dependency links, introduction iteration and last-check iteration. Retain `knowledge.json`, `knowledge_delta.json`, `rejected.json`, and evidence snapshots. If evidence refutes a hypothesis, invalidate dependent code, specs, tests and docs; preserve the correction history. Construct a *relevant* model context instead of repeatedly uploading the whole history.

## 8. Iteration protocol

Iteration N receives: immutable artifacts, current accepted Rust, specifications, documentation/diagrams, knowledge graph, recent deltas, important rejected hypotheses, failed tests, and highest-priority open questions.

1. Select question by uncertainty, impact, testability, expected information gain and cost.
2. Collect evidence (static analysis, reference execution or observation).
3. Infer or revise requirements.
4. Infer or revise semantics and Rust source.
5. Infer or revise documentation and diagrams; capture new documentation questions.
6. Generate/update tests and graph edges.
7. Build, execute tests and compare against the original.
8. Evaluate acceptance gates and metrics.
9. Commit accepted state, otherwise keep last accepted state and record why.
10. Repeat until completion, stagnation, budget limit or explicit failure.

Iteration output: `src/`, `spec_new.md`, `spec_inferred.md`, `docs/`, `knowledge.json`, `knowledge_delta.json`, `documentation_insights.json`, `reasoning.md` (decision summaries), `metrics.json`, `test_results.json`. A knowledge-only or docs-only gain can justify an accepted iteration when it is verified and does not introduce regressions.

## 9. Validation

Use `cargo build`, `cargo test`, static analysis and differential test sequences against the original (Wine, Windows VM, or other reliable reference environment). Validate digit entry, multi-digit input, leading zeros, decimal precision, operations, equals, repeated operators, clears, overflow, errors and formatting where applicable. Regression tests must protect previously established behavior.

Document tests as observed examples, not proof of all possible behavior. Check docs against actual Rust symbols, behavior, and the graph. Mark unresolved claims explicitly.

Initial illustrative score weighting: behavioral correctness 40%, specification coverage 20%, binary-evidence alignment 20%, code quality 10%, documentation consistency 10%. These are engineering heuristics, **not** calibrated probabilities. Track uncertainty separately. Mandatory gates: build succeeds, required tests pass, no unacceptable regression, valid provenance, synchronized code/specs/docs, critical contradictions disclosed.

Goal: >90% measured correctness/coverage **within a declared test scope**, accompanied by explicit untested scope and residual risk. Neither 90% test pass rate nor a weighted score proves 90% probability of universal equivalence.

## 10. Budget-aware Luna-first model policy

This is a hobby project with a small budget. Optimize **verified useful progress per euro**, not fewest iterations or highest single-call accuracy. A cheaper model may take many more rounds and still win economically.

**Default:** a low-cost capable model, initially `Luna` with `High` reasoning *if available in the chosen Codex environment*. Run specification inference, semantic inference, routine coding, test writing and documentation on it first. Do not assume specific model aliases, pricing or access will remain stable.

**Escalation:** use a more costly model (e.g. `Sol`, if available) *only* when a well-defined gate is met: repeated nonprogress, a persistent contradiction, a difficult algorithmic blocker, or evidence that expensive reasoning has better expected value. High-end models are optional, not required. A cheaper retry should use new feedback, not repeat an identical prompt blindly.

**Configurable policy defaults (experimental):**

- `default_model = "luna"`; `reasoning_effort = "high"`.
- `stagnation_window = 5` accepted/attempted refinement opportunities without useful verified progress; adjust after measurements.
- `max_retry_per_question = 3` without new evidence, before changing investigation strategy.
- Set hard per-run and monthly USD ceilings, plus token and iteration ceilings; stop cleanly before violating them.
- Estimate *all* cost: input tokens, output tokens, cached tokens where applicable, analysis-tool runtime and expensive escalations. For flat-rate access/credits, track the actual applicable limits instead of pretending there is a token invoice.
- Preserve a compact context and reusable artifacts to reduce token costs.
- Run cheap deterministic compiler/tests before requesting another expensive model pass.
- Never auto-escalate without a remaining budget allowance and explicit configured permission.

Record per attempt: model identifier, reasoning effort, prompt version, input/output/cached tokens (when reported), estimated and actual billed cost if available, wall-clock time, accepted/rejected result, tests changed, new verified claims, resolved questions, regressions and reason for escalation. Version all pricing assumptions with their date/source, rather than hard-code price claims in the plan.

**Primary economic metric:** verified progress divided by spend, reported with its constituent measures. Candidate progress signals include newly verified requirements, resolved high-impact uncertainties, regression-free differential tests and verified documentation consistency. Do not reward verbosity, fabricated requirements, or self-reported confidence. Use a quality floor so low cost cannot mask failure.

For a small measured sample, compare `Luna High` and an alternative model on *the same bounded task set* and independent tests. Compare total cost through acceptance (including failed iterations), not just first-response quality. Re-evaluate the default based on recorded evidence.

## 11. Convergence and stopping

Track stability of function names, structs, requirements, diagrams, documentation and tests. Convergence is useful but not proof: incorrect assumptions can also stabilize. Stop on declared functional/evidence targets, a budget ceiling, iteration ceiling, repeated stagnation or missing reference evidence. Emit explicit reason and outstanding gaps. Never silently claim the task complete when a safety or budget gate ended it.

## 12. Reproducibility and technology

Use Ghidra for static analysis, Cargo/Rust for the target, Git for snapshots, and a small orchestrator (Python initially or Rust later). Use JSON for knowledge at first; only introduce a graph database if required. Diagram sources should be text-based and versioned. Record executable hash, Ghidra/Rust versions, model IDs, prompt versions, generation configuration, evidence snapshots, tests, scores, decisions, and spend. AI generations need not be deterministic, so save outputs.

## 13. Suggested project tree

```text
calc-re/
├── original/{calc.exe,sha256.txt}
├── screenshots/calculator.png
├── specs/{spec_original.md,spec_inferred.md,spec_new.md}
├── evidence/{disassembly,decompiled,runtime,call_graph.json}
├── knowledge/{knowledge.json,requirements.json,semantic_graph.json,rejected.json}
├── iterations/{000,001,002,...}/
├── src/{main.rs,calculator.rs}
├── tests/{behavior.rs,differential.rs}
├── docs/{user_guide.md,architecture.md,behavior.md,reconstruction.md,faq.md,unknowns.md}
├── docs/diagrams/
├── docs/images/
├── tools/{ghidra,orchestrator,documentation}/
├── config/models.toml
├── Cargo.toml
├── PLAN.md
└── README.md
```

## 14. Orchestrator pseudocode

```text
state = load_last_accepted_state()
while not stop_condition(state):
    question = prioritize_open_question(state, cost_estimate=true)
    evidence = collect_independent_evidence(question)
    candidate = cheap_model_refine(state, evidence, question)
    candidate = reconcile_specs_semantics_rust_docs(candidate)
    candidate.questions += extract_documentation_insights(candidate.docs)
    tests = run_compiler_and_differential_tests(candidate)
    decision = evaluate_gates_and_verified_delta(candidate, tests, state)
    record_attempt_and_cost(candidate, decision)
    if decision == ACCEPT:
        state = persist_as_accepted(candidate)
    elif decision == INVESTIGATE:
        obtain_new_evidence_or_change_question()
    elif decision == REJECT:
        retain_last_accepted_state()
    if stagnated(state) and escalation_is_allowed_and_affordable():
        try_more_capable_model_on_bounded_blocker()
export_final_rust_specs_docs_diagrams_and_cost_report(state)
```

## 15. Implementation milestones

1. Establish a repository and immutable input manifest.
2. Implement structured knowledge, claim statuses, evidence IDs and dependency invalidation.
3. Implement small orchestrator with deterministic test gates, retry and budget policy.
4. Prototype specification inference from `has a 1 button` and measure false assumptions.
5. Add Ghidra extraction, function/call graphs and semantic reconstruction.
6. Produce Rust v0 from meaningful inferred concepts; compile and test.
7. Implement documentation inference, Mermaid diagrams and docs-to-research feedback.
8. Implement reference execution and differential tests.
9. Run repeatable multi-iteration experiments, including cheap-model vs expensive-model comparisons.
10. Publish final implementation, human-friendly docs and evidence/cost report.

## 16. Success criteria

The reconstructed Rust app builds on Linux; matches the original within a clearly measured scope; has a readable architecture; has a traceable mapping from evidence to key decisions; ships coherent `spec_new.md`, documentation and diagrams; preserves corrections and uncertainty; runs reproducible iterations; and demonstrates useful progress under the selected hobby budget.

**Research question:** how far can evidence-grounded domain inference, cumulative memory, continual documentation and budget-aware model selection reconstruct a plausible developer-level design from a binary?
