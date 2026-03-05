# opensees source map: Inputs and Modeling

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for function-level modeling checks.

## Fast source navigation
- `rg -n "TclCommand_specifyModel|node|fix|element|uniaxialMaterial|nDMaterial" SRC/runtime/commands/modeling`
- `rg -n "TclModelBuilder.*MaterialCommand|invoke_material" SRC/material SRC/runtime/commands/modeling`

## Function-level entry points
- `SRC/runtime/commands/modeling/model.cpp` | `TclCommand_specifyModel` and default `ndf` behavior.
- `SRC/runtime/commands/modeling/nodes.cpp` | node creation/lookup command behavior.
- `SRC/runtime/commands/modeling/constraint.cpp` | `fix`/constraint command parsing.
- `SRC/runtime/commands/modeling/element.cpp` | high-level element command dispatch.
- `SRC/runtime/commands/modeling/element/truss.cpp` | truss element command parsing path.
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp` | uniaxial material parser logic.
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp` | nD material parser and dispatch.
- `SRC/runtime/commands/modeling/material/material.hpp` | material command table/types.
- `SRC/runtime/commands/modeling/invoking/invoke_material.cpp` | material-command invocation helpers.
- `SRC/material/uniaxial/TclModelBuilderUniaxialMaterialCommand.cpp` | legacy uniaxial parser bridge.
- `SRC/material/nD/TclModelBuilderNDMaterialCommand.cpp` | legacy nD parser bridge.
- `SRC/runtime/commands/domain/domain.cpp` | domain object add/remove/wipe lifecycle.
