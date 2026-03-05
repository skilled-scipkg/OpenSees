# opensees documentation map: Theory and Methods

Use this map for method selection, convergence strategy, and theory-to-command mapping.
All paths below are verified in this repository.

## Theory references
- `SRC/doc/TrussTheory.tex` | truss formulation assumptions and equations.
- `SRC/doc/BeamTheory.tex` | beam formulation assumptions and equations.
- `SRC/doc/ReturnMap.tex` | constitutive return-mapping method reference.
- `SRC/doc/OpenSeesCommands.tex` | command-side mapping of theory choices.

## Method-oriented runnable checks
- `EXAMPLES/verification/sdofTransient.tcl` | transient method verification baseline.
- `EXAMPLES/verification/NewmarkIntegrator.tcl` | Newmark-focused transient check.
- `EXAMPLES/ExampleScripts/ArcLength01.tcl` | arc-length nonlinear method example.
- `tests/test_particle-dynamics.py` | transient dynamics regression test.
- `tests/test_force-beam-column.py` | nonlinear force-based element regression test.

## Validation checkpoints
- `OpenSees EXAMPLES/verification/sdofTransient.tcl`
- `OpenSees EXAMPLES/ExampleScripts/ArcLength01.tcl`
- `pytest -v tests/test_particle-dynamics.py tests/test_force-beam-column.py`
