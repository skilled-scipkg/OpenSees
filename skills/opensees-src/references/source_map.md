# opensees source map: Src

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for source-level behavior tracing.

## Fast source navigation
- `rg -n "Tcl_CreateCommand|OpenSeesCommands|analyze\(|uniaxialMaterial|nDMaterial|recorder" SRC`
- `rg -n "OPS_GetDomain|LoadOpenSeesRT|Tcl_PkgProvide" SRC/interpreter SRC/runtime`

## Function-level entry points
- `SRC/runtime/executable/main.cpp` | executable start and runtime library load path.
- `SRC/runtime/OpenSeesRT.cpp` | runtime package registration.
- `SRC/interpreter/OpenSeesCommands.h` | command object interfaces.
- `SRC/interpreter/OpenSeesCommands.cpp` | global command dispatch and domain accessors.
- `SRC/runtime/commands/modeling/model.cpp` | model command registration/parse behavior.
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp` | uniaxial material parse/dispatch behavior.
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp` | nD material parse/dispatch behavior.
- `SRC/runtime/commands/domain/domain.cpp` | domain lifecycle operations.
- `SRC/runtime/commands/analysis/analysis.cpp` | analyze command execution path.
- `SRC/runtime/commands/analysis/solver.cpp` | solver/system command behavior.
- `SRC/runtime/commands/analysis/integrator.cpp` | integrator command behavior.
- `SRC/runtime/commands/analysis/algorithm.cpp` | algorithm command behavior.
- `SRC/tcl/commands.cpp` | legacy Tcl command table.
- `SRC/tcl/commands.h` | legacy Tcl command interfaces.
- `SRC/recorder/TclRecorderCommands.cpp` | recorder command parser.
- `SRC/runtime/commands/packages.cpp` | runtime package command plumbing.
