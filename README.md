# semver-skill

> Semantic Versioning 2.0.0 expertise, packaged as an installable agent Skill.

[![CI](https://github.com/h2nguyen/semver-skill/actions/workflows/ci.yml/badge.svg)](https://github.com/h2nguyen/semver-skill/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![SemVer 2.0.0](https://img.shields.io/badge/SemVer-2.0.0-brightgreen.svg)](https://semver.org/spec/v2.0.0.html)

A self-contained Skill that applies the official [SemVer 2.0.0](https://semver.org/spec/v2.0.0.html)
specification rigorously to real versioning questions — bump decisions
(MAJOR / MINOR / PATCH), validation, comparison, pre-release & build-metadata
handling, 0.y.z initial-development heuristics, and recovery from
misversioned releases. Ships with a deterministic Python CLI, the spec
verbatim as a reference, ecosystem notes (npm, Cargo, pip/PEP 440, Maven,
Go modules, Docker, Helm), and a test suite that exercises every clause of
§11 precedence.

## What it does

When loaded by an agent, the Skill answers questions like:

- *"Our public Python library is at 2.4.7. I added a new optional `tz`
  parameter to `parse_date()`. What version should we publish?"* — and
  returns a spec-grounded verdict with the relevant clause cited.
- *"Sort 1.0.0, 1.0.0-rc.1, 1.0.0-rc.11, 1.0.0-rc.2, 1.0.0-beta and explain
  rc.2 vs rc.11."* — using the bundled CLI and citing §11.4.1.
- *"We're at 0.7.3 and rewrote the public auth interface. Three production
  customers. What version?"* — applies the 0.y.z convention and recommends
  whether to graduate to 1.0.0 per the spec FAQ.

## Repository layout

```
semver-skill/
├── semver/                       The installable Skill (this is what gets packaged)
│   ├── SKILL.md                  Entry point: decision flow, triggers, pointers
│   ├── references/
│   │   ├── spec.md               SemVer 2.0.0 verbatim (CC BY 3.0)
│   │   ├── decision-guide.md     Worked MAJOR/MINOR/PATCH examples + recovery
│   │   ├── precedence.md         §11 comparison algorithm + traces
│   │   ├── prerelease.md         Pre-release & build-metadata patterns
│   │   └── ecosystem.md          npm / Cargo / pip / Maven / Go / Docker / Helm
│   ├── scripts/
│   │   ├── semver_tool.py        Deterministic CLI (validate/parse/compare/bump/sort)
│   │   └── README.md
│   └── tests/
│       └── test_semver_tool.py   Spec-rule coverage (every §11 case)
├── evals/
│   └── evals.json                Optional eval prompts for skill iteration
├── .github/workflows/ci.yml      Run the test suite on PR & push
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE                       MIT (spec.md retains CC BY 3.0)
└── README.md
```

## Quick start

### Use the CLI directly

The Skill bundles a zero-dependency Python CLI implementing SemVer 2.0.0
exactly. Useful even outside an agent context — for CI scripts, release
automation, manual verification:

```bash
python semver/scripts/semver_tool.py validate "1.0.0-alpha.beta+sha.abc"
python semver/scripts/semver_tool.py parse    "2.4.7-rc.2+build.91"
python semver/scripts/semver_tool.py compare  "1.0.0-rc.2" "1.0.0-rc.11"
python semver/scripts/semver_tool.py bump     "2.4.7" minor          # → 2.5.0
python semver/scripts/semver_tool.py bump     "1.0.0-rc.7" release    # → 1.0.0
python semver/scripts/semver_tool.py sort     1.0.0 1.0.0-rc.1 1.0.0-alpha 1.0.0-beta
```

See [`semver/scripts/README.md`](./semver/scripts/README.md) for the full
command reference.

### Install as an agent Skill

The `semver/` directory is a self-contained Skill conforming to the Skill
format (an `SKILL.md` with YAML frontmatter at the root, plus optional
`references/`, `scripts/`, `tests/` directories).

To package it into a distributable `.skill` bundle, use any tool that
follows the Skill packaging convention (zip of the skill folder).

### Run the tests

```bash
python -m unittest discover semver/tests/ -v
```

All 34 tests should pass. The canonical §11.4 precedence chain test
(`test_canonical_precedence_chain`) verifies the comparison algorithm
against the example chain from the spec:

```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta < 1.0.0-beta
            < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0
```

## Design principles

- **Spec-grounded, not vibes-grounded.** Every claim in the Skill points to
  a specific clause of [the official spec](https://semver.org/spec/v2.0.0.html).
  Edge cases that the spec doesn't directly cover are flagged as judgment
  calls with explicit reasoning.
- **Deterministic where possible.** Validation, parsing, comparison, bumping,
  and sorting all route through the Python CLI — never reasoned from intuition.
  The CLI uses the official regex from §11 and the precedence algorithm
  exactly as specified.
- **Progressive disclosure.** `SKILL.md` is intentionally lean (~220 lines).
  Deep material lives in `references/` and loads only when the relevant
  question domain comes up.
- **Honest about ecosystem divergences.** Cargo, npm, pip/PEP 440, Maven,
  and Go modules each implement *a dialect* of SemVer. The ecosystem
  reference documents where reality differs from the strict spec so advice
  matches the user's actual tooling.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). PRs welcome — especially:

- New ecosystem coverage (Conan, NuGet, Hex, OPAM, …)
- More worked examples in `references/decision-guide.md`
- Additional eval prompts in `evals/evals.json`
- Bug fixes in `semver_tool.py` with a regression test

Run the tests locally before opening a PR:

```bash
python -m unittest discover semver/tests/ -v
```

## License

This project is licensed under the [MIT License](./LICENSE).

`semver/references/spec.md` reproduces the Semantic Versioning 2.0.0
specification by Tom Preston-Werner, which is licensed under
[CC BY 3.0](https://creativecommons.org/licenses/by/3.0/). That file
retains its original license and attribution.

## Acknowledgments

- [Tom Preston-Werner](https://tom.preston-werner.com) for authoring the
  [SemVer specification](https://semver.org/).
- The community discussion at <https://github.com/semver/semver> for years
  of edge-case clarifications.
