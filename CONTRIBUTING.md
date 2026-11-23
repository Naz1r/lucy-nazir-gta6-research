# Contributing to Lucy × Nazir GTA 6 Research Lab

Thanks for helping improve **self-authored synthetic compatibility research**.

Before contributing, read [SECURITY.md](SECURITY.md), [SOURCE-RELEASE-V0.1.md](docs/SOURCE-RELEASE-V0.1.md) and [REPRODUCIBILITY.md](docs/REPRODUCIBILITY.md).

## We welcome

- Review of the [Leonida constant-buffer diagnostic specification](docs/LEONIDA-CB-DIAGNOSTIC-SPEC.md), especially missing observations or alternative explanations for zero values.
- Source-built, standalone synthetic fixtures for memory aliasing, descriptor ranges, GPU publication, or concurrency.
- Independently repeatable reports with exact source pins, build environment, expected and observed outcomes, and negative controls.
- Corrections of upstream citations and precise source-based ABI claims.

## Please do not submit

Proprietary game assets/executables, confidential console binaries or SDK redistributions, extracted shaders, memory dumps, raw game logs, credentials, bypass techniques, or source copied from another licence without attribution and compatible reuse conditions.

## Evidence categories

- **Measured here:** link a source-build fixture and local run manifest.
- **Reported by local Codex:** link the record and note if raw run data is unavailable publicly.
- **Upstream reported:** link the upstream author, exact revision and observation.
- **Hypothesis:** state what experiment would disprove it.

## How to propose a source release

Prepare a minimal set of reviewed source files and their build/test scripts. Include explicit provenance and confirm no non-public material is needed. Since no repository-wide licence has yet been selected, agree on contribution terms with the maintainer before sharing material intended for unrestricted third-party reuse.

Avoid uploading complete local workspaces or raw program-output folders.
