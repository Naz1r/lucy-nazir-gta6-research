# Research publication and safety rules

This public repository is for **synthetic, self-authored compatibility research** and reviewable reports.

## Do not publish

- Leaked or proprietary game executables, Xbox package DLLs, archives, textures, shaders, audio, save data, keys, or decryption information.
- Game-memory dumps, proprietary binary bytes, title-specific raw captures, private title identifiers, personal file paths, user credentials, tokens, or private logs.
- Unknown executable attachments or tools designed to bypass DRM, licence checks, or access controls.
- Third-party code without attribution and a compatible licence. In particular, assess GPL obligations **before** combining source or distributing binaries.
- Unredacted diagnostics containing IP addresses, account names, machine identifiers, file-system paths or personally identifying data.

## What can be contributed

- Self-authored synthetic fixtures, negative tests, validation methodology, code with a clear provenance and compatible licence.
- Public API/interface names, public documentation links, generic call-shape comparisons and aggregate results.
- Reproductions that state exact branch/commit, platform, GPU, driver, toolchain, test procedure and observed/expected outcomes.
- Clear separation of **measured**, **reported by another team**, **inferred**, and **unknown**.

## Validation

1. Inspect changed files manually; `.gitignore` **does not prevent force-adding a secret**.
2. Use a secrets scan and a content/provenance review before publication.
3. Keep raw source data and non-public logs local; publish only sanitized aggregate evidence.
4. Do not describe successful synthetic tests as proof of a working retail GTA VI build.
5. Never run untrusted binaries on the research workstation. A VM/sandbox reduces risk, but is not a guarantee.
6. Do not bypass system or platform safety restrictions.

For suspected issues in the published material, open a GitHub issue describing the concern **without attaching sensitive content**.
