
# AI-Assisted Semantic Reverse Engineering
## From Binary to Human-Readable Rust

Version: 0.1
Status: Experimental design
Target: Linux
Example application: Windows Calculator (calc.exe)

---

## 1. Project Goal

Develop an iterative, AI-assisted reverse engineering
system that reconstructs a human-readable Rust application
from an existing compiled binary.

The system must reconstruct not only program behavior,
but also the probable intentions, abstractions, structures,
and functional requirements of the original developers.

The final results must include:

1. A functional, compilable Rust implementation.
2. Human-readable, idiomatic Rust source code.
3. A reconstructed functional specification.
4. A reconstructed architectural description.
5. Traceability between binary evidence and Rust code.
6. Tests demonstrating behavioral equivalence.
7. A persistent knowledge base containing discoveries,
   hypotheses, corrections, and unresolved questions.
8. A reproducible history of all refinement iterations.

The initial target is a Windows calculator application,
reconstructed as a native Linux application.

The approach should eventually support other binaries
and application domains.

---

## 2. Fundamental Principles

### 2.1 Semantic reconstruction

The system must not merely translate assembly instructions
into equivalent Rust instructions.

It must attempt to recover the higher-level programming
concepts that a human developer may have used.

Examples include:

- Meaningful function names.
- Meaningful parameter names.
- Meaningful variable names.
- Structs and enums.
- State machines.
- Event handlers.
- Domain-specific types.
- Modules and abstraction boundaries.
- Relationships between functions.
- Relationships between data structures.
- Error-handling semantics.
- Likely original programming intentions.

Human-readable Rust should be generated from iteration
zero whenever sufficient evidence exists.

A literal ASM-to-Rust translation is not a prerequisite.

### 2.2 Evidence versus hypothesis

The system must distinguish between:

OBSERVED:
    Directly supported by an original artifact.

DERIVED:
    Logically follows from established facts.

INFERRED:
    Plausible based on domain knowledge and patterns.

UNKNOWN:
    Not yet determined.

REFUTED:
    Previously proposed but contradicted by evidence.

A hypothesis must never automatically become a fact
merely because it appeared in an earlier iteration.

### 2.3 Behavioral equivalence

The Rust program should reproduce the externally
observable behavior of the original application.

Equivalent behavior does not necessarily require:

- Identical machine instructions.
- Identical memory layout.
- Identical internal algorithms.
- Identical implementation language.
- Identical operating system APIs.

Any deviation in observable behavior must be documented.

### 2.4 Recoverable human intent

The system should attempt to recognize familiar
programming patterns.

For example:

    value = value * 10 + digit

might represent decimal digit accumulation.

A probable semantic implementation could be:

    fn input_digit(&mut self, digit: u8)

However, that interpretation must remain a hypothesis
until sufficient supporting evidence has been collected.

Compiler optimizations may destroy original variable
names, abstractions, and function boundaries.

The system must not claim to have recovered exact
original source code when the evidence cannot prove it.

---

## 3. Input Artifacts

The system may receive incomplete input.

Possible inputs include:

- An original executable binary.
- Screenshots.
- Partial functional specifications.
- User interface observations.
- Disassembly.
- Decompiled pseudocode.
- Imported library information.
- Symbols and debug information.
- Runtime traces.
- Known input/output examples.
- General domain knowledge.

Not all inputs are mandatory.

The system must explicitly record which inputs were
available during each experiment.

### 3.1 Original binary

The binary is an authoritative source of implementation
evidence, but static analysis may not expose all behavior.

Record at least:

- Filename.
- SHA-256 checksum.
- Architecture.
- Executable format.
- Compiler clues.
- Imported libraries.
- Resources.
- External dependencies.
- Analysis tool versions.

### 3.2 Original specification

File:

    specs/spec_original.md

This file contains only the initial requirements
provided by the user.

It must remain immutable during the experiment.

Example:

    # Calculator

    - Contains a button labeled "1".

No additional functionality may be silently inserted
into this original specification.

---

## 4. Specification Inference Engine

### 4.1 Purpose

Reconstruct a more complete specification from
incomplete initial requirements using domain knowledge,
logical reasoning, observed behavior, and binary evidence.

### 4.2 Example

Initial requirement:

    The calculator has a button labeled "1".

The AI may infer that a typical calculator probably
supports:

- Numeric digit entry.
- Digits 0 through 9.
- Numeric display.
- Addition.
- Subtraction.
- Multiplication.
- Division.
- Equals.
- Clear/reset.

These are candidate requirements, not established facts.

The presence of a "1" button alone does not prove
the existence of any other button.

### 4.3 Requirement inference

For each candidate requirement, record:

- Stable requirement ID.
- Requirement description.
- Origin.
- Evidence references.
- Inference explanation.
- Confidence estimate.
- Validation status.
- Dependencies.
- Associated binary functions.
- Associated Rust components.
- Associated test cases.
- Iteration in which it was introduced.

### 4.4 Requirement dependency analysis

The AI must reason about implied behavior.

For example:

If an addition operation exists, the system may need:

- Two operands.
- Operator selection.
- Intermediate state.
- A calculation trigger.
- A way to display a result.

The system should investigate whether these expectations
match the original application.

It must also consider alternative implementations.

### 4.5 Specification output

File:

    specs/spec_inferred.md

This document contains hypotheses generated from
domain knowledge and available evidence.

Unlike spec_original.md, it evolves over time.

It must include unresolved assumptions and questions.

---

## 5. Semantic Inference Engine

### 5.1 Purpose

Convert low-level implementation evidence into
higher-level programming concepts.

Inputs may include:

- Assembly.
- Decompiled pseudocode.
- Call graphs.
- Control-flow graphs.
- Data-flow relationships.
- Memory accesses.
- Constants.
- Resource identifiers.
- API calls.
- Inferred requirements.
- Previous semantic hypotheses.

### 5.2 Pattern recognition

Recognize common software patterns, such as:

- Decimal digit accumulation.
- Arithmetic operations.
- State initialization.
- State reset.
- Input dispatch.
- Button event handling.
- Display formatting.
- Error handling.
- Memory management.
- Lookup tables.
- State transitions.

### 5.3 Function reconstruction

For each analyzed function, attempt to infer:

- Intended responsibility.
- Human-readable name.
- Parameter meanings.
- Return-value meaning.
- State accessed or modified.
- Related functions.
- Possible original abstractions.
- Evidence supporting each interpretation.

Example:

    FUN_00401A20

might become:

    Calculator::input_digit

if its behavior and callers support that interpretation.

### 5.4 Structure reconstruction

Infer potential high-level structures from:

- Memory access patterns.
- Shared offsets.
- Object lifetimes.
- Groups of related functions.
- Repeated state transitions.
- Call relationships.

Possible Rust abstractions:

    struct Calculator
    enum Operator
    enum InputEvent
    enum CalculatorError

These are candidates and must be validated.

### 5.5 Top-down and bottom-up reasoning

The engine uses two complementary directions.

BOTTOM-UP:

    Assembly
      -> Data flow
      -> Patterns
      -> Semantics
      -> Rust abstractions

TOP-DOWN:

    Domain knowledge
      -> Expected functionality
      -> Expected structures
      -> Search binary for evidence
      -> Confirm or reject hypothesis

The two directions should continuously inform each other.

---

## 6. Shared Knowledge Graph

The Specification Inference Engine and Semantic
Inference Engine share a persistent knowledge model.

The graph represents relationships among:

- Requirements.
- Observations.
- Hypotheses.
- Binary functions.
- Call sites.
- Variables.
- Memory regions.
- Rust symbols.
- Tests.
- Runtime observations.
- Supporting evidence.
- Contradictions.

Example relationship:

    REQ-014
        -> numeric digit input
        -> FUN_00401A20
        -> Calculator::input_digit
        -> TEST-023

All relationships must have explicit provenance.

### 6.1 Avoid circular confirmation

A hypothesis generated by the specification engine
must not be treated as independently verified merely
because the code engine repeats it.

Independent evidence must come from sources such as:

- Binary analysis.
- Runtime execution.
- UI observation.
- Original user-provided facts.
- Reproducible tests.

### 6.2 Knowledge persistence

File:

    knowledge/knowledge.json

The knowledge base survives between iterations
and between separate AI sessions.

It must not depend on an LLM's temporary context.

---

## 7. Iterative Refinement

### 7.1 Basic principle

Every iteration N receives the accepted outputs
and accumulated knowledge from earlier iterations.

Iteration N must not start from scratch.

The system must remember:

- Previously discovered facts.
- Previously accepted abstractions.
- Previously rejected hypotheses.
- Previous errors.
- Previous validation results.
- Unresolved questions.
- Previous architectural decisions.
- Prior specification changes.
- Previous test results.

### 7.2 Iteration input

Each iteration receives:

1. Original immutable artifacts.
2. Current specification.
3. Current semantic knowledge.
4. Current Rust source.
5. Previous iteration report.
6. Relevant evidence.
7. Outstanding hypotheses.
8. Failed tests.
9. Validation metrics.
10. Research priorities.

### 7.3 Iteration operations

An iteration may:

- Discover new functionality.
- Improve function names.
- Improve variable names.
- Infer additional types.
- Introduce structs.
- Introduce enums.
- Split functions.
- Merge related functions.
- Improve module boundaries.
- Correct previous assumptions.
- Refine specifications.
- Add or revise tests.
- Improve behavioral compatibility.
- Improve documentation.

Not every iteration must change the Rust code.

An iteration may improve knowledge or documentation
without modifying the implementation.

### 7.4 Iteration outputs

Each iteration produces:

    src/
    spec_new.md
    knowledge.json
    knowledge_delta.json
    reasoning.md
    metrics.json
    test_results.json

An iteration is accepted only after validation.

Rejected iterations remain available for analysis
but must not replace the last accepted state.

---

## 8. Continuous Specification Reconstruction

### 8.1 Three specification documents

spec_original.md:

    Immutable initial user requirements.

spec_inferred.md:

    AI-generated candidate requirements,
    including evidence, confidence, and uncertainty.

spec_new.md:

    Consolidated, human-readable description of
    the reconstructed application.

### 8.2 spec_new.md

This document is a primary project artifact.

It evolves together with the Rust implementation.

It should eventually document:

- Application purpose.
- User-visible functionality.
- Interface behavior.
- Input handling.
- Arithmetic rules.
- State transitions.
- Data structures.
- Internal architecture.
- Error behavior.
- Edge cases.
- Known limitations.
- API behavior.
- Relationships between major components.

### 8.3 Documentation quality

The specification must describe actual or validated
behavior, not merely paraphrase current Rust source code.

Unverified assumptions must be clearly marked.

Every iteration must check whether documentation
and implementation remain consistent.

### 8.4 Spec/code synchronization

When a new feature is confirmed:

1. Update the knowledge graph.
2. Update the inferred specification.
3. Update the consolidated specification.
4. Update the Rust implementation if necessary.
5. Add or adjust validation tests.
6. Record the change in the iteration report.

---

## 9. Semantic Memory and Learning

### 9.1 Historical knowledge

Store the complete accepted knowledge state
and the changes introduced by every iteration.

Example:

    iterations/007/knowledge_delta.json

A delta can contain:

- Newly discovered facts.
- Newly inferred functions.
- Confirmed hypotheses.
- Rejected hypotheses.
- Revised symbol names.
- Revised structure definitions.
- New evidence.
- New uncertainties.
- Resolved uncertainties.

### 9.2 Correcting previous mistakes

If a hypothesis is disproven:

1. Preserve the original hypothesis historically.
2. Mark it as refuted.
3. Record the contradicting evidence.
4. Update dependent conclusions.
5. Reassess affected Rust code.
6. Reassess affected requirements.
7. Add regression tests where appropriate.

The system should avoid repeating previously
disproven interpretations without new evidence.

### 9.3 Efficient context construction

Do not blindly submit the entire history to the LLM
during every iteration.

Construct a relevant context containing:

- Current accepted knowledge.
- Relevant source-code fragments.
- Relevant binary evidence.
- Important historical corrections.
- Open questions for the current task.

Keep full history on disk for reproducibility.

---

## 10. Rust Generation Strategy

### 10.1 First iteration

Generate human-readable Rust immediately when possible.

Do not require an intermediate, instruction-by-instruction
Rust translation.

Prefer semantic abstractions supported by evidence.

### 10.2 Example

Low-level pseudocode:

    value = value * 10 + digit

Candidate Rust:

    impl Calculator {
        fn input_digit(&mut self, digit: u8) {
            self.current_input =
                self.current_input * 10
                + u64::from(digit);
        }
    }

This is only valid if data types, state handling,
and overflow behavior match the original program.

### 10.3 Rust quality objectives

Prefer:

- Meaningful identifiers.
- Appropriate data types.
- Explicit state.
- Small coherent functions.
- Clearly defined responsibilities.
- Modules based on domain concepts.
- Idiomatic ownership and borrowing.
- Explicit error handling.
- Minimal unnecessary unsafe code.

### 10.4 Correctness over elegance

Readable code is desirable, but behavioral fidelity
has priority.

Do not introduce an abstraction that changes behavior
merely because it appears more idiomatic.

---

## 11. Validation and Differential Testing

### 11.1 Compilation

The generated application must compile on Linux.

Minimum commands:

    cargo build
    cargo test

### 11.2 Behavioral tests

Test functionality such as:

- Digit entry.
- Multiple consecutive digits.
- Leading zeros.
- Decimal input.
- Arithmetic operations.
- Repeated operations.
- Clear and reset.
- Error states.
- Overflow behavior.
- Display formatting.
- Keyboard input.
- State transitions.

Only test features applicable to the identified
original application.

### 11.3 Differential testing

Where possible, execute identical input sequences
against the original binary and reconstructed program.

Compare:

- Displayed results.
- Internal state where observable.
- Error messages.
- State transitions.
- Input handling.
- Operation order.

Record every mismatch.

### 11.4 Reference execution

The original Windows binary may require:

- Wine.
- A Windows virtual machine.
- Original Windows dependencies.
- UI automation.
- Additional runtime instrumentation.

If reliable reference execution is impossible,
record that limitation.

### 11.5 Regression testing

Every accepted behavior must become a regression test
where practical.

Later iterations may not silently reintroduce
previously resolved mismatches.

---

## 12. Confidence and Acceptance Thresholds

The system should use several independent metrics.

Suggested initial weights:

    Behavioral correctness       45%
    Specification coverage       20%
    Binary evidence alignment    20%
    Rust code quality            10%
    Uncertainty reduction         5%

These weights are experimental and adjustable.

### 12.1 Thresholds

The target is greater than 90% confidence in
behavioral correctness for the defined application scope.

A weighted quality score is not, by itself, a
statistically valid probability of equivalence.

Confidence estimates must be distinguished from
measured test coverage and observed pass rates.

### 12.2 Mandatory gates

An iteration is accepted only if:

- Compilation succeeds.
- Required tests pass.
- No unacceptable regressions occur.
- Evidence references are valid.
- Code and specification are consistent.
- New claims have appropriate evidence status.
- Critical unresolved contradictions are addressed.

### 12.3 Advancement logic

Possible outcomes:

ACCEPT:
    The iteration improves the accepted state
    and passes mandatory gates.

REJECT:
    The iteration introduces unacceptable errors.

INVESTIGATE:
    More evidence is needed before proceeding.

STOP:
    The defined completion criteria are met,
    or further refinement is no longer productive.

---

## 13. Active Uncertainty Reduction

The system must identify which unanswered questions
should be investigated next.

Examples:

- Does the calculator contain a decimal button?
- Does clear reset the entire state?
- Is arithmetic evaluated immediately?
- How are errors represented?
- Is a function an input handler or display formatter?

An illustrative investigation priority:

    Priority =
        Uncertainty * Impact * Testability

Additional factors can include:

- Expected information gain.
- Analysis cost.
- Dependency importance.
- Risk of an incorrect inference.

The AI should prefer investigations that are likely
to resolve important uncertainty.

Avoid repeatedly rewriting code without acquiring
new knowledge.

---

## 14. Convergence Detection

The pipeline must detect diminishing returns.

Track changes across accepted iterations.

Signals include:

- Stable function names.
- Stable struct definitions.
- Stable module boundaries.
- Stable specifications.
- Decreasing unresolved uncertainty.
- Increasing behavioral test coverage.
- No new meaningful binary discoveries.
- No new test failures.
- Minimal semantic changes.

Convergence alone does not prove correctness.

A stable but incorrect hypothesis can also converge.

Therefore convergence must be evaluated together
with independent validation evidence.

---

## 15. Reproducibility

Every experiment must be reproducible.

Record:

- Binary checksum.
- Ghidra version.
- Analysis settings.
- Rust compiler version.
- AI model identifier.
- Prompt template version.
- Generation parameters.
- Knowledge snapshot.
- Source-code revision.
- Test results.
- Iteration decisions.

Store accepted iterations in Git.

Prefer deterministic analysis and validation tools
where available.

LLM output may be nondeterministic, so preserve
the actual generated artifacts.

---

## 16. Proposed Project Structure

    calc-re/
    |
    +-- original/
    |   +-- calc.exe
    |   +-- sha256.txt
    |
    +-- screenshots/
    |   +-- calculator.png
    |
    +-- specs/
    |   +-- spec_original.md
    |   +-- spec_inferred.md
    |   +-- spec_new.md
    |
    +-- evidence/
    |   +-- disassembly/
    |   +-- decompiled/
    |   +-- call_graph.json
    |   +-- symbols.json
    |   +-- runtime/
    |
    +-- knowledge/
    |   +-- knowledge.json
    |   +-- hypothesis_graph.json
    |   +-- rejected.json
    |
    +-- iterations/
    |   +-- 000/
    |   +-- 001/
    |   +-- 002/
    |   +-- ...
    |
    +-- src/
    |   +-- main.rs
    |   +-- calculator.rs
    |
    +-- tests/
    |   +-- behavior.rs
    |   +-- differential.rs
    |
    +-- tools/
    |   +-- ghidra/
    |   +-- orchestrator/
    |
    +-- Cargo.toml
    +-- PLAN.md
    +-- README.md

---

## 17. Suggested Technology Stack

Binary analysis:

    Ghidra

Orchestration:

    Python initially, potentially Rust later.

Target language:

    Rust

Build system:

    Cargo

Version control:

    Git

AI:

    Configurable LLM backend.

Reference execution:

    Wine or Windows VM, when feasible.

Testing:

    Rust unit tests.
    Integration tests.
    Differential behavior tests.

Structured knowledge:

    JSON initially.
    A graph database may be considered later.

Do not introduce additional infrastructure
without a demonstrated need.

---

## 18. Experiment Phases

### Phase 0: Baseline

- Identify the binary.
- Calculate its checksum.
- Inspect available resources.
- Record screenshots.
- Write the incomplete initial specification.
- Establish reference behavior.
- Create initial testcases.

### Phase 1: Specification inference

- Generate candidate requirements.
- Classify observed and inferred functionality.
- Detect missing requirements.
- Build requirement dependencies.
- Create spec_inferred.md.

### Phase 2: Binary analysis

- Disassemble the original binary.
- Identify functions and call relationships.
- Extract constants and resources.
- Analyze data flow.
- Identify possible application state.

### Phase 3: Semantic reconstruction

- Apply domain knowledge to low-level patterns.
- Infer functions, names, parameters, and structs.
- Correlate binary evidence with requirements.
- Construct the initial semantic knowledge graph.

### Phase 4: Initial Rust implementation

- Generate human-readable Rust v0.
- Implement confirmed or strongly supported behavior.
- Clearly identify speculative functionality.
- Compile and execute initial tests.

### Phase 5: Iterative refinement

Repeatedly:

1. Load previous accepted knowledge.
2. Select the highest-value investigation.
3. Gather additional evidence.
4. Refine semantic hypotheses.
5. Update Rust code.
6. Update specifications.
7. Update knowledge and history.
8. Compile and test.
9. Evaluate metrics.
10. Accept, reject, or investigate further.

### Phase 6: Convergence and evaluation

- Measure behavioral equivalence.
- Assess specification completeness.
- Assess semantic reconstruction quality.
- Measure uncertainty reduction.
- Examine convergence.
- Document unresolved deviations.

---

## 19. High-Level Orchestration Algorithm

    state = initialize_from_original_artifacts()

    while not completion_criteria_met(state):

        question = select_next_investigation(state)

        evidence = collect_evidence(
            question,
            state
        )

        candidate = ai_refine(
            original_artifacts,
            evidence,
            state.knowledge,
            state.specification,
            state.rust_source,
            state.history
        )

        candidate = update_specifications(candidate)

        candidate = update_knowledge_graph(candidate)

        results = validate(candidate)

        decision = evaluate(
            candidate,
            results,
            state
        )

        save_iteration_artifacts(
            candidate,
            results,
            decision
        )

        if decision == ACCEPT:
            state = candidate

        elif decision == INVESTIGATE:
            schedule_additional_investigation()

        elif decision == REJECT:
            retain_previous_accepted_state()

        elif decision == STOP:
            break

    generate_final_documentation(state)

    generate_final_report(state)

The algorithm must also include:

- Iteration limits.
- Failure handling.
- Stagnation detection.
- Budget limits.
- Explicit stop reasons.

---

## 20. Research Questions

The experiment should attempt to answer:

1. Can an AI reconstruct likely human programming
   abstractions directly from machine code?

2. How much does domain knowledge improve
   first-iteration Rust quality?

3. Can missing requirements be reconstructed
   from incomplete specifications?

4. Can specification inference and binary analysis
   reinforce one another without circular reasoning?

5. Does persistent semantic memory improve results
   compared with independent iterations?

6. Can rejected hypotheses be used to prevent
   repeated mistakes?

7. How many iterations are needed before meaningful
   semantic convergence occurs?

8. How strongly does semantic convergence correlate
   with behavioral correctness?

9. Can code and documentation evolve consistently
   throughout reconstruction?

10. Can the approach generalize beyond calculators?

---

## 21. Success Criteria

The experiment succeeds within a declared scope if:

- The reconstructed application builds on Linux.
- Its defined functionality matches the original
  across the required independent test suite.
- Behavioral coverage exceeds the selected target.
- Remaining deviations are explicitly documented.
- Rust code is understandable and maintainable.
- Major semantic abstractions have evidence links.
- spec_new.md accurately describes validated behavior.
- The knowledge history is reproducible.
- The refinement process demonstrably improves
  results over the initial reconstruction.

A test pass rate greater than 90% is a useful target,
but does not establish 90% probability of complete
program equivalence.

The final report must identify both the validated
scope and the untested behavior.

---

## 22. Final Deliverables

The project should ultimately produce:

1. A native Linux executable.
2. Its complete Rust source code.
3. spec_new.md.
4. Human-readable architecture documentation.
5. The reconstructed requirement model.
6. The semantic knowledge graph.
7. Evidence-to-code traceability.
8. Behavioral and regression tests.
9. Iteration history and metrics.
10. A final reconstruction confidence report.

---

## 23. Guiding Research Principle

The purpose is not simply to translate machine code.

The purpose is to reconstruct the most plausible
human-understandable software design consistent
with the available evidence.

The system should continuously improve:

    What we know.
    What we think we know.
    What we can prove.
    What we can execute.
    What we can explain.

Code, specifications, and semantic knowledge must
evolve together.

The original artifacts remain the reference.

Every new claim must be traceable.

Every iteration must be reproducible.

And every refinement should move the implementation
closer to demonstrable behavioral equivalence.

