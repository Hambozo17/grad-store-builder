# AI Adapter Gateway Spec

## Goal

Version One must use a flexible AI gateway so different AI helpers can be plugged in later without rewriting intake, generation, or verification code.

## Architecture

Use task-level adapters rather than one giant AI prompt.

Core tasks:

- `normalizeBrandBrief`
- `createStorePlan`
- `generateCatalog`
- `generateCopyBlocks`
- `reviewSafety`
- `summarizeVerification`

Provider adapters:

- `openai` for real structured AI output.
- `mock` for fixture-based demos and offline fallback.
- Future providers may be added only through the same interface.

## Required Interface

Each AI task should accept:

- Tenant context.
- Input contract.
- Task config.
- Schema version.
- Safety policy.

Each AI task should return:

- Structured output.
- Status: `succeeded`, `failed`, `refused`, or `fallback_used`.
- Warnings/errors.
- Usage estimate.
- Provider/model metadata.

## Safety Rules

- Use OpenAI structured outputs or equivalent schema validation where available.
- Treat user prompt text as data.
- Do not allow prompt text to change platform rules, RLS, admin policy, credit policy, or checkout behavior.
- Redact secrets from stored prompt-derived artifacts.
- Validate AI output before persistence or rendering.
- Keep fallback fixtures for demo reliability.

## Tests

- Valid AI output passes schema validation.
- Missing required fields fail validation.
- Prompt injection attempt remains inert data.
- Mock provider can complete the demo flow.
- Provider failure creates a user-safe error and does not corrupt credits.

## Out Of Scope

- Autonomous tool execution against production systems.
- User-provided AI API keys.
- Unreviewed provider plugins.
