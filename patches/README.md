# Patches

This directory contains patch files applied to Realm Swift during the build process.

## Current Patches

None. As of Realm Swift v20.0.4, upstream removed code signing from `scripts/create-release-package.rb`, so no patches are required.

## How Patches Are Applied

The GitHub Actions workflow automatically applies patches:

```bash
git apply --verbose patches/*.patch
```

If no `.patch` files exist, the loop is a no-op.

## Creating New Patches

If you need to modify Realm Swift source:

1. Clone and edit:
   ```bash
   git clone --branch v20.0.4 https://github.com/realm/realm-swift.git
   cd realm-swift
   # Make your changes
   ```

2. Generate patch:
   ```bash
   git diff > ../patches/my-new-patch.patch
   ```

3. Document it in this README

## Testing Patches

Test patch locally before committing:

```bash
cd realm-swift
git apply --check ../patches/my-patch.patch
```
