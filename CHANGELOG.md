# Changelog

All notable changes to Seq-o-Matic are recorded here, newest first. The version number is set in `pyproject.toml`. Dates are YYYY-MM-DD.

Each version names the git commits it covers. Versions from 2.6.0 on also have a git tag (`v2.6.0`, ...). Older versions have no version tag, so they are identified by commit hash only.

## [Unreleased]

## [2.6.1] - 2026-10-24

**Commits:** everything after the tag `v2.6.0` up to the tag `v2.6.1`.

### Changed
- `scope.json` ships with a 30 um z-stack (was 250 um), still at 1.5 um steps: 20 imaging planes, one projection per tile, and a 60 um focus sweep of 40 planes at 1.5 um. This is the same stack as 2.5.0 and the code default. Set `z_stack_thickness_um` again for thick sections (see [Z-stack thickness keys](docs/config_files.md#z-stack-thickness-keys)).
- README links the syringe-pump version from the Zador lab and the Seq-o-Matic AI Assistant.

## [2.6.0] - 2026-10-19

**Commits:** everything after `6daf2a0` up to the tag `v2.6.0`.

**Testing:** the reference-parity fixes and the configurable z-stack were tested on Elm (water test passed 2026-10-06; 250 um z-stack after a 200 um wet run). The `storage.json` settings and connection checks have been unit-tested and dry-run on a development PC but **still need to be tested on Elm** (checklist: `plans/storage_settings_STATUS.md`).

### Added
- `code/config_file/storage.json`: the AWS bucket and storage-server settings are read from this file instead of the code, so another lab can use its own storage. When creating an experiment in automated mode, the program tests the ticked uploads (AWS and server) and blocks Create on errors, saying which box to untick. A bad `storage.json` blocks Create in both modes. See `plans/storage_settings_plan.md`.
- Configurable z-stack thickness and focus sweep, read from `scope.json`. Thick stacks are max-projected in sub-stack groups from a streaming NDTiff reader. Start shows a z-stack confirmation window and a disk-space estimate. Elm ships with a 250 um stack. See `plans/zstack_thickness_plan.md`.
- Hardware-free dry-run harness (`dryrun/`), which runs the whole program with fake devices and compares two versions of the code device by device, and unit tests in `dryrun/tests/`.
- Hardware installation and day-of setup guide, `docs/setup_guide/`.
- `CHANGELOG.md` (this file).

### Changed
These differ from the version running on Elm, by decision (see `plans/STATUS.md`):
- Cancel stops the pump, skips the PBST flush, and releases the pump, selector, relay and heater.
- Tiling and max-projection waits honor Cancel.
- The piezo parks at 200 um.
- The pump-speed field pre-fills reliably, is validated, and prompts when empty.
- The AWS upload skips a missing folder instead of failing.
- The GUI panel shows a few extra messages.

Other changes:
- Repository renamed to `BarseqLab/Seq_o_matic`.
- The documents call the program "Seq-o-Matic", without a version number. The version is in `pyproject.toml` and this file.
- `environment.yml` adds the packages that the vendored pycromanager and ndtiff need.
- README and `docs/config_files.md` updated to match the current program.

### Fixed
Behavior brought back in line with the version running on Elm, one commit per item (full list and decisions: `plans/STATUS.md` and `docs/parity_inventory.md`):
- Elm's config and data files are taken from the version running on Elm (`scope.json` was then changed for the 250 um z-stack).
- Fluidics completion events are signaled when a sequence finishes.
- Focus acquisition and plane selection (focus image path, sweep range, pass-2 plane choice).
- End-of-run uploads follow the upload checkboxes.
- The heater error handler resets the stage first.
- The pump is set to 1.5 mL/min on connect.
- Live view no longer requires an automated imaging step.
- Tissue-scanner stitching.
- Protocol lookup uses Elm's matching order.
- "Send note to server" catches any upload error.
- `log.txt` holds each line once.
- Relay routing calls and the relay-group configuration are restored.

## [2.5.0] - 2026-04-01

**Commits:** `dc55b5a` (2026-04-01, "Commit to production level"), which set version 2.5.0 in `pyproject.toml`, through `6daf2a0` (2026-10-05, tag `pre-parity-main`). The code under `code/` is the same in all of them; the later commits changed only `README.md`, `.gitignore`, `demo.png` and `plans/`. The tag `v-pre-fork-merge` (`11c4505`, 2026-04-02) is in this range.

- First numbered version. The program moved into `code/`, with `pyproject.toml`, `setup.py`, `MANIFEST.in`, `requirements.txt`, `environment.yml`, `docs/` and a rewritten README.

## Before 2.5.0 (no version number)

**Commits:** `4d9fe94` (2024-08-11, initial commit) through `f30ab9a` (2025-09-30). The program files were at the repository root, not in `code/`.

- 2025-09-30: heating stage and heater device fixes (`54d2fd0`, `f30ab9a`).
- 2024-09-09: README added.
- 2024-08-11: initial commit.
