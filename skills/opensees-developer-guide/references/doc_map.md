# opensees documentation map: Developer Guide

Use this map after the skill playbook for extension-oriented development.
All paths below are verified in this repository.

## Contributor docs and workflows
- `DEVELOPER/UnixInstructions.txt` | Unix extension build/run notes.
- `DEVELOPER/WindowsInstructions.txt` | Windows extension build/run notes.
- `DEVELOPER/Makefile` | top-level developer extension build.
- `DEVELOPER/coreList` | core classes used by developer examples.
- `DEVELOPER/makeCore.tcl` | helper script for core build-related flows.

## Extension examples
- `DEVELOPER/material/cpp/example1.tcl` | C++ material extension smoke test.
- `DEVELOPER/element/cpp/example1.tcl` | C++ element extension smoke test.
- `DEVELOPER/material/c/example1.tcl` | C material extension example.
- `DEVELOPER/element/c/example1.tcl` | C element extension example.
- `DEVELOPER/system/example1.tcl` | system extension example.

## Validation checkpoints
- `cd DEVELOPER && make`
- `OpenSees material/cpp/example1.tcl`
- `OpenSees element/cpp/example1.tcl`
