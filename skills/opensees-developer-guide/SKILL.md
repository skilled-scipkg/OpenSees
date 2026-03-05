---
name: opensees-developer-guide
description: This skill should be used when users ask about developer guide in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Developer Guide

## High-Signal Playbook
### Route conditions
- Use this skill for contributor workflows: adding/extending elements, materials, or system components, and validating extension builds.
- Route end-user model authoring to `opensees-inputs-and-modeling`.
- Route pure build-toolchain failures to `opensees-build-and-install`.
- Route command-level runtime tracing to `opensees-src`.

### Triage questions
- Is the extension in C, C++, or Fortran under `DEVELOPER`?
- Is the target an element, material, recorder, or system module?
- Does the change require new Tcl command registration or only class behavior changes?
- Which minimal example script reproduces expected/actual behavior?
- Is this plugin-only testing or integration into full `SRC` runtime?

### Canonical workflow
1. Start from `DEVELOPER` instructions for OS/compiler setup.
2. Copy nearest working example (for example `DEVELOPER/material/cpp/ElasticPPcpp.cpp` or `DEVELOPER/element/cpp/Truss2D.cpp`).
3. Build extension artifacts (`make` in `DEVELOPER` or subfolder).
4. Run paired example script (`example1.tcl`) with OpenSees.
5. Validate expected response trends before broad refactors.
6. For unresolved behavior, trace command registration and class interfaces in `SRC`.

### Minimal working example
```bash
cd DEVELOPER
make
export LD_LIBRARY_PATH=./material/cpp:./element/cpp:${LD_LIBRARY_PATH}
OpenSees material/cpp/example1.tcl
OpenSees element/cpp/example1.tcl
```

```tcl
# Typical extension script pattern
wipe
model BasicBuilder -ndm 2 -ndf 2
# load custom material/element command from built library context
source example1.tcl
```

### Pitfalls and fixes
- Shared library not found at runtime: include output directory in `LD_LIBRARY_PATH`.
- Header/API mismatch: verify include path against `DEVELOPER/core/elementAPI.h` and related core headers.
- Extension compiles but command missing: inspect command registration path in interpreter/runtime sources.
- No reproducible test: keep and run a tiny `example1.tcl` for each extension.
- Platform drift (Win32/x64): align build target with interpreter binary architecture.

### Convergence and validation checks
- `make` or CMake target completes for the modified extension.
- OpenSees executes corresponding `example1.tcl` without parser/runtime errors.
- Output response is finite and directionally correct (at least one numerical sanity check).
- Regression check: rerun an unchanged baseline extension example.

### Source-code jump table
- Developer build roots: `DEVELOPER/Makefile`, `DEVELOPER/CMakeLists.txt`
- Core extension interfaces: `DEVELOPER/core/UniaxialMaterial.h`, `DEVELOPER/core/NDMaterial.h`, `DEVELOPER/core/elementAPI.h`
- Example extension implementations: `DEVELOPER/material/cpp/ElasticPPcpp.cpp`, `DEVELOPER/element/cpp/Truss2D.cpp`, `DEVELOPER/system/SkylineSPD.cpp`
- Fortran API bridges: `DEVELOPER/material/fortran/materialAPI.f`, `DEVELOPER/element/fortran/elementAPI.f`
- Runtime class broker/registration context: `SRC/runtime/runtime/TclPackageClassBroker.cpp`, `SRC/interpreter/OpenSeesCommands.cpp`

Doc anchors: `DEVELOPER/UnixInstructions.txt`, `DEVELOPER/WindowsInstructions.txt`, `DEVELOPER/material/cpp/example1.tcl`, `DEVELOPER/element/cpp/example1.tcl`.

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `DEVELOPER/UnixInstructions.txt`
- `DEVELOPER/WindowsInstructions.txt`
- `DEVELOPER/Makefile`
- `DEVELOPER/coreList`
- `DEVELOPER/material/cpp/example1.tcl`
- `DEVELOPER/element/cpp/example1.tcl`

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
- `DEVELOPER/Makefile`
- `DEVELOPER/CMakeLists.txt`
- `DEVELOPER/core/UniaxialMaterial.h`
- `DEVELOPER/core/NDMaterial.h`
- `DEVELOPER/core/elementAPI.h`
- `DEVELOPER/material/cpp/ElasticPPcpp.cpp`
- `DEVELOPER/element/cpp/Truss2D.cpp`
- `DEVELOPER/system/SkylineSPD.cpp`
- `DEVELOPER/material/fortran/materialAPI.f`
- `DEVELOPER/element/fortran/elementAPI.f`
- `SRC/runtime/runtime/TclPackageClassBroker.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" DEVELOPER SRC/runtime SRC/interpreter`).
