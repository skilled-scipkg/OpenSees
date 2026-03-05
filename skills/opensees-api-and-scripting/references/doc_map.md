# opensees documentation map: API and Scripting

Use this map after the skill primary references when Tcl/Python behavior diverges.
All paths below are verified in this repository.

## Core API documents
- `SRC/doc/g3API.tex` | runtime API architecture and extension interfaces.
- `SRC/doc/OpenSeesCommands.tex` | canonical command syntax shared across interfaces.
- `SRC/doc/OpenSeesExamples.tex` | command usage patterns used by shipped scripts.
- `tests/README.md` | Python import fallback and local test workflow.

## Runnable API examples
- `EXAMPLES/ExampleScripts/Example1.1.tcl` | minimal Tcl workflow.
- `EXAMPLES/ExamplePython/Example1.1.py` | minimal Python workflow.
- `EXAMPLES/ExamplePython/example_variable_analysis.py` | looped analysis and `getTime()` checks.
- `DEVELOPER/system/example1.tcl` | developer-side Tcl example for extension validation.

## Validation checkpoints
- `OpenSees EXAMPLES/ExampleScripts/Example1.1.tcl`
- `python EXAMPLES/ExamplePython/Example1.1.py`
- `pytest -v tests`
