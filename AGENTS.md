# Tap instructions

This repository distributes Upmaru Homebrew formulae. Keep installable definitions under `Formula/`, use `main` for published formula history and `feature/*` branches for PR work. The Macus source repository owns its OpenSpec plan, implementation and Git Flow release lifecycle; do not initialize another OpenSpec project here.

Use Homebrew's formula DSL and `brew tap-new` workflow conventions. Pin GitHub Actions to full commit SHAs and keep default token permissions read-only. Bottle publication must remain manually dispatched for a reviewed PR head SHA. Do not add automatic publishing, merging or version bumps without explicit authorization.

Add a Macus formula only after real candidate source/bottle artifacts and checksums exist. Do not publish fake digests, moving source archives, local URLs, unverified bottle tags or signing/notarization claims. Keep the canonical Macus install logic in its source checkout; export the completed definition here.

Formula installation and tests must not activate launchd, boot a guest or create runtime state. Preserve user runtime and Incus data through upgrade, uninstall and failures. Actual package installation must use an explicitly selected test environment; physical guest acceptance is separate and opt-in.

Validate workflow YAML and Homebrew style/tap syntax. For formula changes, run relevant audit, source build, bottle install and `brew test` checks on the supported native toolchain. The current `xcode-27` ARM64 CI runner matches Macus's Swift 6.4 build prerequisite. Hosted CI success is not hardware acceptance.

Do not push, publish assets, merge PRs or dispatch the publication workflow unless the user has authorized that action. Leave generated bottles, source archives, runtime disks, credentials and build output out of Git.
