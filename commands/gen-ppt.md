---
description: Generate a pptx from an Excel/Word/PDF input file using the bundled master template
argument-hint: <input-file> [output-path]
---

Use the `ppt-orchestrator` agent to generate a pixel-faithful pptx from this input: $ARGUMENTS

If the user provided only one argument, default the output path to `output/<input-stem>.pptx` (resolve relative to the plugin root). Pass both paths to the orchestrator and relay its final report to the user.
