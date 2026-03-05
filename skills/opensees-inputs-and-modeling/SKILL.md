---
name: opensees-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for model construction patterns: `model`, `node`, `fix`, material commands, element commands, and load-pattern setup.
- Route install/build blockers to `opensees-build-and-install`.
- Route Tcl vs Python interface differences to `opensees-api-and-scripting`.
- Route nonlinear solution tuning and convergence strategy to `opensees-theory-and-methods`.

### Triage questions
- What are `ndm`/`ndf` and unit system assumptions?
- Which element family is required (truss, beam-column, solid, zeroLength, etc.)?
- Is constitutive model uniaxial, nD, or section-based?
- Static or transient loading path?
- Are required time series and load patterns defined?
- What is the smallest model that reproduces the issue?

### Canonical workflow
1. Define global model shape first (`model BasicBuilder -ndm ... -ndf ...`).
2. Add nodes and boundary conditions.
3. Define material prototypes (`uniaxialMaterial` / `nDMaterial` / `section`).
4. Create elements with consistent node ordering and material tags.
5. Add time series and load pattern.
6. Build analysis stack and run a short solve.
7. Validate force balance/displacements before scaling model size.

### Minimal working example
```tcl
wipe
model BasicBuilder -ndm 2 -ndf 2
node 1 0.0 0.0
node 2 144.0 0.0
fix 1 1 1
uniaxialMaterial Elastic 1 3000
element truss 1 1 2 10.0 1
timeSeries Linear 1
pattern Plain 1 1 { load 2 100 0 }
system BandSPD
constraints Plain
numberer RCM
algorithm Linear
integrator LoadControl 1.0
analysis Static
analyze 1
```

```python
from openseespy.opensees import *
wipe(); model("BasicBuilder", "-ndm", 3, "-ndf", 3)
# Elastic 3D material command family from Elastic3DCommands.tex
nDMaterial("ElasticIsotropic3D", 10, 30000.0, 0.2, 2.5)
```

### Pitfalls and fixes
- Wrong default DOFs: if `-ndf` omitted, defaults depend on `ndm` (1->1, 2->3, 3->6).
- Material/element mismatch: ensure uniaxial materials feed uniaxial-consumer elements and nD materials feed continuum/plate wrappers.
- Silent tag conflicts: keep unique tags per object type and verify with `printModel`.
- Load does not apply: missing or mis-tagged `timeSeries`/`pattern`.
- Response unrealistic: check unit consistency and boundary conditions before changing solver settings.

### Convergence and validation checks
- Run smallest possible model and verify non-NaN displacement/reaction outputs.
- For static models, check reactions sum to applied loads.
- For transient models, validate time advancement and stable response trend before long runs.
- Use known examples (`Example1.1`, `Test.Example1.1.ops`) to validate command sequence.

### Source-code jump table
- `model` command parsing and defaults: `SRC/runtime/commands/modeling/model.cpp`
- Uniaxial material dispatch: `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`, `SRC/material/uniaxial/TclModelBuilderUniaxialMaterialCommand.cpp`
- nD material dispatch/maps: `SRC/runtime/commands/modeling/material/nDMaterial.cpp`, `SRC/runtime/commands/modeling/material/material.hpp`, `SRC/material/nD/TclModelBuilderNDMaterialCommand.cpp`
- Material probing/utility commands: `SRC/runtime/commands/modeling/invoking/invoke_material.cpp`
- Domain object lifecycle commands: `SRC/runtime/commands/domain/domain.cpp`

Doc anchors: `SRC/doc/OpenSeesCommands.tex`, `SRC/doc/Elastic3DCommands.tex`, `SRC/doc/Template3DEPCommands.tex`, `SRC/doc/NewMaterial.tex`, `EXAMPLES/ExampleScripts/Example1.1.tcl`, `EXAMPLES/ExamplePython/Example1.1.py`.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `SRC/doc/OpenSeesCommands.tex`
- `SRC/doc/Elastic3DCommands.tex`
- `SRC/doc/Template3DEPCommands.tex`
- `SRC/doc/NewMaterial.tex`
- `EXAMPLES/ExampleScripts/Example1.1.tcl`
- `EXAMPLES/ExamplePython/Example1.1.py`

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
- `SRC/runtime/commands/modeling/model.cpp`
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp`
- `SRC/runtime/commands/modeling/material/material.hpp`
- `SRC/runtime/commands/modeling/invoking/invoke_material.cpp`
- `SRC/material/uniaxial/TclModelBuilderUniaxialMaterialCommand.cpp`
- `SRC/material/nD/TclModelBuilderNDMaterialCommand.cpp`
- `SRC/runtime/commands/domain/domain.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" SRC/runtime/commands/modeling SRC/material`).
