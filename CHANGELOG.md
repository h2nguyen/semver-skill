# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
