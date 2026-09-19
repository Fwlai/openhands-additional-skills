# Evidence and Validation Reference

## Purpose

This reference defines how the agent should evaluate technical evidence, validate conclusions, reproduce findings, and prevent unsupported claims.

It is especially important for reverse engineering, APK analysis, binary analysis, runtime investigation, and large software changes.

The objective is to make conclusions reproducible and distinguish verified facts from interpretation.

## 1. Evidence Classification

Every important technical conclusion should use one of these classifications.

### Observed

Directly visible, measured, or reproduced.

Examples:

- A file exists.
- A method is called during runtime.
- A crash occurs with a specific stack trace.
- A resource has a specific ID.
- A binary contains a specific string.

Observed evidence should identify how it was observed.

### Recovered

Directly reconstructed from source, bytecode, Smali, native code, resources, or binary structures.

Examples:

- A method reads a particular field.
- A parser reads a specific binary offset.
- A native function is registered through JNI.
- A DEX class references a specific resource.

Recovered evidence should identify the artifact and location.

### Inferred

A strong interpretation supported by multiple independent pieces of evidence.

Examples:

- Several methods and assets indicate that a subsystem manages inventory.
- A loader and binary structure together indicate a custom map format.
- Runtime behavior matches the recovered code path.

Explain the evidence supporting the inference.

### Hypothesis

A plausible explanation that has not been sufficiently verified.

Hypotheses are useful for deciding what to investigate next.

Never present a hypothesis as a recovered fact.

## 2. Evidence Records

Important findings should be recorded with:

- unique ID
- classification
- confidence
- claim
- evidence
- source
- source location
- related entities
- reproduction method
- validation status
- unresolved questions

Example:

ID:
worldgen-001

Classification:
Recovered

Claim:
A world-generation routine initializes a random-number generator using a stored seed.

Evidence:
Class X, method Y, field Z.

Validation:
Confirmed through static analysis and runtime logging.

## 3. Source Traceability

Every important claim should be traceable to its source.

Possible sources include:

- source code
- DEX
- Smali
- native library
- resource
- asset
- configuration
- runtime log
- crash trace
- generated output
- binary structure
- version comparison

For source code record:

- file
- line or symbol

For DEX or Smali record:

- class
- method
- field
- relevant instruction

For native code record:

- library
- architecture
- symbol or address when available

For assets record:

- file
- format
- relevant metadata

For runtime evidence record:

- timestamp
- log source
- relevant event

## 4. Independent Evidence

Prefer multiple independent sources when possible.

For example:

Static code evidence
+
Asset evidence
+
Runtime observation

is stronger than a single class name suggesting the same behavior.

Independent evidence should actually be independent.

Repeated references to the same underlying fact do not automatically constitute independent confirmation.

## 5. Confidence

Confidence should represent evidence quality.

Suggested interpretation:

Very High:
Directly observed or directly recovered and independently validated.

High:
Strongly supported by multiple sources.

Medium:
Supported by meaningful evidence but with unresolved uncertainty.

Low:
Plausible interpretation with limited evidence.

Very Low:
Primarily speculative.

Confidence must never be used to disguise missing evidence.

## 6. Hypothesis Management

When a hypothesis is created:

1. state the hypothesis
2. explain why it is plausible
3. identify supporting evidence
4. identify contradictory evidence
5. identify what would confirm it
6. identify what would disprove it

Example:

Hypothesis:
The world is generated per chunk using a deterministic seed.

Supporting evidence:
- chunk-related classes
- deterministic random calls
- chunk coordinates passed into generation methods

Missing evidence:
- direct seed initialization
- confirmation that identical coordinates produce identical results

Next test:
Run generation twice using the same seed and chunk coordinates.

## 7. Contradictory Evidence

Do not discard evidence merely because it conflicts with the current model.

When contradictory evidence appears:

1. record it
2. identify the conflict
3. determine whether the evidence concerns different versions or states
4. inspect surrounding code
5. test the behavior
6. update the model if necessary

The latest hypothesis is not automatically correct.

## 8. Version Awareness

Always distinguish between versions.

A behavior recovered from one APK version must not automatically be attributed to another version.

Record:

- artifact version
- package version
- hash
- analysis date
- relevant build information

When comparing versions, explicitly identify which version supports each conclusion.

## 9. Static Validation

Static conclusions should be checked through:

- references
- call chains
- data flow
- resource relationships
- inheritance
- serialization structures
- native boundaries
- configuration
- binary structures

A single symbol name is weak evidence.

A symbol combined with callers, data flow, assets, and runtime behavior is stronger evidence.

## 10. Runtime Validation

When runtime access is available, use controlled experiments.

A useful experiment should define:

- initial state
- input
- expected observation
- actual observation
- relevant logs
- relevant files
- conclusion

Example:

Initial state:
New world with seed X.

Input:
Generate region (10, 20).

Expected:
Deterministic result.

Observed:
Same terrain and object placement on repeated generation.

Conclusion:
Deterministic behavior is supported for this test case.

Do not generalize beyond the tested conditions without additional evidence.

## 11. Reproducibility

A finding should be reproducible whenever practical.

Record:

- commands
- input files
- versions
- parameters
- environment
- expected result
- observed result

Avoid relying on undocumented manual steps.

When possible, turn repeated procedures into scripts.

## 12. Differential Testing

For uncertain behavior, compare controlled variations.

Possible variables:

- seed
- coordinates
- input
- configuration
- asset
- APK version
- device state
- save state

Change one meaningful variable at a time whenever practical.

This helps isolate causality.

## 13. Binary Validation

For unknown binary formats validate proposed structures against multiple samples.

For each candidate structure:

- identify magic bytes
- identify fixed fields
- identify variable fields
- identify lengths
- identify offsets
- compare multiple files
- check consistency
- test parser output

A structure that works for one file is not automatically a recovered format.

## 14. Asset Validation

When associating an asset with a system, look for:

- code references
- resource references
- loading paths
- naming relationships
- metadata
- runtime loading
- visual behavior

Do not conclude that similarly named assets belong to the same subsystem without additional evidence.

## 15. Call-Graph Validation

A call graph should distinguish:

- direct calls
- indirect calls
- reflection
- callbacks
- native calls
- dynamically registered methods

Do not treat a textual reference as proof that execution reaches the referenced method.

When possible, confirm execution through:

- control flow
- runtime traces
- instrumentation
- logs
- known entry points

## 16. Reflection and Dynamic Loading

Special care is required for:

- reflection
- dynamic class loading
- generated classes
- JNI registration
- dynamically loaded native libraries
- resource lookup by string
- plugin systems

Static references may be incomplete.

Search for:

- class names stored as strings
- method names stored as strings
- reflection APIs
- class loaders
- library loading
- native registration tables

## 17. Encrypted and Compressed Data

Do not assume encrypted data is unreadable.

Investigate:

- where it is loaded
- where it is decompressed
- where keys or parameters originate
- which functions process it
- whether plaintext exists in memory
- whether configuration controls the process

Distinguish:

Compressed:
Data transformed for size reduction.

Encoded:
Data transformed for representation.

Encrypted:
Data transformed to prevent direct interpretation.

Obfuscated:
Data or code intentionally made harder to understand.

These categories are not interchangeable.

## 18. Runtime vs Static Mismatch

When runtime behavior contradicts static analysis:

1. verify the APK version
2. verify the loaded code path
3. verify dynamic loading
4. inspect configuration
5. inspect native components
6. inspect generated data
7. check for cached or persisted state
8. repeat the experiment

Do not immediately assume the static analysis is wrong.

## 19. Regression Validation

After modifying code:

- reproduce the original problem
- apply the change
- repeat the same test
- test relevant neighboring behavior
- run broader tests when appropriate

A fix is stronger when the original failure is demonstrably gone.

## 20. Build Validation

A successful build proves only that the build pipeline completed.

It does not prove behavioral correctness.

Validation should distinguish:

Build:
The project compiles and packages.

Installation:
The resulting artifact installs.

Launch:
The application starts.

Behavior:
The requested feature works.

Regression:
Existing relevant behavior remains correct.

## 21. APK Validation

For generated APKs verify:

- package name
- version
- signing
- ABI
- manifest
- expected resources
- expected DEX/native libraries
- installation
- launch
- target functionality

When comparing a modified APK with the original, document both intentional and unexpected differences.

## 22. Change Validation

Before finalizing a code change:

1. inspect modified files
2. inspect Git diff
3. verify dependencies
4. run targeted tests
5. run broader tests when appropriate
6. build
7. install when applicable
8. run the relevant functionality
9. inspect logs
10. document limitations

## 23. Unsupported Claims

Never claim:

- an exact algorithm without evidence
- an exact original source implementation from imperfect decompilation
- a file format without validation
- an engine feature based only on appearance
- a runtime behavior based only on names
- a recovered system based only on speculation

Instead use precise language such as:

Observed:
The application loads this file.

Recovered:
Method X parses this field.

Inferred:
The field appears to control world-generation parameters.

Hypothesis:
The value may determine structure placement.

## 24. Stopping Conditions

Do not continue analysis indefinitely.

Stop when:

- the requested question has sufficient evidence
- remaining uncertainty cannot be reduced with available artifacts
- additional analysis would have low value
- the next useful step requires new evidence

Record unresolved questions instead of filling gaps with speculation.

## 25. Validation Summary

Every major investigation should end with:

Confirmed:
Facts directly supported by evidence.

Strongly supported:
Conclusions supported by multiple sources.

Uncertain:
Interpretations requiring more evidence.

Next:
The smallest useful experiment or artifact needed to resolve the remaining uncertainty.
