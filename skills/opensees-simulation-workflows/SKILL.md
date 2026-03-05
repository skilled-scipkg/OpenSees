---
name: opensees-simulation-workflows
description: This skill should be used when users ask about simulation workflows in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Use this skill for end-to-end run control: analysis stack assembly, stepping strategy, recorder setup, and runtime checkpoints.
- Route model assembly questions (elements/materials/tags) to `opensees-inputs-and-modeling`.
- Route Tcl/Python command parity to `opensees-api-and-scripting`.
- Route deep algorithm theory to `opensees-theory-and-methods`.

### Triage questions
- Static or transient run?
- Required outputs (node disp, element force, reactions, modal values)?
- Single run or incremental loop with stop/retry logic?
- Fixed step (`analyze n dt`) or adaptive/manual loop?
- Is the failure parser-level, convergence-level, or physically unrealistic output?

### Canonical workflow
1. Start from a known baseline script (`EXAMPLES/ExampleScripts/Example1.1.tcl` or `EXAMPLES/verification/sdofTransient.tcl`).
2. Build full analysis stack explicitly: `constraints`, `numberer`, `system`, `test`, `algorithm`, `integrator`, `analysis`.
3. Add recorder commands before `analyze`.
4. Run a short horizon first (1-10 steps), inspect outputs, then scale duration.
5. If failure occurs, adjust one control at a time (step size, algorithm, test limits).
6. Re-run unchanged baseline after each major mutation.

### Minimal working examples
```tcl
# Static run
constraints Plain
numberer RCM
system BandSPD
test NormDispIncr 1.0e-12 10 0
algorithm Linear
integrator LoadControl 1.0
analysis Static
set ok [analyze 1]
puts "static ok = $ok"
```

```tcl
# Transient run with checkpoints
set dt 0.01
constraints Plain
numberer Plain
system ProfileSPD
test NormDispIncr 1.0e-10 20 0
algorithm Newton
integrator Newmark 0.5 [expr 1.0/6.0]
analysis Transient
set ok [analyze 5 $dt]
puts "ok=$ok time=[getTime]"
```

### Pitfalls and fixes
- Missing `analysis` object before `analyze`: ensure full stack is declared.
- Recorder files stay empty: recorder command must be defined before first `analyze`.
- Non-convergence in copied script: reduce increment (`dt` or load step), then retest.
- Mixing static/transient control blocks: rebuild stack intentionally after `wipeAnalysis`.
- Relative input paths fail: run from the script directory or normalize file paths.

### Convergence and validation checks
- Check return code from `analyze` and log current `getTime()`.
- Confirm recorder time column is monotonic when using `-time`.
- Verify reactions approximately balance loads for static checks.
- For transient runs, halve `dt` once and compare response trends.
- Re-run baseline script to confirm no environment/regression drift.

### Source-code jump table
- Analysis command dispatch: `SRC/runtime/commands/analysis/analysis.cpp`, `SRC/runtime/commands/analysis/transient.cpp`
- Algorithm/integrator/test commands: `SRC/runtime/commands/analysis/algorithm.cpp`, `SRC/runtime/commands/analysis/integrator.cpp`, `SRC/runtime/commands/analysis/ctest.cpp`
- Solver selection: `SRC/runtime/commands/analysis/solver.cpp`
- Time-series integration helper: `SRC/runtime/commands/domain/loading/TclSeriesIntegratorCommand.cpp`
- Runtime builders: `SRC/runtime/runtime/BasicAnalysisBuilder.cpp`, `SRC/runtime/runtime/BasicModelBuilder.cpp`
- Recorder command path: `SRC/recorder/TclRecorderCommands.cpp`, `SRC/recorder/NodeRecorder.cpp`, `SRC/recorder/ElementRecorder.cpp`

Doc anchors: `tests/README.md`, `EXAMPLES/ExampleScripts/Example1.1.tcl`, `EXAMPLES/verification/sdofTransient.tcl`, `EXAMPLES/verification/NewmarkIntegrator.tcl`, `EXAMPLES/ExamplesForTesting/Test.example1.ops`.

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `tests/README.md`
- `EXAMPLES/ExampleScripts/Example1.1.tcl`
- `EXAMPLES/verification/sdofTransient.tcl`
- `EXAMPLES/verification/NewmarkIntegrator.tcl`
- `EXAMPLES/ExamplesForTesting/Test.example1.ops`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `EXAMPLES`
- `Workshops`

## Test references
- `tests`
- `EXAMPLES/ExamplesForTesting`
- `SRC/unittest`

## Optional deeper inspection
- `DEVELOPER`
- `SRC`

## Source entry points for unresolved issues
- `SRC/runtime/commands/analysis/analysis.cpp`
- `SRC/runtime/commands/analysis/transient.cpp`
- `SRC/runtime/commands/analysis/algorithm.cpp`
- `SRC/runtime/commands/analysis/integrator.cpp`
- `SRC/runtime/commands/analysis/ctest.cpp`
- `SRC/runtime/commands/analysis/solver.cpp`
- `SRC/runtime/commands/domain/loading/TclSeriesIntegratorCommand.cpp`
- `SRC/runtime/runtime/BasicAnalysisBuilder.cpp`
- `SRC/recorder/TclRecorderCommands.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" SRC/runtime/commands/analysis SRC/recorder`).
