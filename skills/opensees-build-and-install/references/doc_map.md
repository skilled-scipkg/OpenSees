# opensees documentation map: Build and Install

Use this map after the skill playbook for platform/toolchain specific build setup.
All paths below are verified in this repository.

## Build setup and packaging docs
- `README.md` | repository orientation and build context.
- `CMakeLists.txt` | top-level CMake options and target selection.
- `conanfile.py` | Conan dependency definitions (legacy path).
- `conanfile2.py` | Conan v2 dependency path.
- `DEVELOPER/UnixInstructions.txt` | Unix extension-build notes.
- `DEVELOPER/WindowsInstructions.txt` | Windows extension-build notes.

## Build scripts and legacy paths
- `cmake/makeUbuntu.sh` | Linux shell build flow.
- `OpenSeesAWS-Ubuntu22.04.sh` | practical Ubuntu bootstrap script.
- `Makefile` | legacy make entry point.
- `MAKES/Makefile.def.EC2-UBUNTU` | representative legacy make config.
- `MAKES/Makefile.def.Ubuntu20.04` | modern Ubuntu make config example.

## Validation checkpoints
- `cmake .. -DOPS_FINAL_TARGET=OpenSees`
- `cmake --build . --target OpenSees --parallel 4`
- `./OpenSees ../EXAMPLES/ExampleScripts/Example1.1.tcl`
