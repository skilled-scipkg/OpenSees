---
name: opensees-theory-and-methods
description: This skill should be used when users ask about theory and methods in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Theory and Methods

## High-Signal Playbook
### Route conditions
- Use this skill for method selection (algorithm/integrator/system), convergence behavior, and theory-to-command mapping.
- Route concrete model assembly questions to `opensees-inputs-and-modeling`.
- Route interface syntax issues to `opensees-api-and-scripting`.
- Route code-level parser/implementation tracing to `opensees-src`.

### Triage questions
- Static or transient analysis?
- Linear, mildly nonlinear, or strongly nonlinear response expected?
- Which solver family is intended (`BandSPD`, `ProfileSPD`, sparse general, parallel)?
- Which algorithm/integrator pair is currently used?
- What convergence test/tolerance/iteration cap is set?
- Is there a known analytic or benchmark response for validation?

### Canonical workflow
1. Map physics assumptions from theory docs (truss/beam formulations).
2. Choose analysis components consistent with that model class.
3. Start with conservative tolerances and short run horizon.
4. Verify convergence behavior per step before long simulations.
5. If non-convergent, change one knob at a time: algorithm, test, dt/increment, then solver.
6. Validate against verification scripts or closed-form checks.

### Minimal working example
```tcl
# Minimal analysis-method stack (pattern from verification scripts)
constraints Plain
numberer Plain
system ProfileSPD
test NormDispIncr 1.0e-12 10 0
algorithm Newton
integrator Newmark 0.5 [expr 1.0/6.0]
analysis Transient
analyze 1 $dt
```

```tcl
# Static counterpart
constraints Plain
numberer RCM
system BandSPD
algorithm Linear
integrator LoadControl 1.0
analysis Static
analyze 1
```

### Pitfalls and fixes
- Using `algorithm Linear` on materially nonlinear problems: switch to Newton-family algorithms.
- Solver mismatch (SPD solver on non-SPD system): use compatible solver/system pair.
- Overly aggressive time/load increments: reduce `dt` or load step size first.
- Missing/loose convergence test: define `test` tolerance and max iterations explicitly.
- Comparing against no baseline: use `EXAMPLES/verification/sdofTransient.tcl`, `tests/test_particle-dynamics.py`, or `tests/test_force-beam-column.py`.

### Convergence and validation checks
- Monitor iteration count and residual trend each step.
- For transient runs, perform a time-step sensitivity check (e.g., halve `dt` and compare response envelope).
- For static nonlinear runs, compare response with smaller load increments.
- Validate against known references (analytic checks in `sdofTransient.tcl`, test assertions in `tests/`).

### Source-code jump table
- Algorithm command parsing and option handling: `SRC/runtime/commands/analysis/algorithm.cpp`
- Integrator selection/dispatch: `SRC/runtime/commands/analysis/integrator.cpp`
- System of equations selection: `SRC/runtime/commands/analysis/solver.cpp`
- Algorithm implementations: `SRC/analysis/algorithm/equiSolnAlgo/NewtonRaphson.cpp`, `SRC/analysis/algorithm/equiSolnAlgo/ModifiedNewton.cpp`, `SRC/analysis/algorithm/equiSolnAlgo/KrylovNewton.cpp`, `SRC/analysis/algorithm/equiSolnAlgo/NewtonLineSearch.cpp`
- Base algorithm abstraction: `SRC/analysis/algorithm/SolutionAlgorithm.h`, `SRC/analysis/algorithm/SolutionAlgorithm.cpp`
- Theory references tied to formulations: `SRC/doc/TrussTheory.tex`, `SRC/doc/BeamTheory.tex`

Doc anchors: `SRC/doc/TrussTheory.tex`, `SRC/doc/BeamTheory.tex`, `SRC/doc/OpenSeesCommands.tex`, `EXAMPLES/verification/sdofTransient.tcl`, `tests/test_particle-dynamics.py`, `tests/test_force-beam-column.py`.

## Scope
- Handle questions about theoretical background and algorithmic methods.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `SRC/doc/TrussTheory.tex`
- `SRC/doc/BeamTheory.tex`
- `SRC/doc/OpenSeesCommands.tex`
- `EXAMPLES/verification/sdofTransient.tcl`
- `tests/test_particle-dynamics.py`
- `tests/test_force-beam-column.py`

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
- `SRC/runtime/commands/analysis/algorithm.cpp`
- `SRC/runtime/commands/analysis/integrator.cpp`
- `SRC/runtime/commands/analysis/solver.cpp`
- `SRC/analysis/algorithm/SolutionAlgorithm.h`
- `SRC/analysis/algorithm/SolutionAlgorithm.cpp`
- `SRC/analysis/algorithm/equiSolnAlgo/NewtonRaphson.cpp`
- `SRC/analysis/algorithm/equiSolnAlgo/ModifiedNewton.cpp`
- `SRC/analysis/algorithm/equiSolnAlgo/KrylovNewton.cpp`
- `SRC/analysis/algorithm/equiSolnAlgo/NewtonLineSearch.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" SRC/analysis SRC/runtime/commands/analysis`).
