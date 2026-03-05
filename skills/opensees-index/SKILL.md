---
name: opensees-index
description: This skill should be used when users ask how to use opensees and the correct generated documentation skill must be selected before going deeper into source code.
---

# opensees Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer abstract, workflow-level guidance for large scientific packages; do not attempt full function-by-function coverage unless explicitly requested.

## Generated topic skills
- `opensees-examples-and-tutorials`: Examples and Tutorials (worked examples, tutorials, and cookbook usage)
- `opensees-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `opensees-api-and-scripting`: API and Scripting (language bindings, APIs, and programmatic interfaces)
- `opensees-inputs-and-modeling`: Inputs and Modeling (inputs, system setup, models, and physical parameterization)
- `opensees-theory-and-methods`: Theory and Methods (theoretical background and algorithmic methods)
- `opensees-developer-guide`: Developer Guide (developer architecture, extension points, and contribution workflow)
- `opensees-simulation-workflows`: Simulation Workflows (simulation setup, execution flow, and runtime controls)
- `opensees-src`: Src (documentation grouped under the 'src' theme)

## Documentation-first inputs
- `DEVELOPER`
- `SRC/doc`

## Tutorials and examples roots
- `EXAMPLES`
- `Workshops`

## Test roots for behavior checks
- `tests`
- `EXAMPLES/ExamplesForTesting`
- `SRC/unittest`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, open the topic skill map file (for example `skills/opensees-inputs-and-modeling/references/doc_map.md`).
- If documentation still leaves ambiguity, open the topic skill source map (for example `skills/opensees-inputs-and-modeling/references/source_map.md`) and inspect the suggested source entry points.
- Use targeted symbol search while inspecting source (e.g., `rg -n "<symbol_or_keyword>" DEVELOPER SRC`).

## Source directories for deeper inspection
- `DEVELOPER`
- `SRC`
