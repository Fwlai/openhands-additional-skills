# APK Analysis Reference

## Purpose

This reference defines a repeatable workflow for deep static and runtime analysis of Android APKs.

The objective is to reconstruct technically supported knowledge about an APK's structure, code, native components, resources, assets, runtime behavior, and relationships between them.

Decompiler output is evidence, not necessarily the original source.

## 1. Preserve the Artifact

Never modify the original APK.

Create a separate analysis workspace containing:

- original APK
- SHA-256 hash
- file size
- analysis timestamp
- APK filename
- known version information
- extracted files
- decoded files
- analysis reports
- findings
- scripts used during analysis

The original APK must remain unchanged.

If multiple APKs exist, assign each a stable identifier.

## 2. Initial Fingerprint

Determine:

- package name
- version code
- version name
- minimum SDK
- target SDK
- compile SDK when recoverable
- signing information
- certificate fingerprints
- supported ABIs
- APK size
- DEX count
- native library count
- asset count
- resource count
- apparent engine or framework
- compression methods
- embedded archives

Record the source of every important value.

## 3. APK Structure

Inventory at least:

AndroidManifest.xml
resources.arsc
classes*.dex
assets/
res/
lib/
META-INF/

Also search for:

- secondary archives
- databases
- serialized data
- configuration files
- engine-specific containers
- custom binary formats
- encrypted or compressed blobs
- cache-like data
- dynamically loaded resources
- embedded native libraries

A missing conventional directory does not prove that the corresponding system does not exist.

## 4. Manifest Analysis

Analyze the application configuration and identify:

- application class
- label
- icon
- theme
- process
- debuggable state
- backup settings
- network security configuration
- native library configuration

Analyze all:

- Activities
- Services
- BroadcastReceivers
- ContentProviders

For each component record:

- exported state
- permissions
- intent filters
- launch modes
- process
- metadata

Search for:

- custom URI schemes
- HTTP and HTTPS links
- intent actions
- intent categories
- implicit broadcasts
- application entry points

## 5. Resource Analysis

Analyze:

- resources.arsc
- layouts
- drawables
- mipmaps
- strings
- colors
- dimensions
- styles
- XML resources
- raw resources
- resource IDs

Build relationships between resource IDs and code references.

Useful relationship:

resource ID
to resource definition
to code references
to calling classes and methods

Resource names can reveal architecture, subsystem names, feature names, internal terminology, or debugging clues.

Names are evidence, not proof.

## 6. DEX Analysis

Analyze every DEX file.

Recover:

- classes
- interfaces
- inheritance
- fields
- methods
- constructors
- annotations
- strings
- method signatures
- references
- package namespaces

Search for:

- application initialization
- lifecycle methods
- game loops
- rendering
- input
- storage
- networking
- asset loading
- configuration
- random-number generation
- serialization
- JNI
- logging

Build searchable indexes for large APKs.

## 7. Smali Analysis

Use Smali when decompiled Java or Kotlin is incomplete, misleading, or too abstract.

Inspect:

- registers
- method invocations
- field access
- branches
- constants
- object creation
- exception handlers
- reflection
- dynamic loading
- JNI calls

When recovering behavior, trace:

entry point
to caller
to method
to relevant branch
to data source
to output or side effect

Do not infer the meaning of a method from that method alone when surrounding call chains are available.

## 8. Obfuscation

Detect:

- meaningless class names
- meaningless method names
- shortened field names
- package flattening
- control-flow obfuscation
- string encryption
- reflection-heavy code
- dynamically generated names

When obfuscation exists, assign local semantic names in the analysis.

Do not pretend those names are the original names.

Example:

a.b.c.a()

can be documented as:

[Recovered] probable inventory initialization method

until stronger evidence is available.

## 9. Call Graphs

Build call relationships for important systems.

Prioritize:

- application startup
- game initialization
- world creation
- map loading
- save and load
- inventory
- loot
- AI
- rendering
- collision
- camera
- visibility
- networking
- random generation
- asset loading

Useful structure:

Entry Point
to Subsystem
to Controller
to Core Logic
to Data or Asset

## 10. Native Libraries

Inspect every relevant .so library.

Record:

- architecture
- ELF type
- section information
- imports
- exports
- symbols
- strings
- JNI functions
- dynamic dependencies

Search for:

- JNI_OnLoad
- registered native methods
- engine-specific entry points
- file-format parsers
- rendering systems
- physics systems
- encryption or decryption
- compression
- procedural generation

Native function names alone are not sufficient evidence of their purpose.

## 11. JNI Analysis

Map:

Java or Kotlin
to JNI declaration
to native method
to C or C++ implementation
to native subsystem

Look for:

- dynamically registered JNI methods
- static JNI naming
- native object handles
- native memory ownership
- callbacks into Java
- strings crossing the JNI boundary
- asset paths crossing the JNI boundary

JNI boundaries can help locate engine internals.

## 12. Asset Analysis

Inventory and classify:

- sprites
- sprite sheets
- texture atlases
- tiles
- tilemaps
- animation data
- fonts
- audio
- shaders
- materials
- models
- configuration tables
- databases
- serialized objects
- custom binary files

For image assets inspect:

- dimensions
- format
- color depth
- alpha channel
- atlas layout
- naming patterns

For sprite systems investigate:

- frame ordering
- animation metadata
- directional variants
- facing direction
- pivot or origin
- collision regions
- visibility-related metadata

## 13. Unknown File Formats

When a file format is unknown:

1. Inspect magic bytes.
2. Inspect headers.
3. Inspect strings.
4. Calculate entropy when useful.
5. Check compression signatures.
6. Check encryption indicators.
7. Search the codebase for references.
8. Locate the loader or parser.
9. Trace the parser's field accesses.
10. Create a parser only when enough structure has been established.

Document discovered binary structures using offsets and field meanings.

Never invent unknown fields.

## 14. Game-System Discovery

For game APKs actively investigate:

World:

- map representation
- tilemaps
- chunks
- regions
- world boundaries
- streaming
- persistence

Generation:

- seed creation
- RNG implementation
- random distributions
- terrain generation
- structure placement
- object spawning
- loot generation
- deterministic generation

Gameplay:

- inventory
- items
- crafting
- health
- status effects
- AI
- combat
- movement
- collision
- animation

Rendering:

- renderer
- camera
- sprite batching
- layers
- visibility
- culling
- lighting
- effects

Persistence:

- save format
- serialization
- databases
- checkpoints
- world persistence
- player persistence

## 15. Procedural Generation Investigation

When procedural generation is suspected, trace:

seed source
to RNG initialization
to generation stage
to random decisions
to generated data
to world representation
to runtime loading

Determine whether generation is:

- deterministic
- seed-based
- state-dependent
- chunk-based
- region-based
- runtime-only
- persisted

Record exact evidence for each conclusion.

## 16. Version Diffing

When multiple APK versions exist, compare:

- manifest
- resources
- DEX
- Smali
- native libraries
- assets
- strings
- classes
- methods
- configuration
- file structure

Use hashes for initial comparison.

For changed files perform deeper structural comparison.

Classify changes as:

- Added
- Removed
- Modified
- Renamed or likely renamed
- Unknown

Do not assume identical filenames represent identical semantics.

## 17. Runtime Analysis

When an emulator or Android device is available:

- install test APKs when appropriate
- collect Logcat
- collect crash traces
- collect stack traces
- observe runtime behavior
- observe runtime file creation
- inspect loaded libraries
- correlate runtime behavior with static analysis

Example:

Runtime crash
to stack trace
to class or method
to Smali or decompiled code
to resource or asset

## 18. Static and Runtime Evidence

Keep static evidence and runtime evidence separate until they can be correlated.

Use these classifications:

Observed:
Directly visible or measured.

Recovered:
Directly reconstructed from binary or source evidence.

Inferred:
Strong interpretation supported by multiple pieces of evidence.

Hypothesis:
Plausible but not sufficiently verified.

Never silently upgrade a hypothesis into a fact.

## 19. APK Findings Database

For large investigations maintain a machine-readable findings store.

Each finding should contain:

- ID
- classification
- confidence
- claim
- evidence
- source
- location
- related entities
- reproduction method
- status

Example finding:

ID: worldgen-001
Classification: Recovered
Confidence: 0.91
Claim: World generation initializes an RNG from a stored seed.
Evidence: class X, method Y, constant Z
Status: verified

Confidence describes evidence quality, not unsupported certainty.

## 20. Analysis Automation

For repeated investigations build an automated pipeline:

fingerprint
to extract
to decode
to inventory
to DEX analysis
to native analysis
to resource analysis
to asset analysis
to cross-reference
to runtime analysis
to report

The pipeline should be modular.

If one stage fails, preserve the results of completed stages.

Do not unnecessarily repeat expensive stages.

## 21. Tool Selection

Possible tools include:

- apktool
- JADX
- dex2jar
- baksmali and smali
- AAPT and AAPT2
- adb
- logcat
- ELF analysis utilities
- ZIP utilities
- binary and hex inspection tools

Choose tools according to the detected APK structure.

Do not blindly run every tool against every APK.

## 22. Creating New Analyzers

When existing tools cannot answer an important question:

1. Identify the missing capability.
2. Identify the required evidence.
3. Inspect the relevant format or code.
4. Implement the smallest useful analyzer.
5. Validate it against known data.
6. Document its output.
7. Integrate it into the workflow.

Reusable scripts belong under:

skills/project-engineering/scripts/

Keep analysis scripts deterministic and configurable.

## 23. Mobile and Termux Constraints

Account for:

- limited RAM
- limited storage
- CPU constraints
- large APKs
- large extracted directories
- long-running analysis
- process termination

Prefer:

- streaming
- incremental analysis
- caching
- targeted extraction
- indexed searches
- resumable workflows

Avoid unnecessary duplication of large artifacts.

## 24. Evidence Quality

Evidence priority generally follows:

1. direct runtime observation
2. directly recovered code or data
3. multiple independent static references
4. single static reference
5. naming conventions
6. visual similarity
7. speculation

Do not present weak evidence as strong evidence.

## 25. Final APK Report

A substantial investigation should be able to produce:

- APK Overview
- Architecture
- Manifest
- Resources
- DEX
- Smali
- Native Libraries
- Assets
- Unknown Formats
- Game Systems
- World Generation
- Runtime Behavior
- Version Differences
- Evidence Ledger
- Unresolved Questions
- Reproduction Steps

The final goal is not merely to decompile an APK.


The goal is to reconstruct reliable technical knowledge that can be used by the engineering workflow without repeatedly rediscovering the same information.
