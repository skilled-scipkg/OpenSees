---
name: opensees-src
description: This skill should be used when users ask about src in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Src

## High-Signal Playbook
### Route conditions
- Use this skill when docs/examples are insufficient and behavior must be verified in concrete source files.
- Route user-facing modeling workflows to `opensees-inputs-and-modeling` or `opensees-examples-and-tutorials` first.
- Route interface-specific scripting questions to `opensees-api-and-scripting`.

### Triage questions
- Which exact command/symbol is in question?
- Is the path Tcl runtime, Python runtime, or legacy Tcl interpreter code?
- Is the issue in model setup, analysis configuration, or recorder/output behavior?
- Is this command registration, argument parsing, or algorithm implementation?
- Is behavior different from docs or only from user expectations?

### Canonical workflow
1. Identify command from docs (`OpenSeesCommands.tex` / topic doc).
2. Locate parser/dispatcher file in `SRC/runtime/commands/modeling`, `SRC/runtime/commands/analysis`, or `SRC/runtime/commands/domain`.
3. Trace object creation into domain/analysis/material class.
4. Cross-check with example/test script that invokes the same command.
5. Inspect legacy code path (`SRC/tcl`, `SRC/interpreter`) only if runtime path is ambiguous.

### Minimal working example
```bash
# Map command language to concrete implementation files
rg -n "TclCommand_specifyModel|model BasicBuilder" SRC/runtime/commands/modeling SRC/doc
rg -n "TclCommand_addNDMaterial|nDMaterial" SRC/runtime/commands/modeling SRC/material
rg -n "specifyIntegrator|TclCommand_specifyAlgorithm|analysis\(" SRC/runtime/commands/analysis
rg -n "recorder Node|recorder Element|closeOnWrite" SRC/recorder
```

### Pitfalls and fixes
- Looking only at one command stack: OpenSees has runtime command path and legacy Tcl/interpreter path.
- Assuming command names are unique: aliases are registered in parser maps.
- Debugging behavior without minimal reproducer: always reduce to smallest script first.
- Ignoring compile guards (`_PARALLEL_INTERPRETERS`, `_MUMPS`, etc.) while tracing code.
- Treating recorder timing as exact equality: recorder code uses tolerance logic for floating-point time steps.

### Convergence and validation checks
- Confirm the symbol is found in parser file and in downstream implementation class.
- Run a tiny script that exercises only the target command and verify output side effects.
- Verify object lifecycle (add/remove/wipe) through domain command paths when leaks or stale state are suspected.
- Cross-check with at least one `tests/` or `EXAMPLES/` script for intended behavior.

### Source-code jump table
- Command language manuals and roots: `SRC/doc/OpenSeesCommands.tex`, `SRC/doc/g3Primer.tex`, `SRC/doc/quick.tex`
- Runtime executable and package load: `SRC/runtime/executable/main.cpp`, `SRC/runtime/OpenSeesRT.cpp`
- Core command object model: `SRC/interpreter/OpenSeesCommands.cpp`, `SRC/interpreter/OpenSeesCommands.h`
- Modeling command dispatch: `SRC/runtime/commands/modeling/model.cpp`, `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`, `SRC/runtime/commands/modeling/material/nDMaterial.cpp`
- Analysis dispatch: `SRC/runtime/commands/analysis/solver.cpp`, `SRC/runtime/commands/analysis/integrator.cpp`, `SRC/runtime/commands/analysis/algorithm.cpp`
- Legacy Tcl command table: `SRC/tcl/commands.cpp`, `SRC/tcl/commands.h`
- Domain and utilities: `SRC/runtime/commands/domain/domain.cpp`, `SRC/runtime/commands/strings.cpp`, `SRC/runtime/commands/pragma.cpp`

Doc anchors: `SRC/doc/OpenSeesCommands.tex`, `SRC/doc/Elastic3DCommands.tex`, `SRC/doc/upUCommands.tex`, `SRC/doc/quick.tex`, `SRC/doc/g3Primer.tex`, `SRC/doc/UCD_CG_OpenSees_Commands.tex`.

## Scope
- Handle questions about documentation grouped under the 'src' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `SRC/doc/OpenSeesCommands.tex`
- `SRC/doc/Elastic3DCommands.tex`
- `SRC/doc/upUCommands.tex`
- `SRC/doc/quick.tex`
- `SRC/doc/g3Primer.tex`
- `SRC/doc/UCD_CG_OpenSees_Commands.tex`
- `SRC/doc/ReturnMap.tex`
- `SRC/doc/BrickCommands.tex`

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
- `SRC/interpreter/OpenSeesCommands.h`
- `SRC/interpreter/OpenSeesCommands.cpp`
- `SRC/runtime/commands/modeling/model.cpp`
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp`
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp`
- `SRC/runtime/commands/analysis/solver.cpp`
- `SRC/runtime/commands/analysis/integrator.cpp`
- `SRC/runtime/commands/analysis/algorithm.cpp`
- `SRC/tcl/commands.cpp`
- `SRC/tcl/commands.h`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" SRC`).
