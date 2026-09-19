---
name: project-engineering
description: Deep project engineering and APK reverse-engineering workflow for OpenHands. Use for complex codebases, architecture reconstruction, large-scale changes, debugging, testing, Android/Termux development, APK analysis, DEX/Smali/native analysis, binary and asset investigation, runtime analysis, and evidence-driven reverse engineering.
triggers:
  - project engineering
  - codebase analysis
  - architecture analysis
  - reverse engineering
  - APK analysis
  - Android analysis
  - Smali analysis
  - DEX analysis
  - native library analysis
  - binary analysis
  - asset analysis
  - runtime analysis
---

# Project Engineering

Use the main project-engineering skill as the authoritative workflow for this plugin.

Read:

`skills/project-engineering/SKILL.md`

Before performing specialized work, load the relevant references from:

`skills/project-engineering/references/`

Important references include:

- `apk-analysis.md`
- `codebase-engineering.md`
- `evidence-and-validation.md`
- `reverse-engineering.md`
- `android-runtime.md`

Reusable analysis and automation tools are located in:

`skills/project-engineering/scripts/`

## Operating Rules

Understand the project before making significant changes.

For reverse engineering, preserve original artifacts and distinguish:

- Observed
- Recovered
- Inferred
- Hypothesis

Do not present unsupported inferences as established facts.

Prefer reproducible analysis and machine-readable intermediate results.

For large changes:

1. Inspect the current state.
2. Build an evidence-backed plan.
3. Make changes in controlled stages.
4. Validate each stage.
5. Run relevant tests and builds.
6. Review the resulting diff.
7. Preserve a rollback path.

When static analysis is insufficient, use controlled runtime experiments and correlate runtime evidence with static findings.

When existing tooling cannot answer an important question, create a focused reusable analyzer rather than performing an undocumented one-off operation.
