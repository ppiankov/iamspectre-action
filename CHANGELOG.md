# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-07-25

### Added

- `fail-on-findings` input to optionally fail the step (not just warn) when IAMSpectre reports findings (WO-3)
- `report` output format, matching IAMSpectre's `--format report` Markdown deliverable (WO-4)

### Fixed

- Download step now detects the runner's OS and architecture (Linux/macOS/Windows on amd64/arm64) instead of always downloading the Linux binary; unsupported combinations now fail clearly instead of downloading a mismatched binary (WO-2)
- Windows `.zip` release assets are now extracted with PowerShell's `Expand-Archive` instead of an unverified `unzip` dependency (WO-5)
- README's "Propagates scan exit codes for CI/CD gating" claim corrected to describe the actual default-warn/opt-in-fail behavior

### Changed

- First tagged release; establishes `v1` as the major-version reference documented in the README
