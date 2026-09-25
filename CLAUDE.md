# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository builds and distributes community XCFrameworks for Realm Swift using GitHub Actions. There is no local build system — all builds run in CI via `workflow_dispatch`.

## Repository Structure

- `.github/workflows/build-release.yml` — the sole workflow; triggered manually with `tag` and `xcode-version` inputs
- `patches/` — git patch files applied to the upstream `realm/realm-swift` repo before building
- `patches/disable-codesigning.patch` — removes codesign requirement from Realm's Ruby release script

## How Builds Work

1. Workflow clones `realm/realm-swift` at the specified tag
2. Applies all `patches/*.patch` files via `git apply`
3. Runs `sh build.sh download-core` then `sh build.sh ios-swift`
4. Assembles XCFrameworks using `xcodebuild -create-xcframework`
5. Zips as `Realm@<xcode-version>.spm.zip` / `RealmSwift@<xcode-version>.spm.zip`
6. Creates or updates a GitHub Release tagged with the Realm tag (e.g. `v20.0.3`)

Multiple Xcode versions can be built for the same Realm tag — each run uploads additional zip files to the existing release.

## Adding or Updating Patches

To create a new patch against a Realm Swift tag:

```bash
git clone --branch <tag> https://github.com/realm/realm-swift.git
cd realm-swift
# make changes
git diff > ../patches/my-new-patch.patch
```

Test a patch locally before committing:

```bash
cd realm-swift
git apply --check ../patches/disable-codesigning.patch
```

Document new patches in `patches/README.md`.

## Updating Default Xcode Version

Change the `default` value for `xcode-version` input in `.github/workflows/build-release.yml` and update the `runs-on` runner if needed (currently `macos-27`). The macOS runner major version must match the Xcode major version (e.g. Xcode 27.x requires `macos-27`).
