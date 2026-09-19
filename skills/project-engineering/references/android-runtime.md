# Android Runtime Analysis and Validation

## Purpose

This reference defines how the agent should analyze, test, debug, and validate Android applications at runtime.

The goal is to connect static APK findings with observable runtime behavior and produce reproducible evidence rather than assumptions.

## Runtime Environment Discovery

Before runtime analysis, identify:

- connected Android devices
- emulators, if any
- Android version
- API level
- device architecture
- ABI
- screen resolution and density
- available storage
- available RAM
- application package name
- installed application version
- debug/release state when observable
- current process and activity state

Record the environment because runtime behavior can differ between devices, Android versions, and architectures.

## ADB and Device Control

Use Android Debug Bridge capabilities when available.

The agent should be able to:

- discover connected devices
- select a specific device
- inspect device properties
- install and reinstall APKs
- uninstall applications when appropriate
- launch and stop applications
- inspect package information
- inspect activities and running processes
- capture runtime output
- collect accessible runtime files

Do not destroy application data or modify persistent state unnecessarily.

Prefer reproducible commands and record important commands used during experiments.

## APK Installation Validation

After building or receiving an APK, verify:

1. APK exists and is readable.
2. APK fingerprint/hash is recorded.
3. Package name is identified.
4. Version code and version name are recorded.
5. APK architecture is compatible with the target device.
6. Signing state is understood.
7. Installation succeeds.
8. Application launches.
9. Initial startup behavior is recorded.
10. Relevant logs are captured.

If installation fails, investigate:

- invalid APK structure
- signature problems
- Android version incompatibility
- ABI incompatibility
- package conflicts
- version downgrade
- missing split APKs
- malformed resources
- manifest problems
- runtime restrictions
- storage limitations
- permission/security restrictions

Do not assume the cause from the installation error alone.

## Application Lifecycle

Observe important lifecycle transitions:

- application startup
- Activity creation
- Activity destruction
- Activity recreation
- task creation
- task switching
- process creation
- process termination
- background/foreground transitions
- configuration changes
- pause/resume behavior
- application restart

Correlate lifecycle events with observed behavior.

## Logcat Analysis

Collect logs during controlled experiments.

Analyze:

- fatal exceptions
- Java/Kotlin stack traces
- native crashes
- warnings
- resource failures
- permission failures
- missing files
- asset loading failures
- rendering errors
- OpenGL/Vulkan errors
- network errors
- JNI errors
- lifecycle errors
- memory pressure
- ANR-related messages

When possible, filter logs by package name, process ID, relevant tag, and experiment time window.

Preserve original relevant log output before interpreting it.

## Crash and ANR Analysis

For crashes, identify:

- exception type
- crashing thread
- stack trace
- application frame
- native frame
- triggering action
- preceding log events
- relevant APK component
- corresponding source, Smali, or native location when available

For ANRs, investigate:

- blocked main thread
- long-running operations
- synchronization
- I/O
- rendering
- network operations
- startup delays
- background task interactions

Never treat a nearby warning as the crash cause without evidence connecting the two.

## Native Crash Analysis

When native code is involved, correlate:

- crash signal
- native library
- instruction address
- architecture
- symbol information
- loaded library ranges
- native stack trace
- JNI entry points
- relevant Smali/native calls

If symbols are unavailable, preserve addresses and offsets rather than inventing function names.

Use static analysis to map runtime addresses back to native functions when sufficient information exists.

## Runtime File and Data Observation

When accessible, inspect runtime data relevant to the experiment:

- save files
- configuration files
- databases
- caches
- temporary files
- generated world data
- downloaded assets
- serialized objects
- logs
- preferences
- local network state

Compare files before and after controlled actions.

Use binary diffs where appropriate.

Record:

- file path
- file size
- hash
- modification time
- detected format
- structural changes

Do not assume a changed file is responsible for a behavior without correlation.

## Static and Runtime Correlation

Continuously connect runtime observations to static APK findings.

Examples:

- runtime log to class/method
- crash to native function
- Activity launch to manifest declaration
- resource loading to resource ID and asset
- file access to code path
- JNI call to native library
- rendering behavior to rendering-related code
- world generation behavior to generator, seed, or RNG code
- asset loading to asset/resource location

Every correlation must preserve the evidence supporting it.

## Controlled Experiments

Runtime investigation should use small, reproducible experiments.

Examples:

- launch application
- start a new game
- load an existing save
- move the character
- enter a structure
- collect an item
- change inventory state
- trigger an animation
- cross a world/chunk boundary
- restart the application
- reproduce a crash
- modify one controlled input
- repeat the same action with the same initial state

For each experiment record:

- initial state
- exact action
- expected observation
- actual observation
- logs
- changed files
- relevant static evidence
- conclusion
- confidence

Change one important variable at a time whenever practical.

## Determinism and Reproducibility

When investigating procedural or state-dependent systems, test whether behavior is reproducible.

Check:

- identical input
- identical starting state
- identical APK version
- identical device/runtime environment
- identical seed when known
- repeated execution

If identical conditions produce different results, investigate:

- random seeds
- time-based seeds
- device state
- persistent state
- network state
- asynchronous execution
- nondeterministic ordering

Do not claim deterministic generation without repeated evidence.

## Runtime Investigation of Game Systems

When analyzing games, runtime experiments may identify:

- game loop
- input handling
- rendering
- camera movement
- visibility
- collision
- character state
- animation
- inventory
- item spawning
- loot generation
- enemy AI
- world generation
- chunk loading
- procedural generation
- seed/RNG behavior
- save/load behavior
- map transitions
- networking
- local multiplayer behavior

Runtime observations should be correlated with static APK evidence whenever possible.

## Performance Observation

When tools permit, collect:

- CPU usage
- memory usage
- process memory
- frame timing
- startup time
- loading time
- storage usage
- battery impact when measurable
- garbage-collection activity
- rendering workload
- crashes caused by resource pressure

For mobile projects, pay particular attention to:

- memory growth
- texture and asset loading
- excessive object creation
- chunk/world streaming
- large allocations
- repeated file I/O
- background processing
- rendering workload

Performance conclusions should be based on measurements rather than visual impressions.

## Offline Runtime Testing

If the target application is intended to operate offline:

- disable network connectivity when appropriate
- repeat important experiments offline
- identify unexpected network dependencies
- identify network initialization failures
- distinguish required local services from remote services
- verify save/load behavior without connectivity

Do not assume that an application is offline-capable merely because it launches without an obvious connection.

## LAN and Local Multiplayer

When investigating local multiplayer or LAN functionality, identify:

- discovery mechanism
- transport protocol
- ports
- host/client roles
- connection initialization
- session state
- synchronization mechanism
- serialization format
- disconnect/reconnect behavior
- local network dependencies

For Wi-Fi Direct or similar mechanisms, distinguish Android-level connectivity from the application's actual multiplayer protocol.

Do not infer a multiplayer architecture solely from the presence of networking APIs.

## Runtime Instrumentation

When static analysis is insufficient, use controlled instrumentation or runtime observation where practical.

Potential goals include:

- tracing method execution
- observing function arguments
- observing return values
- recording file operations
- tracking resource loading
- tracing JNI calls
- monitoring network activity
- observing state transitions

Instrumentation must be targeted.

Avoid modifying unrelated application behavior during experiments.

## Version Comparison at Runtime

When multiple APK versions exist, repeat equivalent experiments across versions.

Compare:

- startup behavior
- logs
- crashes
- file changes
- performance
- resource loading
- game behavior
- network behavior
- world generation
- save compatibility

Combine runtime differences with static APK diffs to identify likely implementation changes.

## Evidence Classification

Runtime findings must use these classifications:

### Observed

Directly seen or measured during execution.

### Recovered

Directly extracted from runtime artifacts such as logs, files, traces, or dumps.

### Inferred

Strongly supported by multiple observations or static/runtime correlation.

### Hypothesis

A plausible explanation that has not yet been sufficiently validated.

Never present a hypothesis as an implementation fact.

## Runtime Evidence Record

Important runtime findings should contain:

    Experiment:
    APK:
    Version:
    Device:
    Android/API:
    Architecture:
    Initial State:
    Action:
    Observation:
    Evidence:
    Related Static Artifact:
    Classification:
    Confidence:
    Reproduction Steps:
    Notes:

For crashes, additionally record:

    Exception:
    Signal:
    Thread:
    Stack Trace:
    Native Library:
    Address/Offset:
    Likely Component:

## Automated Runtime Pipeline

When useful, build a repeatable pipeline:

    APK
     ↓
    Fingerprint
     ↓
    Install
     ↓
    Launch
     ↓
    Collect Logs
     ↓
    Run Controlled Actions
     ↓
    Collect Runtime Artifacts
     ↓
    Analyze Crashes and Behavior
     ↓
    Correlate With Static Analysis
     ↓
    Record Findings
     ↓
    Generate Report

The pipeline should preserve raw evidence separately from interpreted results.

## Build and Runtime Verification

After code changes:

1. Build the project.
2. Record build output.
3. Verify the generated APK.
4. Record APK fingerprint.
5. Install it.
6. Launch it.
7. Run relevant regression tests.
8. Collect logs.
9. Reproduce the affected behavior.
10. Verify unrelated critical behavior.
11. Compare results with the previous known-good version.

A successful build is not sufficient evidence that the change works.

## Termux and Mobile Constraints

When the development environment is Termux or another constrained Android environment:

- avoid assumptions about desktop-only tooling
- prefer command-line workflows
- keep analysis incremental
- avoid unnecessary temporary copies
- monitor storage usage
- monitor memory-heavy operations
- preserve intermediate artifacts when useful
- allow long-running analysis to resume
- design scripts to fail clearly
- make important operations reproducible

Do not introduce desktop-specific dependencies unless the environment actually supports them.

## Safe Runtime Modification

Before modifying an APK or runtime artifact:

- preserve the original artifact
- record hashes
- document the intended modification
- make the smallest practical change
- test installation
- test startup
- test the affected behavior
- retain a rollback copy

Never modify the only copy of an important artifact.

## Runtime Findings Database

Important observations should be persisted so later sessions do not repeat the same experiments.

Useful fields include:

- APK/version
- device/environment
- experiment
- observation
- artifact
- related code
- related asset
- classification
- confidence
- reproduction procedure
- unresolved questions
- last verification

Findings from different APK versions must remain distinguishable.

## Stopping Rule

Stop runtime investigation when:

- the requested behavior has sufficient evidence
- additional experiments are no longer producing useful information
- remaining uncertainty requires unavailable instrumentation or artifacts
- further testing risks modifying important state without sufficient benefit

Record unresolved questions instead of filling gaps with assumptions.

## Final Runtime Report

A runtime report should contain:

1. Environment
2. APK/version information
3. Experiments performed
4. Observed behavior
5. Recovered runtime artifacts
6. Static/runtime correlations
7. Crash or performance findings
8. Relevant logs
9. Evidence classification
10. Confidence
11. Reproduction steps
12. Unresolved questions
13. Recommended next investigation

The report must clearly separate facts, interpretations, and hypotheses.
