# opensees source map: Simulation Workflows

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for runtime execution checks.

## Fast source navigation
- `rg -n "analyze\(|wipeAnalysis|Transient|Static|specifyIntegrator|TclCommand_specifyAlgorithm" SRC/runtime/commands/analysis`
- `rg -n "TclCommand_addRecorder|record\(|closeOnWrite" SRC/recorder`

## Function-level entry points
- `SRC/runtime/commands/analysis/analysis.cpp` | `analyze` command handling and return paths.
- `SRC/runtime/commands/analysis/transient.cpp` | transient execution command path.
- `SRC/runtime/commands/analysis/algorithm.cpp` | algorithm command parser (`TclCommand_specifyAlgorithm`).
- `SRC/runtime/commands/analysis/integrator.cpp` | integrator parser (`specifyIntegrator`).
- `SRC/runtime/commands/analysis/ctest.cpp` | convergence-test command parser.
- `SRC/runtime/commands/analysis/solver.cpp` | system/solver selection behavior.
- `SRC/runtime/commands/domain/loading/TclSeriesIntegratorCommand.cpp` | series/time-step helper command path.
- `SRC/runtime/runtime/BasicAnalysisBuilder.cpp` | analysis object assembly at runtime.
- `SRC/runtime/runtime/BasicModelBuilder.cpp` | model-to-analysis bridge and state.
- `SRC/recorder/TclRecorderCommands.cpp` | recorder creation command path.
- `SRC/recorder/NodeRecorder.cpp` | nodal recorder write behavior.
- `SRC/recorder/ElementRecorder.cpp` | element recorder write behavior.
