# Plan: add CHANGELOG.md, move the version to 2.6.0, update the README

**Date:** 2026-10-19. **Status:** done 2026-10-19 on branch `release/2.6.0`, tagged `v2.6.0` locally (not pushed).

## Decisions (user, 2026-10-19)

- Q1: tag from now on only: `v2.6.0`. No tags for older versions; the changelog names their commits instead.
- Q2: call the program "Seq-o-Matic" with no version number anywhere in the documents. So the README title,
  `requirements.txt`, `code/mainwindow.py`, `docs/architecture.md` and the setup guide lose "2.5" instead of
  getting "2.6". The README "Version History" section names no number either. The version is only in
  `pyproject.toml` and `CHANGELOG.md`.
- Q3: set 2.6.0 now; the changelog says storage.json still needs testing on Elm.
- Q4: no version in the window title.

Done as planned otherwise, plus `CHANGELOG.md` in `MANIFEST.in` and in the README file tree. Checks: no
"Seq-o-Matic 2.x" left outside the changelog; `tomllib` reads 2.6.0; all changelog hashes and paths exist;
`mainwindow.py` compiles; 80 unit tests pass in `c:\barseq_envs\seqomatic-dryrun`.
The draft below is kept as written; the final text is in `CHANGELOG.md` and `README.md`.

## Facts this plan is based on (checked 2026-10-19)

- `pyproject.toml` says `version = "2.5.0"`. The line was added in `dc55b5a` ("Commit to production level",
  2026-04-01) and has not changed since. There is no changelog.
- Tags: `v-pre-fork-merge` (`11c4505`, 2026-04-02), `pre-parity-main` (`6daf2a0`, 2026-10-05),
  `parity-elm-tested` (`8afb597`, 2026-10-06). None is a version tag.
- Everything on `main` after 2026-04-02 is new since 2.5.0: the reference-parity fixes, configurable z-stack
  thickness, storage.json settings and connection checks, the dry-run harness and tests, the setup guide, the README
  and docs updates, and the new repository name.
- Other places that say "2.5" for this program (not counting the Elm reference folder `Seq_o_matics_2.5`, which is
  a path and stays):
  - `README.md:1` title `# Seq-o-Matic 2.5`
  - `requirements.txt:1` `# Seq-o-Matic 2.5 -- Production Dependencies`
  - `code/mainwindow.py:2` docstring `Seq-o-Matic 2.5 -- Application entry point.`
  - `docs/architecture.md:1` title `# Seq-o-Matic 2.5 -- Architecture Guide`
  - `docs/setup_guide/Seq-o-matic_Setup_Guide.md:5` `Version 2026-10-18, for Seq-o-Matic 2.5.` (the manuscript
    ships a snapshot of this guide)
- The GUI window title is just "Seq-o-Matic" (`code/front_end/logwindow.py:94`), with no version.

## Assumptions (say if any is wrong)

- A1. 2.5.0 = `main` as of 2026-04-01/02 (`dc55b5a` to `11c4505`, the `v-pre-fork-merge` tag).
- A2. 2.6.0 = `main` today (`4e3b64b`), dated 2026-10-19 (the day the version is set).
- A3. Versions before 2.5.0 were never numbered in git. I list that history once, under "Before 2.5.0", by date,
  and do not invent version numbers for it.
- A4. The 2.6.0 entry says plainly that storage.json and the connection checks are on `main` but **not yet run on
  Elm**, and that the reference-parity fixes and the 250 um z-stack **were** tested on Elm (2026-10-06).
- A5. Format: [Keep a Changelog](https://keepachangelog.com) style, newest first, with Added / Changed / Fixed
  groups, an "Unreleased" section on top for future work, and short items that point to the plan or STATUS file
  for detail instead of repeating it. Commit hashes only where they help find the detail.

## Steps

1. Branch `release/2.6.0` from `main` (the rules say not to work on `main` directly).
2. Add `CHANGELOG.md` at the repository root. Draft below.
3. `pyproject.toml`: `version = "2.5.0"` -> `"2.6.0"`.
4. Change "2.5" to "2.6" in `README.md:1`, `requirements.txt:1`, `code/mainwindow.py:2`, `docs/architecture.md:1`
   (see Q2 for the setup guide).
5. README: add a "Version history" section (one short paragraph plus a link to `CHANGELOG.md`) and its entry in
   the Table of Contents. Under "For Developers", add a short "Releasing a new version" note: bump
   `pyproject.toml`, the title lines listed above, and add a `CHANGELOG.md` entry.
6. Sanity checks:
   - `grep` for "2.5" again; the only hits left should be the Elm folder path, numbers like `figsize=(2.5, 2.5)`,
     and the plan/inventory history files.
   - Parse `pyproject.toml` with `tomllib` and check the version reads `2.6.0`.
   - Check that every link and anchor added to the README resolves, and that the changelog's commit hashes
     exist (`git cat-file -e`).
   - Unit tests are not affected by docs and a version string, but I will run
     `python -m unittest discover -s dryrun/tests -t .` once to confirm the docstring edit in `mainwindow.py`
     changed nothing (if the env is available on this PC).
   - No dry run is needed: no behavior changes.
7. Show you the diff. No commit, tag or push until you say so.

## Questions

- **Q1. Tags.** Add `v2.6.0` on the release commit? Add `v2.5.0` afterwards on `dc55b5a` (or `11c4505`)?
  Pushing tags to `origin` is visible to everyone with access, so only on your go-ahead.
- **Q2. Setup guide.** Change "for Seq-o-Matic 2.5" to "2.6" and its date line to 2026-10-19? The manuscript
  ships a snapshot of this guide, so you may want to leave it as it is.
- **Q3. Is 2.6.0 a release before the Elm test of storage.json?** Option (a): call it 2.6.0 now, with the
  "not yet run on Elm" note (my recommendation, since that is the code people get from `main`). Option (b): keep
  the work under "Unreleased" until Elm passes, and set 2.6.0 then.
- **Q4. GUI.** Show the version in the window title ("Seq-o-Matic 2.6.0")? Off by default: it is a code change
  and was not asked for.

## Draft CHANGELOG.md

```markdown
# Changelog

All notable changes to Seq-o-Matic are recorded here, newest first. Version numbers are set in
`pyproject.toml`. Dates are YYYY-MM-DD.

## [Unreleased]

## [2.6.0] - 2026-10-19

The reference-parity work and the z-stack change were tested on Elm on 2026-10-06. The storage.json settings
and connection checks are on `main` but have **not yet been run on Elm**
(checklist: `plans/storage_settings_STATUS.md`).

### Added
- `config_file/storage.json`: AWS bucket and storage-server settings read from a file instead of the code, so
  another lab can set up its own storage. Create Experiment checks the ticked uploads (AWS and server) and blocks
  on errors, and says which box to untick (`plans/storage_settings_plan.md`).
- Configurable z-stack thickness and focus sweep in `scope.json`, with sub-stack max projection, a streaming
  NDTiff reader, a z-stack confirmation window and a disk-space estimate at Start. Elm ships with 250 um
  (`plans/zstack_thickness_plan.md`).
- Hardware-free dry-run harness (`dryrun/`) that runs the whole program with fake devices and compares two
  versions of the code, plus unit tests in `dryrun/tests`.
- Hardware installation and day-of setup guide, `docs/setup_guide/`.
- `environment.yml` lists the packages the vendored pycromanager and ndtiff need.

### Changed
- Behavior brought back in line with the version running on Elm (the reference), item by item; list and
  decisions in `plans/STATUS.md` and `docs/parity_inventory.md`. Approved differences from the reference:
  - Cancel stops the pump, skips the PBST flush and releases the pump, selector, relay and heater.
  - Imaging waits (tiling, max projection) honor Cancel.
  - The piezo parks at 200 um.
  - The pump-speed field pre-fills reliably and is validated.
  - The AWS upload skips a missing folder instead of failing.
- Elm's config and data files are now identical to the reference.
- Repository renamed to `BarseqLab/Seq_o_matic`.
- README and `docs/config_files.md` updated to match the current program.

### Fixed
- Fluidics completion events are signaled when a sequence finishes.
- Relay routing calls and the relay-group configuration restored.
- Heater error handler resets the stage first; the pump is set to 1.5 mL/min on connect.
- `log.txt` holds each line once.
- Protocol lookup uses the reference's matching order.

## [2.5.0] - 2026-04-01

First numbered version: the code reorganized under `code/`, with `pyproject.toml`, `setup.py`,
`requirements.txt`, `environment.yml` and a rewritten README. Tagged `v-pre-fork-merge` (2026-04-02).

## Before 2.5.0

Not numbered in git.
- 2025-09-30: heating stage and heater device fixes.
- 2024-08-11: initial commit.
```

## README additions (draft)

Table of Contents gets `- [Version History](#version-history)` before "For Developers". New section:

```markdown
## Version History

The current version is **2.6.0** (2026-10-19). [CHANGELOG.md](CHANGELOG.md) lists what changed in each version,
with dates.
```

Under "For Developers", new subsection "Releasing a New Version": set `version` in `pyproject.toml`; update the
version in the README title, `requirements.txt`, `code/mainwindow.py` and `docs/architecture.md`; move the
"Unreleased" items in `CHANGELOG.md` under the new version and date.
