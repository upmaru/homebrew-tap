# Upmaru Homebrew Tap

Homebrew formulae maintained by Upmaru.

This repository is initialized for the Macus distribution work. No formula or bottle is published yet. The planned install command below becomes available after a verified `Formula/macus.rb` and its artifacts are added:

```sh
brew install upmaru/tap/macus
macus start
```

Macus targets Apple Silicon and macOS 15+. Prebuilt bottles will be offered only for verified platforms. `macus start` manages the per-user service and standard Incus client setup; package installation must not boot a VM or activate a service.

## Repository layout

- `Formula/`: installable formulae with real source and bottle checksums.
- `.github/workflows/tests.yml`: Homebrew setup, tap syntax and PR formula/bottle checks.
- `.github/workflows/publish.yml`: manually dispatched bottle publication for a reviewed PR commit.
- `.github/dependabot.yml`: weekly updates for pinned GitHub Actions.

The workflow structure follows Homebrew's `brew tap-new` templates. Formula checks use the Upmaru `xcode-27` ARM64 runner and selected Xcode 27 toolchain, matching Macus's Swift 6.4 build. The publishing job uses Ubuntu for Homebrew/GitHub metadata and artifact operations; it does not run Macus. Runner access must be available to this repository before CI can execute. Hosted CI does not establish physical guest acceptance.

## Development

Keep this checkout beside the [Macus source repository](https://github.com/upmaru/macus). Macus owns the packaging implementation and OpenSpec plan; this tap receives the completed formula and bottle metadata. Do not maintain a second independent copy of its install logic.

This tap uses `main` as its published formula branch. Work on `feature/*` branches and open pull requests against `main`. Macus retains its own Git Flow release process. There is no `develop` branch or OpenSpec root in this distribution repository.

Once a formula exists, use a dedicated package test environment for installation checks:

```sh
brew style upmaru/tap/macus
brew audit --strict upmaru/tap/macus
brew test upmaru/tap/macus
```

The source-build check requires the supported Swift toolchain:

```sh
brew install --build-from-source upmaru/tap/macus
```

Source installation is not proof that the prebuilt bottle works. Bottle acceptance must verify the actual poured executable, signature and virtualization entitlement. Refuse conflicts with an existing Macus installation; an isolated runtime state directory does not isolate machine-wide Homebrew packages.

## Publishing bottles

After the formula PR passes CI and its exact commit is reviewed, manually run **brew pr-pull** in GitHub Actions with the PR number and full 40-character reviewed head SHA. The workflow passes that SHA to Homebrew and refuses an invalid input. It uploads the tested bottles and pushes the resulting formula commits to `main`; dispatch it only when publication of that candidate is authorized. No label, push or schedule triggers publication.

Keep the repository's default Actions token permission read-only. The publication job requests its own write/provenance permissions. Configure branch protection or rulesets for `main` to fit this deliberate maintainer publication flow; do not silently disable protection to publish. Select the `test-bot` job as a required check after its first successful run.

Automated upstream version bumps are intentionally absent while Macus uses explicitly qualified development candidates. A source URL, source commit, bottle version and digest must refer to the same candidate. Do not publish placeholder hashes, mutable development-branch archives, local file URLs, unverified platform tags or production signing/notarization claims.

## Documentation

- [Homebrew tap setup and maintenance](https://docs.brew.sh/How-to-Create-and-Maintain-a-Tap)
- [Formula cookbook](https://docs.brew.sh/Formula-Cookbook)
- [Homebrew bottles](https://docs.brew.sh/Bottles)
