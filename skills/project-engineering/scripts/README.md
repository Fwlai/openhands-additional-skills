# Project Engineering Scripts

This directory contains reusable scripts used by the project-engineering skill.

Scripts should support:

- APK inspection
- APK extraction
- manifest analysis
- DEX and Smali analysis
- native library analysis
- asset discovery
- binary and file-format analysis
- resource analysis
- APK version comparison
- runtime data collection
- logcat collection
- crash analysis
- build validation
- APK installation and testing
- evidence collection
- report generation
- project/codebase analysis
- reproducible experiments

Scripts should be:

- deterministic where possible
- safe by default
- non-destructive unless explicitly requested
- usable from command-line environments
- compatible with Termux when practical
- explicit about failures
- documented with usage examples
- independent from project-specific hardcoded paths
- designed to preserve raw evidence
- suitable for automation by OpenHands

## Script Design Rules

A script should:

1. Clearly state its purpose.
2. Validate its inputs.
3. Avoid modifying source artifacts unless explicitly intended.
4. Record important errors.
5. Return meaningful exit codes.
6. Produce machine-readable output when practical.
7. Preserve original evidence.
8. Avoid silently discarding information.
9. Make assumptions explicit.
10. Be reproducible.

## Analysis Pipeline

Scripts may be composed into larger workflows:

    Input
      ↓
    Detection
      ↓
    Extraction
      ↓
    Analysis
      ↓
    Cross-reference
      ↓
    Validation
      ↓
    Evidence
      ↓
    Report

The agent should select only the tools required for the current task.

## Self-Extension

When existing scripts cannot analyze a discovered format, artifact, or behavior, the agent may create a new focused analysis script.

Before creating a new script:

- verify that an existing script cannot perform the task
- define the required input and output
- keep the new script narrowly scoped
- document the script
- test it against known input
- preserve reproducibility

New scripts should solve reusable analysis problems rather than encode one-off conclusions.

## Evidence Handling

Scripts that produce analytical findings should preserve enough information to trace the finding back to the original artifact.

Useful metadata includes:

- input path
- input hash
- tool/version
- command
- timestamp
- output path
- detected format
- analysis status
- errors or warnings

A script must not convert an uncertain result into a definitive claim.

## Output Conventions

Prefer:

- stdout for concise human-readable results
- files for detailed reports
- JSON or another structured format for machine-readable findings
- stderr for errors and diagnostic information
- non-zero exit codes for failures

Do not mix large diagnostic output with structured machine-readable output.

## Tool Detection

Scripts should detect whether required external tools are available before attempting to use them.

If a required tool is unavailable:

- report which tool is missing
- explain which analysis cannot be performed
- preserve everything that can still be analyzed
- do not fabricate results

The agent may choose an alternative supported tool when equivalent functionality exists.

## Safety

Never overwrite the original APK, binary, asset, source file, or evidence artifact by default.

Prefer:

- separate output directories
- copied working files
- immutable evidence
- explicit output paths

Potentially destructive operations require explicit intent.

## Testing Scripts

Every important script should be tested with:

- valid input
- invalid input
- missing input
- empty input where applicable
- unexpected format where applicable
- large input when practical

Document known limitations.

## Script Documentation

Each script should have documentation covering:

- purpose
- inputs
- outputs
- dependencies
- usage
- limitations
- exit codes
- example invocation

Keep this README updated when the script collection changes significantly.
