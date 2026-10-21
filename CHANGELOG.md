# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/) and this project adheres to
[Semantic Versioning](https://semver.org/).

## [Unreleased]

### Fixed

- A publish no longer re-signs RPMs already in the repository. Each
  signature carries a timestamp, so every publish changed every RPM under its
  old URL, and for the CDN's ten-minute cache dnf could fetch old bytes
  against new metadata and fail with "checksum doesn't match".

### Changed

- Added the per-repo constitution as a v1.18.0 manifest (Package Repository):
  products, formats, signing key, smoke-test matrix and the hourly schedule.
- Constitution validation pinned to v1.18.0 via `constitution.yml`.
- Dependabot auto-merges GitHub Actions minor and patch updates.
- Added `LICENSE` (AGPL-3.0-or-later).
