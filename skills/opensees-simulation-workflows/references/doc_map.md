# opensees documentation map: Simulation Workflows

Use this map for execution flow, stepping strategy, and run validation.
All paths below are verified in this repository.

## Run-control docs and scripts
- `tests/README.md` | test execution and Python import workflow.
- `EXAMPLES/ExampleScripts/Example1.1.tcl` | minimal static execution flow.
- `EXAMPLES/verification/sdofTransient.tcl` | transient control baseline.
- `EXAMPLES/verification/NewmarkIntegrator.tcl` | integrator-focused transient check.
- `EXAMPLES/ExampleScripts/TimeSeriesTest.tcl` | time-series driven workflow.
- `EXAMPLES/ExamplePython/example_variable_analysis.py` | iterative control with `getTime()`.

## Parallel or partitioned run examples
- `EXAMPLES/ParallelModelMP/exampleMP.tcl` | MP workflow entry.
- `EXAMPLES/ParallelModelMP/exampleSP.tcl` | SP/MP comparison baseline.
- `EXAMPLES/SmallMP/analysis.tcl` | split analysis script for staged runs.
- `EXAMPLES/SmallMP/model.tcl` | paired model script for staged runs.

## Validation checkpoints
- `OpenSees EXAMPLES/verification/sdofTransient.tcl`
- `OpenSees EXAMPLES/ExampleScripts/TimeSeriesTest.tcl`
- `pytest -v tests`
