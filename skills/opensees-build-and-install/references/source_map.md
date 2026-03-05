# opensees source map: Build and Install

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files tied to build behavior.

## Fast source navigation
- `rg -n "OPS_FINAL_TARGET|CMAKE_DISABLE_IN_SOURCE_BUILD|add_subdirectory" CMakeLists.txt SRC DEVELOPER cmake`
- `rg -n "find_package|target_link_libraries|option\(" cmake/cmake CMakeLists.txt`

## Function-level/build entry points
- `CMakeLists.txt` | top-level options (`OPS_FINAL_TARGET`, in-source guard, target routing).
- `cmake/cmake/OpenSeesFunctions.cmake` | helper functions/macros used by build targets.
- `cmake/cmake/OpenSeesDependenciesUnix.cmake` | Unix dependency discovery and link mapping.
- `cmake/cmake/OpenSeesDependenciesWin.cmake` | Windows dependency handling.
- `cmake/cmake/conan.cmake` | Conan bootstrap integration.
- `SRC/runtime/CMakeLists.txt` | runtime library target composition.
- `SRC/interpreter/CMakeLists.txt` | Tcl/Python interpreter build integration.
- `SRC/tcl/CMakeLists.txt` | Tcl executable/runtime target wiring.
- `SRC/runtime/executable/CMakeLists.txt` | final executable target assembly.
- `DEVELOPER/CMakeLists.txt` | developer extension CMake entry.
- `DEVELOPER/element/cpp/CMakeLists.txt` | sample extension target wiring.
