# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- `bump` kinds `premajor` / `preminor` / `prepatch` in `semver_tool.py`:
  bump the core version (with §7/§8 resets), then start `-TAG.1` — the
  correct way to begin a pre-release when the current version is already
  published.
- `bump … prerelease` now warns on stderr when the result would not sort
  above the input (starting a pre-release on a released version per §11.3,
  or switching to an earlier pre-release lane per §11.4).
- `parse` output includes a `note` field when a leading `v` was stripped.
- Guidance on starting next-version pre-releases in `SKILL.md` and
  `references/prerelease.md`, and the spec-FAQ `0.1.0` starting point for
  initial development in `SKILL.md` and `references/decision-guide.md`.
- `docs/GAP_ANALYSIS.md` — gap analysis of this skill against the official
  [semver/semver](https://github.com/semver/semver) repository
  (master @ `f99d548`), documenting the findings resolved in this release.
- 14 new tests (48 total): strict `v`-prefix validation, malformed-shape
  rejection, build-metadata leading zeros, `premajor`/`preminor`/`prepatch`,
  release-on-final-version error, and downgrade-warning behavior.

### Changed

- `references/spec.md` is now a true verbatim copy of `semver.md` from the
  official repository (master branch), replacing the earlier paraphrase.
  This brings in the full Introduction, "Why Use Semantic Versioning?",
  and the complete FAQ, and removes a paraphrase error (the deprecation
  FAQ says at least one minor release *should* — not *must* — carry a
  deprecation before removal).
- `validate` is now strict per the spec FAQ: `v1.2.3` is rejected (exit 1)
  with a hint that the underlying SemVer is `1.2.3`. Other commands still
  strip a leading `v` for convenience.
- `bump VERSION release` on a version with no pre-release and no build
  metadata is now an error: re-releasing an identical version would
  violate rule 3 (immutability). Previously it silently echoed the input.

### Fixed

- The official validation regex is now correctly attributed to the spec's
  FAQ (it was previously cited as coming from §11) in the tool, its
  README, `SKILL.md`, and the repository README.
- Docstring typo in `_bump_prerelease`.

## [0.1.0] - 2026-05-15

### Added

- Initial release.
- `semver/SKILL.md` — entry point with the four-step decision flow, trigger
  description, bump table with mandatory resets, and pointers to deeper
  references.
- `semver/references/spec.md` — the SemVer 2.0.0 specification reproduced
  verbatim from <https://semver.org/spec/v2.0.0.html> (CC BY 3.0).
- `semver/references/decision-guide.md` — twelve worked MAJOR / MINOR / PATCH
  examples, 0.y.z initial-development heuristics, deprecation procedure, and
  recovery procedures for misversioned releases.
- `semver/references/precedence.md` — full §11 comparison algorithm with
  seven traced examples covering the most error-prone pre-release ordering
  cases.
- `semver/references/prerelease.md` — pre-release identifier patterns
  (alpha / beta / rc), build metadata, iteration within pre-release, and
  antipatterns.
- `semver/references/ecosystem.md` — practical notes on how npm, Cargo,
  pip/PEP 440, Maven, Go modules, Docker, and Helm implement (or diverge
  from) strict SemVer.
- `semver/scripts/semver_tool.py` — zero-dependency Python 3.10+ CLI
  implementing the official §11 algorithm. Subcommands: `validate`, `parse`,
  `compare`, `bump`, `sort`.
- `semver/tests/test_semver_tool.py` — 34-test suite covering every spec
  rule, including the canonical §11.4 precedence chain.
- `evals/evals.json` — three realistic eval prompts for iterative
  improvement of the Skill's trigger description and decision quality.
- GitHub Actions CI workflow running the test suite on Python 3.10 – 3.13.

[Unreleased]: https://github.com/h2nguyen/semver-skill/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/h2nguyen/semver-skill/releases/tag/v0.1.0
