# opensees source map: Developer Guide

Use this map only after exhausting topic docs in `doc_map.md`.
All entry points below are concrete files for extension behavior checks.

## Fast source navigation
- `rg -n "class .*Material|setTrialStrain|getStress|sendSelf|recvSelf" DEVELOPER/core DEVELOPER/material`
- `rg -n "elementAPI|materialAPI|TclPackageClassBroker|OPS_GetDomain" DEVELOPER SRC`

## Function-level entry points
- `DEVELOPER/core/UniaxialMaterial.h` | uniaxial material interface contract.
- `DEVELOPER/core/UniaxialMaterial.cpp` | default/base implementation behavior.
- `DEVELOPER/core/NDMaterial.h` | nD material interface contract.
- `DEVELOPER/core/NDMaterial.cpp` | base implementation and virtual hooks.
- `DEVELOPER/core/elementAPI.h` | element extension API boundary.
- `DEVELOPER/material/fortran/materialAPI.f` | Fortran material API shims.
- `DEVELOPER/element/fortran/elementAPI.f` | Fortran element API shims.
- `DEVELOPER/material/cpp/ElasticPPcpp.cpp` | C++ material example implementation.
- `DEVELOPER/element/cpp/Truss2D.cpp` | C++ element example implementation.
- `DEVELOPER/system/SkylineSPD.cpp` | system component extension example.
- `SRC/runtime/runtime/TclPackageClassBroker.cpp` | package/class broker integration path.
- `SRC/interpreter/OpenSeesCommands.cpp` | command-to-domain integration context.
