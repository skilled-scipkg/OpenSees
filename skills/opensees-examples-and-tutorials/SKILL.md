---
name: opensees-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Use this skill when users need runnable starting points, example curation (beginner to advanced), or mapping examples to command docs.
- Route command semantics and API parity questions to `opensees-api-and-scripting`.
- Route model design/material selection questions to `opensees-inputs-and-modeling`.
- Route solver/convergence tuning to `opensees-theory-and-methods`.

### Triage questions
- Tcl or Python example needed?
- Static, transient, or nonlinear cyclic workflow?
- Is a minimal example required or closest full benchmark?
- Are local ground-motion/text input files present and referenced with correct paths?
- Is expected output format known (node/element recorder files, JSON model dump)?
- Is this for learning, regression testing, or reproducing a bug?

### Canonical workflow
1. Start from a known baseline (`Example1.1` Tcl/Python) and run it unchanged.
2. Confirm output files are produced (recorder and/or model print output).
3. Move to nearest advanced family (`EXAMPLES/ExampleScripts/Example3.1.tcl`, `EXAMPLES/ExampleScripts/ArcLength01.tcl`, `EXAMPLES/verification/sdofTransient.tcl`).
4. Change one dimension at a time (material, integrator, load pattern, dt).
5. Compare with `EXAMPLES/ExamplesForTesting/Test.Example1.1.ops` and `EXAMPLES/ExamplesForTesting/Test.example3.ops`.
6. If mismatch persists, inspect recorder and command parser source entry points.

### Minimal working example
```bash
# Tcl baseline
OpenSees EXAMPLES/ExampleScripts/Example1.1.tcl

# Python baseline
python EXAMPLES/ExamplePython/Example1.1.py

# Regression sanity suite (OpenSeesPy-side)
pytest -v tests
```

```tcl
# Core transient analysis pattern from EXAMPLES/verification/sdofTransient.tcl
constraints Plain
numberer Plain
algorithm Linear
integrator Newmark 0.5 [expr 1.0/6.0]
system ProfileSPD
analysis Transient
analyze 1 $dt
```

### Pitfalls and fixes
- Example fails due to missing record files (`tabasFP.txt`, `tabasFN.txt`, `NR228.txt`, `elCentro.AT2`): run from the example directory or fix relative paths.
- Recorder files are empty: ensure `analysis` is created and `analyze` is actually called.
- Transient drift/instability in copied examples: reduce `dt` and verify integrator/algorithm pair.
- Script mutation breaks silently: keep a known-good baseline script and diff small edits.
- Cross-language mismatch: validate equivalent Tcl/Python example pair before deeper changes.

### Convergence and validation checks
- Confirm baseline scripts complete without parser/runtime errors.
- Check recorder time column is monotonic when `-time` is requested.
- For verification scripts, compare against embedded reference checks/tolerances (`EXAMPLES/verification/sdofTransient.tcl`, `tests/test_particle-dynamics.py`, `tests/test_force-beam-column.py`).
- Re-run unchanged baseline after each mutation to isolate regressions.

### Source-code jump table
- Runtime entry and script loading: `SRC/runtime/executable/main.cpp`, `SRC/runtime/OpenSeesRT.cpp`
- Interpreter/input bridge: `SRC/runtime/parsing/InterpreterAPI.cpp`, `SRC/runtime/parsing/InputAPI.h`
- Time-series command plumbing: `SRC/interpreter/OpenSeesTimeSeriesCommands.cpp`
- Recorder command parsing: `SRC/recorder/TclRecorderCommands.cpp`
- Recorder implementations: `SRC/recorder/NodeRecorder.cpp`, `SRC/recorder/ElementRecorder.cpp`

Doc anchors: `EXAMPLES/ExampleScripts/Example1.1.tcl`, `EXAMPLES/ExamplePython/Example1.1.py`, `EXAMPLES/ExampleScripts/ArcLength01.tcl`, `EXAMPLES/verification/sdofTransient.tcl`, `EXAMPLES/ExamplesForTesting/Test.Example1.1.ops`, `tests/README.md`.

## Scope
- Handle questions about worked examples, tutorials, and cookbook usage.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `EXAMPLES/ExampleScripts/Example1.1.tcl`
- `EXAMPLES/ExamplePython/Example1.1.py`
- `EXAMPLES/verification/sdofTransient.tcl`
- `EXAMPLES/ExampleScripts/ArcLength01.tcl`
- `EXAMPLES/ExamplesForTesting/Test.Example1.1.ops`
- `tests/README.md`

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
- `SRC/runtime/executable/main.cpp`
- `SRC/runtime/OpenSeesRT.cpp`
- `SRC/runtime/parsing/InterpreterAPI.cpp`
- `SRC/runtime/parsing/InputAPI.h`
- `SRC/interpreter/OpenSeesTimeSeriesCommands.cpp`
- `SRC/recorder/TclRecorderCommands.cpp`
- `SRC/recorder/NodeRecorder.cpp`
- `SRC/recorder/ElementRecorder.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" EXAMPLES SRC`).
