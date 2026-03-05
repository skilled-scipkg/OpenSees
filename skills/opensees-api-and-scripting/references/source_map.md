# opensees source map: API and Scripting

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files with function-level checks.

## Fast source navigation
- `rg -n "PyInit_opensees|PYBIND11_MODULE|analyze\(|model" SRC/runtime SRC/interpreter`
- `rg -n "TclCommand_specifyModel|TclCommand_addNDMaterial|specifyIntegrator" SRC/runtime/commands`

## Function-level entry points
- `SRC/runtime/python/OpenSeesPyRT.cpp` | check `PYBIND11_MODULE(OpenSeesPyRT, m)` and Python runtime registration.
- `SRC/interpreter/OpenSeesCommandsPython.cpp` | check `PyInit_opensees` and Python command wrappers.
- `SRC/interpreter/OpenSeesCommandsTcl.cpp` | inspect Tcl-facing command binding surface.
- `SRC/interpreter/OpenSeesCommands.cpp` | shared command object model (`OPS_GetDomain`, command dispatch state).
- `SRC/runtime/executable/main.cpp` | executable startup and `LoadOpenSeesRT` library load.
- `SRC/runtime/OpenSeesRT.cpp` | package registration (`Tcl_PkgProvide`) and runtime init.
- `SRC/runtime/commands/modeling/model.cpp` | `TclCommand_specifyModel` argument handling and defaults.
- `SRC/runtime/commands/modeling/uniaxialMaterial.cpp` | uniaxial material command parser behavior.
- `SRC/runtime/commands/modeling/material/nDMaterial.cpp` | `TclCommand_addNDMaterial` parser/dispatch behavior.
- `SRC/runtime/commands/analysis/analysis.cpp` | `builder->analyze(...)` invocation path.
- `SRC/runtime/commands/analysis/integrator.cpp` | `specifyIntegrator` command parsing.
