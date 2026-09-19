# Reverse Engineering Reference

## Purpose

This reference defines a disciplined workflow for reverse engineering software, Android applications, APKs, native libraries, binary formats, assets, and runtime behavior.

The goal is to reconstruct technical behavior from available evidence while avoiding unsupported assumptions.

## 1. Reverse Engineering Principles

Reverse engineering should follow these principles:

- preserve original artifacts
- work on copies
- record hashes
- keep analysis reproducible
- separate evidence from interpretation
- validate important conclusions
- document uncertainty
- avoid modifying evidence during investigation
- prefer the smallest useful experiment
- preserve intermediate findings

Never assume that decompiled output is identical to the original source code.

## 2. Investigation Lifecycle

Use this general workflow:

1. Preserve artifact.
2. Fingerprint artifact.
3. Identify technology and architecture.
4. Extract and decode.
5. Inventory files and symbols.
6. Locate entry points.
7. Identify important subsystems.
8. Trace execution and data flow.
9. Analyze resources and assets.
10. Analyze native boundaries.
11. Perform runtime investigation when available.
12. Validate findings.
13. Record evidence.
14. Build a technical model.
15. Report confirmed and unresolved findings.

Do not jump directly into detailed code analysis before understanding the artifact structure.

## 3. Artifact Preservation

For every important artifact record:

- filename
- SHA-256
- size
- version
- architecture
- origin
- analysis date

Keep the original untouched.

Use separate directories for:

- originals
- extracted files
- decoded files
- generated analysis
- reports
- scripts

## 4. Technology Identification

Determine whether the target uses:

- native Android
- Java
- Kotlin
- C or C++
- Unity
- Unreal Engine
- Godot
- libGDX
- Mono or IL2CPP
- custom engines
- scripting engines
- embedded runtimes
- custom frameworks

Use evidence from:

- package names
- libraries
- class namespaces
- assets
- manifest
- native libraries
- strings
- resource structure
- executable metadata

Do not identify an engine from visual appearance alone.

## 5. Entry Point Discovery

Locate:

- Android application class
- launcher Activity
- initialization routines
- native initialization
- engine startup
- game startup
- main loops
- event dispatch
- rendering initialization

Trace from entry points toward important systems.

Useful model:

Entry Point
to Initialization
to Runtime
to Subsystem
to Data

## 6. Symbol Discovery

Collect:

- classes
- methods
- fields
- functions
- exports
- imports
- strings
- resource IDs
- native symbols
- JNI methods

Create searchable indexes for large targets.

When symbols are obfuscated, preserve the original identifier and assign a local semantic label.

Example:

Original:
a.b.c()

Analysis label:
probableWorldLoader()

Do not represent the analysis label as an original name.

## 7. Call-Chain Analysis

For important behavior trace:

Entry
to caller
to target method
to dependent method
to data source
to output

Record:

- direct calls
- indirect calls
- callbacks
- reflection
- native calls
- dynamically registered functions

A textual reference is not proof that a method executes.

## 8. Data-Flow Analysis

Trace important values through the program.

Examples:

seed
to RNG
to generator
to generated object
to world storage

item ID
to item table
to inventory
to UI

resource ID
to loader
to asset
to renderer

Track:

- source
- transformations
- consumers
- storage
- serialization

## 9. Control-Flow Analysis

When behavior is unclear inspect:

- conditional branches
- loops
- switch statements
- exception handlers
- early returns
- state machines
- callbacks

Determine what conditions control important behavior.

Do not infer behavior from a single branch without understanding how its inputs are produced.

## 10. String Analysis

Extract and classify strings.

Search for:

- class names
- method names
- file paths
- asset names
- resource names
- URLs
- configuration keys
- debug messages
- error messages
- serialization identifiers
- engine identifiers
- feature names

Strings can provide valuable leads but are not proof of runtime behavior.

## 11. Reflection

Search for reflection APIs and dynamic invocation.

Investigate:

- class lookup
- method lookup
- field lookup
- dynamic object creation
- annotation processing
- string-based dispatch

Map dynamically referenced identifiers back to concrete classes and methods where possible.

## 12. Dynamic Loading

Investigate:

- class loaders
- native library loading
- plugin systems
- dynamically downloaded components
- runtime-generated resources
- dynamically generated code

Static analysis may miss behavior that only appears after loading.

## 13. Native Analysis

For native libraries inspect:

- architecture
- ELF headers
- sections
- symbols
- imports
- exports
- strings
- relocations
- dynamic dependencies
- JNI registration

Identify native subsystems where possible.

Useful mapping:

Java or Kotlin
to JNI
to native function
to native subsystem

## 14. Native Function Reconstruction

When symbols are missing:

Use:

- callers
- callees
- constants
- strings
- memory access patterns
- JNI registration
- imported APIs
- control flow
- runtime behavior

Do not assign an exact semantic meaning solely from an address or function shape.

Use labels such as:

probableTextureLoader

until evidence supports a stronger conclusion.

## 15. JNI Investigation

Look for:

- JNI_OnLoad
- RegisterNatives
- Java-style JNI exports
- native method declarations
- native callbacks
- Java/native object handles
- string conversion
- byte-array transfer

Map Java/Kotlin declarations to native implementations.

## 16. Binary Format Reconstruction

When encountering an unknown format:

1. collect multiple samples
2. compare file sizes
3. inspect magic bytes
4. inspect headers
5. locate repeated patterns
6. locate length fields
7. locate offsets
8. identify tables
9. inspect strings
10. inspect compression
11. inspect encryption indicators
12. locate the loader
13. trace parsing code
14. validate the proposed structure

Never define a format from a single unexplained observation.

## 17. Hex Analysis

For binary data inspect:

- offsets
- byte sequences
- integers
- floating-point candidates
- strings
- alignment
- repeated structures
- pointers or offsets
- length fields
- checksums
- magic values

When documenting a structure use explicit offsets.

Example:

offset 0x00:
magic

offset 0x04:
version

offset 0x08:
entry count

offset 0x0C:
table offset

Unknown fields remain unknown.

## 18. Entropy Analysis

Entropy can help distinguish:

- plaintext
- structured binary data
- compressed data
- encrypted data

High entropy alone does not prove encryption.

Combine entropy with:

- headers
- known compression signatures
- code references
- loading behavior
- repeated samples

## 19. Compression and Encryption

Distinguish:

Encoding:
Representation transformation.

Compression:
Size reduction.

Encryption:
Protection against direct interpretation.

Obfuscation:
Intentional difficulty of analysis.

Determine where transformation occurs.

If encrypted or compressed content becomes readable at runtime, locate the transformation stage and trace its inputs and outputs.

## 20. Asset Reverse Engineering

Investigate:

- image formats
- sprite sheets
- texture atlases
- tilemaps
- animation data
- audio
- fonts
- shaders
- materials
- models
- configuration tables
- serialized objects

For game assets investigate relationships between:

asset
to metadata
to loader
to runtime object
to renderer or gameplay system

## 21. Sprite and Animation Analysis

When analyzing sprites determine:

- frame dimensions
- frame ordering
- atlas coordinates
- animation timing
- animation states
- directional variants
- facing direction
- pivot
- origin
- collision metadata
- visibility metadata

Do not assume sprite orientation from image appearance alone.

Search for metadata and code that determine orientation.

## 22. World and Map Analysis

When analyzing a game world investigate:

- world representation
- tilemaps
- tile metadata
- chunks
- regions
- terrain
- structures
- object placement
- spawning
- persistence
- streaming
- boundaries

Trace:

world creation
to generation
to storage
to loading
to rendering

## 23. Procedural Generation

When procedural generation is suspected investigate:

- seed creation
- seed storage
- RNG initialization
- random-number calls
- coordinate inputs
- chunk coordinates
- generation stages
- object placement
- structure placement
- loot generation
- deterministic behavior

A useful model is:

Seed
to RNG
to generation stage
to random decisions
to generated data
to world representation

Determine which parts are deterministic and which depend on runtime state.

## 24. Game-System Reconstruction

Look for evidence of:

- input
- game loop
- rendering
- camera
- visibility
- collision
- physics
- AI
- inventory
- loot
- crafting
- health
- animation
- world generation
- save/load
- networking
- configuration

Identify implementation boundaries rather than only surface-level features.

## 25. Version Comparison

When multiple builds are available compare:

- file structure
- manifest
- resources
- DEX
- Smali
- native libraries
- assets
- configuration
- strings
- classes
- methods

Start with hashes and structural comparison.

Then perform deeper analysis on changed files.

Use version differences to locate:

- added functionality
- removed functionality
- changed algorithms
- changed assets
- changed configuration
- changed dependencies

## 26. Runtime Reverse Engineering

When runtime access exists, collect:

- Logcat
- stack traces
- application logs
- runtime-created files
- loaded libraries
- relevant system events
- observable state changes

Use controlled experiments.

Change one important variable at a time whenever possible.

Record:

Initial state
Input
Expected result
Actual result
Evidence
Conclusion

## 27. Static and Runtime Correlation

Combine:

static code
+
assets
+
resources
+
native code
+
runtime evidence

Example:

Static:
Method loads file X.

Asset:
File X contains structured data.

Runtime:
File X is loaded during world initialization.

Combined conclusion:
File X is strongly associated with world initialization.

The combined conclusion should still preserve the distinction between direct evidence and inference.

## 28. Reverse Engineering Reports

A useful report should contain:

- target information
- methodology
- technology identification
- architecture
- entry points
- important classes
- important methods
- native components
- assets
- binary formats
- subsystem relationships
- runtime observations
- evidence ledger
- unresolved questions

Include exact locations for important findings.

## 29. Reusable Analysis Scripts

When an investigation requires repeated manual operations:

1. identify the repeated task
2. define inputs
3. define outputs
4. implement a reusable script
5. validate it
6. document it
7. integrate it into the workflow

Reusable scripts belong under:

skills/project-engineering/scripts/

Scripts should:

- accept configurable paths
- avoid hard-coded environment assumptions
- produce predictable output
- fail clearly
- preserve intermediate results when useful

## 30. Self-Extension

If an unknown artifact cannot be analyzed with existing tooling:

- identify the missing capability
- determine what evidence is required
- create a focused analyzer
- validate it
- document it
- reuse it

Do not create large frameworks for a problem that can be solved with a small parser or analyzer.

## 31. Evidence Discipline

Use:

Observed:
Directly measured or reproduced.

Recovered:
Directly reconstructed from code or binary evidence.

Inferred:
Strong interpretation supported by multiple observations.

Hypothesis:
Plausible but not verified.

Never convert a hypothesis into a fact without new evidence.

## 32. Investigation Stopping Rule

Stop when:

- the requested question has sufficient evidence
- additional analysis is unlikely to change the conclusion
- required evidence is unavailable
- the next useful step requires a new artifact or experiment

Record unresolved questions instead of guessing.

## 33. Final Objective

The objective of reverse engineering is not simply to obtain readable code.

The objective is to reconstruct a reliable technical model of the target.

That model should explain:

- what exists
- how components connect
- how data moves
- what happens at runtime
- which conclusions are verified
- which conclusions are inferred
- what remains unknown
- how another engineer can reproduce the investigation
