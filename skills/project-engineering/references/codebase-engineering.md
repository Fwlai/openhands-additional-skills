# Codebase Engineering Reference

## Purpose

This reference defines a disciplined workflow for understanding, modifying, testing, debugging, and maintaining an unfamiliar software project.

The objective is to allow an agent to work on a project without repeatedly rediscovering its architecture or making unsupported assumptions.

## 1. Initial Repository Inspection

Before modifying a project, inspect:

- repository structure
- source directories
- build files
- dependency files
- configuration files
- scripts
- tests
- documentation
- generated files
- assets
- platform-specific code
- CI configuration
- Git history when useful

Identify the primary:

- programming languages
- frameworks
- build systems
- runtime platforms
- application entry points

Do not modify files during the initial inspection unless explicitly required.

## 2. Architecture Reconstruction

Build a working model of the project.

Identify:

- application entry points
- major modules
- important classes
- important functions
- data models
- services
- managers
- controllers
- rendering systems
- storage systems
- networking systems
- asset systems
- platform-specific layers

Trace dependencies between major systems.

Useful model:

Entry Point
to Application
to Subsystem
to Core Logic
to Data
to External Dependency

The model does not need to represent every file.

Prioritize systems relevant to the current task.

## 3. Codebase Search

Use layered search.

Start with:

- filenames
- directory names
- class names
- function names
- configuration keys
- resource names
- strings

Then search for:

- callers
- callees
- implementations
- interfaces
- inheritance
- references
- imports
- generated code

When a symbol is important, determine both where it is defined and where it is used.

## 4. Dependency Analysis

Identify:

- direct dependencies
- transitive dependencies
- version constraints
- platform-specific dependencies
- optional dependencies
- generated dependencies

Before changing a dependency:

1. identify why it exists
2. determine which code uses it
3. check compatibility
4. inspect build configuration
5. check for platform implications
6. build and test after the change

Do not upgrade dependencies merely because newer versions exist.

## 5. Configuration Analysis

Inspect:

- environment configuration
- build configuration
- runtime configuration
- feature flags
- platform configuration
- compiler options
- optimization settings
- packaging configuration

Distinguish configuration that affects:

- development
- testing
- release builds
- runtime behavior

Never expose secrets while reporting configuration.

## 6. Change Planning

For a non-trivial modification:

1. identify the requested behavior
2. identify the affected subsystem
3. locate the implementation
4. locate callers and dependencies
5. identify possible side effects
6. define the smallest viable change
7. identify tests required to validate it

Prefer minimal targeted changes over broad rewrites.

## 7. Large-Scale Refactoring

When refactoring:

- preserve externally visible behavior unless change is intentional
- maintain compatibility where required
- update all references
- avoid partial migrations
- keep intermediate states buildable when practical
- use automated search and replacement carefully
- review the complete diff

For large migrations, divide the work into stages.

Each stage should have a clear validation point.

## 8. Debugging Workflow

When a bug is reported:

1. reproduce it
2. capture the exact error
3. identify the first relevant failure
4. trace the execution path
5. inspect state and inputs
6. identify the root cause
7. implement the smallest appropriate fix
8. reproduce the original failure
9. verify the fix
10. run regression tests

Do not treat the final error message as automatically being the root cause.

## 9. Build Failure Analysis

When a build fails, classify the failure.

Possible categories:

- syntax
- compilation
- dependency resolution
- API incompatibility
- configuration
- resource processing
- packaging
- signing
- platform/toolchain
- generated code
- environment
- runtime

Find the earliest meaningful error rather than fixing only the final cascade error.

After fixing:

- rebuild
- inspect warnings
- verify affected functionality
- check for new failures

## 10. Test Strategy

Use the smallest relevant test scope first.

Typical order:

1. targeted unit test
2. affected module tests
3. integration tests
4. full test suite
5. build
6. runtime test

Tests should validate behavior, not merely code execution.

When fixing a bug, add a regression test when practical.

## 11. Android Projects

For Android projects inspect:

- Gradle configuration
- AndroidManifest
- application modules
- library modules
- resource directories
- assets
- native libraries
- SDK configuration
- ABI configuration
- build variants
- signing configuration
- APK packaging

When an APK is produced, verify:

- APK exists
- expected package name
- expected version
- expected ABI
- installation succeeds when a device is available
- application launches
- relevant functionality works

## 12. Termux and Mobile Development

Assume that mobile development environments may have:

- limited RAM
- limited storage
- slower I/O
- interrupted processes
- limited background execution
- thermal constraints

Prefer:

- incremental builds
- targeted tests
- cached analysis
- smaller intermediate artifacts
- resumable operations
- controlled parallelism

Do not repeatedly perform expensive full-project operations when a targeted operation is sufficient.

## 13. Runtime Debugging

When runtime behavior is incorrect, collect:

- logs
- stack traces
- crash information
- relevant input
- configuration
- device information
- build version

For Android, correlate:

Logcat
to stack trace
to class
to method
to source code
to relevant state

When possible, reproduce the same issue after the fix.

## 14. Performance Analysis

When performance is relevant, identify:

- CPU-heavy operations
- memory-heavy operations
- allocations
- excessive I/O
- repeated computation
- rendering bottlenecks
- asset loading
- world generation
- background tasks
- synchronization

For mobile games pay particular attention to:

- frame time
- memory usage
- texture memory
- object counts
- draw calls
- chunk generation
- entity updates
- pathfinding
- collision checks
- save operations

Do not optimize based solely on intuition.

Measure before and after when practical.

## 15. Git Workflow

Before significant changes inspect:

- current branch
- working tree
- recent commits
- current diff

Keep changes logically grouped.

After changes:

- inspect diff
- inspect status
- verify intended files changed
- remove accidental artifacts
- commit meaningful units of work

Avoid mixing unrelated changes into the same commit.

## 16. Rollback

Before risky changes establish a recoverable state.

Possible methods:

- Git commit
- Git branch
- patch
- backup copy

For destructive or large migrations, ensure rollback is possible before proceeding.

## 17. Code Review

Review changes for:

- correctness
- unintended behavior
- broken dependencies
- missing error handling
- resource leaks
- concurrency issues
- performance regressions
- security problems
- compatibility problems
- test coverage
- unnecessary complexity

Review the complete diff rather than only the edited lines.

## 18. Generated Code and Build Artifacts

Distinguish source files from generated files.

Do not manually edit generated output unless the project explicitly requires it.

Identify:

- generated source
- generated resources
- build directories
- caches
- temporary analysis files
- packaged artifacts

Avoid committing generated artifacts unless the repository requires them.

## 19. Documentation

When discovering important architecture or behavior, document it.

Useful documentation includes:

- architecture notes
- subsystem descriptions
- dependency relationships
- build procedures
- debugging procedures
- reverse-engineering findings
- unresolved questions
- implementation decisions

Documentation should explain why something works, not merely what files exist.

## 20. Decision Preservation

Important technical decisions should preserve:

- problem
- options considered
- selected approach
- reason
- consequences
- limitations

This prevents future agents from repeating the same investigation.

## 21. Task Tracking

For complex work maintain explicit tasks.

Each task should contain:

- objective
- status
- dependencies
- affected files
- validation method
- unresolved issues

Use states such as:

- pending
- investigating
- implementing
- testing
- blocked
- complete

Do not mark a task complete merely because code was changed.

## 22. Self-Extension

If the project repeatedly requires an operation that does not have adequate tooling:

1. identify the repeated operation
2. define the desired input/output
3. create a reusable script or analyzer
4. test it
5. document it
6. integrate it into the workflow

Prefer reusable project tooling over repeated manual commands.

## 23. Evidence and Assumptions

Separate:

Observed:
Directly verified.

Recovered:
Reconstructed from source, binary, logs, or other artifacts.

Inferred:
Strong conclusion supported by multiple observations.

Hypothesis:
Plausible but unverified.

Do not present assumptions as facts.

## 24. Completion Criteria

A task is complete only when:

- requested behavior has been implemented
- affected code has been reviewed
- relevant tests have been run
- build status is known
- runtime behavior has been checked when applicable
- Git diff has been reviewed
- important limitations are documented

If validation cannot be performed, state exactly what could not be verified.

## 25. Final Report

For substantial tasks report:

- objective
- investigation performed
- implementation
- affected files
- important technical decisions
- tests performed
- build result
- runtime result
- remaining issues
- confidence in important conclusions
- recommended next step

The final report should allow another agent to continue the work without repeating completed investigation.
