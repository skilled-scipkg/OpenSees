# opensees documentation map: Inputs and Modeling

Use this map for command-level model assembly and parameterization.
All paths below are verified in this repository.

## Core modeling docs
- `SRC/doc/OpenSeesCommands.tex` | core command syntax and sequencing.
- `SRC/doc/NewMaterial.tex` | material command conventions and extension context.
- `SRC/doc/Elastic3DCommands.tex` | representative nD elastic material commands.
- `SRC/doc/Template3DEPCommands.tex` | 3D elasto-plastic material command patterns.
- `SRC/doc/BrickCommands.tex` | continuum/brick modeling command guidance.

## Runnable modeling examples
- `EXAMPLES/ExampleScripts/Example1.1.tcl` | minimal model + static analysis.
- `EXAMPLES/ExampleScripts/truss1.tcl` | compact truss model setup.
- `EXAMPLES/ExampleScripts/RCFrame1.tcl` | multi-component frame modeling.
- `EXAMPLES/ExamplePython/Example1.1.py` | Python mirror of baseline setup.
- `EXAMPLES/ExamplesForTesting/Test.truss1.ops` | regression script for basic model path.

## Validation checkpoints
- `OpenSees EXAMPLES/ExampleScripts/Example1.1.tcl`
- `OpenSees EXAMPLES/ExampleScripts/truss1.tcl`
- `python EXAMPLES/ExamplePython/Example1.1.py`
