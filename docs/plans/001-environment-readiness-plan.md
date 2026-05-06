# Environment Readiness Plan

## Summary

Prepare the workspace for planning and later implementation by organizing documents, merging agent rules, and establishing docs, plans, specs, architecture, research, and logs folders.

## Current State

- Project MCP configs exist for Cursor and VS Code/Copilot.
- MCP profiles exist in `mcp-profiles.json`.
- Project skills are installed across agents.
- Research files were previously loose in the root folder.

## Implementation Steps

1. Keep active agent instructions in root `AGENTS.md`.
2. Keep MCP profile configuration at root in `mcp-profiles.json`.
3. Store all planning and research docs under `docs/`.
4. Move loose research files into accurately named files under `docs/research/`.
5. Archive duplicate source material under `docs/archive/duplicates/`.
6. Keep imported external instructions under `docs/references/`.
7. Use templates for future plans and notes.

## Test Plan

- Confirm root folder contains only active config and project entry files.
- Confirm docs folders exist.
- Confirm moved research files are present.
- Confirm `mcp-profiles.json` remains valid JSON.
- Confirm `AGENTS.md` includes merged verification, docs, and current-docs guidance.

## Assumptions

- This workspace is still in planning/pre-implementation mode.
- Root MCP files should remain at root for tool discovery.
- Raw source research should be preserved, not deleted.
