# opensees source map: Examples and Tutorials

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for behavior tracing.

## Fast source navigation
- `rg -n "LoadOpenSeesRT|InterpreterAPI|TclCommand_addRecorder|record\(" SRC/runtime SRC/interpreter SRC/recorder`
- `rg -n "analyze\(|specifyIntegrator|TclCommand_specifyAlgorithm" SRC/runtime/commands/analysis`

## Function-level entry points
- `SRC/runtime/executable/main.cpp` | script startup and runtime library loading.
- `SRC/runtime/OpenSeesRT.cpp` | package initialization used by Tcl entry path.
- `SRC/runtime/python/OpenSeesPyRT.cpp` | Python runtime module init and command exposure.
- `SRC/runtime/parsing/InterpreterAPI.cpp` | script parsing and interpreter bridge.
- `SRC/interpreter/OpenSeesTimeSeriesCommands.cpp` | time-series command parsing used by many examples.
- `SRC/runtime/commands/analysis/analysis.cpp` | `analyze` command execution path.
- `SRC/runtime/commands/analysis/transient.cpp` | transient-analysis command behavior.
- `SRC/recorder/TclRecorderCommands.cpp` | recorder command parser (`Node`, `Element`, `-closeOnWrite`).
- `SRC/recorder/NodeRecorder.cpp` | node recorder runtime write conditions.
- `SRC/recorder/ElementRecorder.cpp` | element recorder runtime write conditions.
- `SRC/recorder/floating-point-tolerance-for-recorder-time-step.md` | recorder timestep tolerance rationale.
