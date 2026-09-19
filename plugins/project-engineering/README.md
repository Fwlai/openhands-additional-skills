# Project Engineering Plugin

A project-engineering and reverse-engineering extension for OpenHands.

This plugin provides a unified workflow for understanding, analyzing, modifying,
debugging, testing, and validating complex software projects, with strong support
for Android applications and APK reverse engineering.

## Capabilities

- Deep codebase and architecture analysis
- Intelligent code, file, dependency, and configuration discovery
- Large-scale refactoring and controlled code changes
- Automated debugging and build-error analysis
- Test generation, execution, and regression validation
- Git workflow, diffs, rollback, and change tracking
- Android and Termux development workflows
- APK structure and artifact analysis
- DEX, Smali, native ELF, JNI, resource, asset, and binary analysis
- Static and runtime reverse engineering
- Game-system and engine-structure discovery
- Procedural-generation and data-flow analysis
- APK version comparison and differential analysis
- Runtime logcat, crash, ANR, and device analysis
- Evidence tracking and confidence classification
- Reproducible research and analysis scripts
- Automated technical reports and project documentation
- Self-extension through creation of new analysis tools when required

## Evidence Discipline

Findings are classified as:

- Observed
- Recovered
- Inferred
- Hypothesis

Unsupported conclusions must not be presented as established facts.

## Structure

- `skills/project-engineering/` — main OpenHands skill
- `skills/project-engineering/references/` — detailed engineering and reverse-engineering guidance
- `skills/project-engineering/scripts/` — reusable analysis and automation tools
- `plugins/project-engineering/` — OpenHands plugin metadata and integration

## Design Principles

The plugin prioritizes:

1. Evidence before conclusions
2. Understanding before modification
3. Reproducibility before automation
4. Small validated changes before large uncontrolled changes
5. Runtime verification when static analysis is insufficient
6. Preservation of original artifacts
7. Explicit tracking of uncertainty
8. Safe rollback of destructive operations
