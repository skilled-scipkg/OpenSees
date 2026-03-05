---
name: opensees-build-and-install
description: This skill should be used when users ask about build and install in opensees; it prioritizes documentation references and then source inspection only for unresolved details.
---

# opensees: Build and Install

## High-Signal Playbook
### Route conditions
- Use this skill for toolchain setup, dependency resolution, `cmake`/`make` build failures, or selecting the correct build target (`OpenSees`, `OpenSeesPy`, `OpenSeesMP`, `OpenSeesSP`).
- Route model authoring and command usage questions to `opensees-inputs-and-modeling`.
- Route Tcl/Python command parity questions to `opensees-api-and-scripting`.
- Route deep runtime behavior/debugging to `opensees-src`.

### Triage questions
- Which OS and compiler are in use (Linux/macOS/Windows, GCC/Clang/MSVC/Intel)?
- Which output target is required (`OpenSees`, `OpenSeesPy`, `OpenSeesMP`, `OpenSeesSP`)?
- Is the build path CMake+Conan or legacy `Makefile.def`?
- Are Tcl/HDF5/ZLIB/Eigen detected by CMake?
- Is MPI/MUMPS required (`OpenSeesMP` / distributed solve path)?
- Is this a contributor build that also needs `DEVELOPER` plugin artifacts?

### Canonical workflow
1. Pick build system: CMake (default) vs legacy make.
2. Install compilers and baseline dependencies.
3. Create out-of-source build directory (`CMAKE_DISABLE_IN_SOURCE_BUILD` is enabled in `CMakeLists.txt`).
4. Run dependency bootstrap (`conan install` if using Conan path).
5. Configure with correct target/options.
6. Build specific targets first, then additional targets only if needed.
7. Run smoke test script (`Example1.1`) before debugging performance/physics issues.
8. Only after docs/config checks, inspect CMake and source entry files.

### Minimal working example
```bash
mkdir -p build
cd build
conan install .. --build missing
cmake .. -DCMAKE_BUILD_TYPE=Release -DOPS_FINAL_TARGET=OpenSees
cmake --build . --target OpenSees --parallel 4
./OpenSees ../EXAMPLES/ExampleScripts/Example1.1.tcl
```

```bash
# Legacy path (uses root Makefile + Makefile.def)
cp MAKES/Makefile.def.EC2-UBUNTU Makefile.def
make OpenSees
```

### Pitfalls and fixes
- In-source configure/build fails: configure inside a dedicated `build` directory only (`CMAKE_DISABLE_IN_SOURCE_BUILD ON` in `CMakeLists.txt`).
- Tcl found but runtime init missing: verify `TCL_INIT_FILE` detection path and include/library mapping in `CMakeLists.txt`.
- Wrong binary gets built by default: set `-DOPS_FINAL_TARGET=<target>`.
- Windows DLL runs in Win32 but not x64: follow the x64 platform switch in `DEVELOPER/WindowsInstructions.txt`.
- Developer plugin tests cannot find shared libs: include `./` in `LD_LIBRARY_PATH` per `DEVELOPER/UnixInstructions.txt`.
- Legacy make path fails quickly: confirm `Makefile.def` copied/edited from `MAKES/` before `make`.

### Convergence and validation checks
- Build artifacts exist for intended target(s): `OpenSees`, `OpenSeesPy` (`opensees.so`), or `OpenSeesMP`.
- Script smoke test runs without parser/runtime errors (`EXAMPLES/ExampleScripts/Example1.1.tcl`).
- Python binding check succeeds (`python -c "import opensees as ops"`) when `OpenSeesPy` is built.
- `pytest -v tests` runs for interface-level sanity after major toolchain changes.

### Source-code jump table
- Top-level build orchestration/options: `CMakeLists.txt`
- Dependency loaders and helper macros: `cmake/cmake/OpenSeesFunctions.cmake`, `cmake/cmake/OpenSeesDependenciesUnix.cmake`, `cmake/cmake/OpenSeesDependenciesWin.cmake`
- Reference build scripts: `cmake/makeUbuntu.sh`, `OpenSeesAWS-Ubuntu22.04.sh`, `cmake/makeFrontera.sh`
- Legacy build entry: `Makefile`, `MAKES/Makefile.def.EC2-UBUNTU`, `MAKES/Makefile.def.Ubuntu20.04`
- Developer extension build paths: `DEVELOPER/CMakeLists.txt`, `DEVELOPER/element/cpp/CMakeLists.txt`

Doc anchors: `DEVELOPER/UnixInstructions.txt`, `DEVELOPER/WindowsInstructions.txt`, `CMakeLists.txt`, `Makefile`, `cmake/makeUbuntu.sh`, `OpenSeesAWS-Ubuntu22.04.sh`.

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `DEVELOPER/UnixInstructions.txt`
- `DEVELOPER/WindowsInstructions.txt`
- `CMakeLists.txt`
- `Makefile`
- `cmake/makeUbuntu.sh`
- `OpenSeesAWS-Ubuntu22.04.sh`
- `DEVELOPER/CMakeLists.txt`
- `DEVELOPER/element/cpp/CMakeLists.txt`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `EXAMPLES`
- `Workshops`

## Test references
- `tests`
- `EXAMPLES/ExamplesForTesting`
- `SRC/unittest`

## Optional deeper inspection
- `DEVELOPER`
- `SRC`

## Source entry points for unresolved issues
- `CMakeLists.txt`
- `cmake/cmake/OpenSeesFunctions.cmake`
- `cmake/cmake/OpenSeesDependenciesUnix.cmake`
- `cmake/cmake/OpenSeesDependenciesWin.cmake`
- `Makefile`
- `MAKES/Makefile.def.EC2-UBUNTU`
- `DEVELOPER/CMakeLists.txt`
- `DEVELOPER/element/cpp/CMakeLists.txt`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" DEVELOPER SRC CMakeLists.txt cmake`).
