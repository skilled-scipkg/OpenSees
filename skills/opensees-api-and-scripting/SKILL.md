---
name: opensees-api-and-scripting
description: This skill should be used when users ask about api and scripting in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this skill for Tcl vs Python workflow selection, command parity checks, interpreter usage, and script-level automation patterns.
- Route build/import/install blockers to `opensees-build-and-install`.
- Route element/material choice and physical parameterization to `opensees-inputs-and-modeling`.
- Route low-level command implementation/debugging to `opensees-src`.

### Triage questions
- Is the user running Tcl (`OpenSees script.tcl`) or Python (`opensees`/`openseespy`)?
- Is the request about command syntax, runtime behavior, or extending APIs?
- Which command family is failing (`model`, `uniaxialMaterial`, `nDMaterial`, `analysis`, `recorder`)?
- Is there a working minimal script that reproduces the issue?
- Is this single-process or MPI/parallel interpreter usage?
- Is extension work needed (new command/material binding) or only script authoring?

### Canonical workflow
1. Pick interface first: Tcl script, Python script, or C++ extension route.
2. Build minimal model (`wipe`, `model`, nodes, materials, elements, loads).
3. Add analysis stack (`system`, `constraints`, `numberer`, `algorithm`, `integrator`, `analysis`).
4. Add recorder(s), run `analyze`, and inspect output.
5. If Tcl/Python mismatch appears, compare command signatures against docs/examples.
6. If still ambiguous, inspect command parser and runtime dispatch files.

### Minimal working example
```python
from openseespy.opensees import *

wipe()
model("BasicBuilder", "-ndm", 2, "-ndf", 2)
node(1, 0.0, 0.0); node(2, 144.0, 0.0)
fix(1, 1, 1)
uniaxialMaterial("Elastic", 1, 3000.0)
element("truss", 1, 1, 2, 10.0, 1)
timeSeries("Linear", 1)
pattern("Plain", 1, 1)
load(2, 100.0, 0.0)
system("BandSPD")
numberer("RCM")
constraints("Plain")
algorithm("Linear")
integrator("LoadControl", 1.0)
analysis("Static")
analyze(1)
printModel("node", 2)
wipe()
```

```tcl
wipe
model basic -ndm 2 -ndf 2
node 1 0.0 0.0
node 2 144.0 0.0
fix 1 1 1
uniaxialMaterial Elastic 1 3000
element truss 1 1 2 10.0 1
pattern Plain 1 Linear { load 2 100 0 }
system BandSPD
constraints Plain
numberer RCM
algorithm Linear
integrator LoadControl 1.0
analysis Static
analyze 1
print node 2
```

### Pitfalls and fixes
- Python import confusion: follow fallback pattern in `tests/README.md` (`import opensees as ops` then `openseespy.opensees`).
- Missing/incorrect `model` arguments: `ndm` required; `ndf` defaults depend on `ndm` (from `SRC/doc/OpenSeesCommands.tex` and `model.cpp`).
- Stateful contamination across runs: always `wipe` before re-building model.
- Mixed static/transient assumptions: ensure `integrator` and `analysis` are compatible.
- Command-name drift/aliases: check parser maps in runtime command files before assuming removal.

### Convergence and validation checks
- Reproduce behavior with a <=20 line script before changing larger scripts.
- Confirm time/displacement progression is finite after `analyze`.
- For transient or variable-step runs, verify `getTime()` increments as expected (`example_variable_analysis.py`).
- Run `pytest -v tests` for cross-interface sanity on common workflows.

### Source-code jump table
- Runtime library initialization: `SRC/runtime/OpenSeesRT.cpp`, `SRC/runtime/executable/main.cpp`
- Python bridge and wrappers: `SRC/runtime/python/OpenSeesPyRT.cpp`
- Core command object model: `SRC/interpreter/OpenSeesCommands.cpp`
- Model command parsing: `SRC/runtime/commands/modeling/model.cpp`
- Material dispatch: `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`, `SRC/runtime/commands/modeling/material/nDMaterial.cpp`, `SRC/runtime/commands/modeling/material/material.hpp`
- Analysis dispatch: `SRC/runtime/commands/analysis/solver.cpp`, `SRC/runtime/commands/analysis/integrator.cpp`, `SRC/runtime/commands/analysis/algorithm.cpp`
- Legacy Tcl command registry: `SRC/tcl/commands.cpp`

Doc anchors: `SRC/doc/g3API.tex`, `SRC/doc/OpenSeesCommands.tex`, `EXAMPLES/ExamplePython/Example1.1.py`, `DEVELOPER/system/example1.tcl`, `tests/README.md`, `EXAMPLES/ExamplePython/example_variable_analysis.py`.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `SRC/doc/g3API.tex`
- `SRC/doc/OpenSeesCommands.tex`
- `EXAMPLES/ExamplePython/Example1.1.py`
- `DEVELOPER/system/example1.tcl`
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
- `SRC/runtime/OpenSeesRT.cpp`
- `SRC/runtime/python/OpenSeesPyRT.cpp`
- `SRC/runtime/executable/main.cpp`
- `SRC/interpreter/OpenSeesCommands.cpp`
- `SRC/runtime/commands/modeling/model.cpp`
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp`
- `SRC/runtime/commands/analysis/solver.cpp`
- `SRC/runtime/commands/analysis/integrator.cpp`
- `SRC/runtime/commands/analysis/algorithm.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" DEVELOPER SRC`).
