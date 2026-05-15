# Contributing to semver-skill

Thanks for considering a contribution. This project aims to be a definitive,
spec-grounded reference for Semantic Versioning 2.0.0 packaged as an agent
Skill — accuracy matters more than feature volume.

## Ground rules

1. **Spec-grounded claims.** Anything stated about SemVer should cite a
   specific clause of <https://semver.org/spec/v2.0.0.html>. If the spec is
   silent or ambiguous, label the claim as a convention or judgment call.
2. **Verified ecosystem claims.** Cross-ecosystem comparisons (npm vs Cargo,
   pip vs PEP 440, etc.) need a link to an authoritative doc in the PR
   description. Reasoning from memory is not sufficient.
3. **Tests for every code change.** `semver/scripts/semver_tool.py` is
   covered by `semver/tests/test_semver_tool.py`. New behavior needs a new
   test; bug fixes need a regression test.
4. **Lean SKILL.md.** Keep `semver/SKILL.md` under ~500 lines. Detail goes
   in `semver/references/`; the entry point should remain a fast index.

## Setup

No dependencies — Python 3.10+ standard library only.

```bash
git clone https://github.com/h2nguyen/semver-skill.git
cd semver-skill
python -m unittest discover semver/tests/ -v
```

## How to contribute

### Reporting bugs

Open an issue with:

- A minimal reproduction (version string, command invoked, output observed).
- The expected output and the spec clause that supports it.
- Your Python version (`python --version`) and OS.

### Suggesting features

Open an issue tagged `enhancement`. Good candidates:

- Additional ecosystem coverage (Conan, NuGet, Hex, OPAM, Composer, …).
- Worked examples for under-documented edge cases.
- Improvements to the trigger description so the Skill activates more
  reliably on relevant queries.

Bad candidates (out of scope):

- Anything that diverges from SemVer 2.0.0 (e.g. adopting CalVer features).
- Auto-fixing of incorrect versions in user projects (the Skill advises;
  applying changes is the user's call).
- Network-using functionality in the CLI — it must stay offline and stdlib-only.

### Pull requests

1. Fork the repo and create a feature branch from `main`.
2. Make the change. Update tests. Update `CHANGELOG.md` under a new
   `## [Unreleased]` heading (Keep a Changelog format).
3. Run the test suite: `python -m unittest discover semver/tests/ -v`.
   All 34 baseline tests plus your new ones must pass.
4. Update relevant `references/` files if behavior or recommendations change.
5. Open the PR with:
   - A description of what changed and why.
   - Spec or doc links backing any new claims.
   - The output of the test run.

### Coding style

- Python: standard library only. PEP 8. Type hints on public functions.
  Docstrings on user-facing functions citing the relevant spec clause.
- Markdown: prose. Code fences with language hints. Tables for comparisons.
- Keep lines under ~80 characters where reasonable.

## Releases

This project follows its own subject matter — Semantic Versioning 2.0.0 — for
its own version numbers. See [CHANGELOG.md](./CHANGELOG.md).

## Code of conduct

This project adheres to the [Contributor Covenant Code of Conduct](./CODE_OF_CONDUCT.md).
By participating, you are expected to uphold it.

## License

By contributing, you agree that your contributions will be licensed under
the project's [MIT License](./LICENSE).
