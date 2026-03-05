# opensees source map: Theory and Methods

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for function-level method checks.

## Fast source navigation
- `rg -n "TclCommand_specifyAlgorithm|specifyIntegrator|test|analyze\(" SRC/runtime/commands/analysis`
- `rg -n "Newton|ModifiedNewton|KrylovNewton|LineSearch" SRC/analysis/algorithm`

## Function-level entry points
- `SRC/runtime/commands/analysis/algorithm.cpp` | algorithm command parser and option routing.
- `SRC/runtime/commands/analysis/integrator.cpp` | integrator command parser and argument handling.
- `SRC/runtime/commands/analysis/solver.cpp` | linear-system/solver command mapping.
- `SRC/runtime/commands/analysis/ctest.cpp` | convergence test command parsing.
- `SRC/runtime/commands/analysis/analysis.cpp` | `analyze` stepping invocation logic.
- `SRC/analysis/algorithm/SolutionAlgorithm.h` | base algorithm interface.
- `SRC/analysis/algorithm/SolutionAlgorithm.cpp` | shared algorithm behavior.
- `SRC/analysis/algorithm/equiSolnAlgo/NewtonRaphson.cpp` | Newton-Raphson implementation details.
- `SRC/analysis/algorithm/equiSolnAlgo/ModifiedNewton.cpp` | modified Newton behavior.
- `SRC/analysis/algorithm/equiSolnAlgo/KrylovNewton.cpp` | Krylov-Newton behavior.
- `SRC/analysis/algorithm/equiSolnAlgo/NewtonLineSearch.cpp` | line-search Newton behavior.
- `SRC/analysis/integrator/Newmark.cpp` | Newmark integrator implementation.
- `SRC/analysis/integrator/LoadControl.cpp` | static load-step integrator implementation.
