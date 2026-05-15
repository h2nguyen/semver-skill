# Semantic Versioning 2.0.0 (verbatim spec)

> Source: <https://semver.org/spec/v2.0.0.html>
> License: CC BY 3.0. Authored by Tom Preston-Werner.
> This file is the authoritative spec. When in doubt, this wins over any
> other text in this skill.

## Summary

Given a version number `MAJOR.MINOR.PATCH`, increment the:

1. MAJOR version when you make incompatible API changes
2. MINOR version when you add functionality in a backward compatible manner
3. PATCH version when you make backward compatible bug fixes

Additional labels for pre-release and build metadata are available as
extensions to the `MAJOR.MINOR.PATCH` format.

## Introduction (paraphrased)

In systems with many dependencies, releasing new package versions can become
a nightmare. If specifications are too tight, you get *version lock* (can't
upgrade without re-releasing every dependent). If too loose you get *version
promiscuity* (assuming compatibility with too many future versions). This
spec defines how version numbers are assigned and incremented so that the
number itself communicates the kind of change. A precise public API is a
prerequisite — the API may live in code or documentation, but it must exist.

## Specification rules

RFC 2119 keywords (MUST, MUST NOT, SHOULD, MAY, …) apply.

1. **Public API required.** Software using SemVer MUST declare a public API.
   It MAY live in code or documentation but SHOULD be precise and
   comprehensive.

2. **Form.** A normal version number MUST take the form `X.Y.Z` where X, Y, Z
   are non-negative integers with no leading zeroes. X = major, Y = minor,
   Z = patch. Each MUST increase numerically (`1.9.0 → 1.10.0 → 1.11.0`).

3. **Immutability.** Once a versioned package is released, its contents MUST
   NOT be modified. Any modifications MUST be released as a new version.

4. **0.y.z initial development.** Major version 0 (`0.y.z`) is for initial
   development. Anything MAY change at any time. The public API SHOULD NOT
   be considered stable.

5. **1.0.0 defines the public API.** After 1.0.0, version increments depend
   on this declared public API and how it changes.

6. **PATCH bump.** Patch version Z (`x.y.Z | x > 0`) MUST be incremented if
   ONLY backward-compatible bug fixes are introduced. A bug fix is an
   internal change that fixes incorrect behavior.

7. **MINOR bump.** Minor version Y (`x.Y.z | x > 0`) MUST be incremented if
   new, backward-compatible functionality is introduced to the public API.
   It MUST be incremented if any public API functionality is marked as
   deprecated. It MAY be incremented if substantial new functionality or
   improvements are introduced within the private code. It MAY include patch
   level changes. **Patch version MUST be reset to 0 when minor is
   incremented.**

8. **MAJOR bump.** Major version X (`X.y.z | X > 0`) MUST be incremented if
   any backward-incompatible changes are introduced to the public API. It
   MAY also include minor and patch level changes. **Patch and minor
   versions MUST be reset to 0 when major is incremented.**

9. **Pre-release.** A pre-release version MAY be denoted by appending a
   hyphen and a series of dot-separated identifiers immediately following
   the patch version. Identifiers MUST comprise only ASCII alphanumerics
   and hyphens `[0-9A-Za-z-]`. Identifiers MUST NOT be empty. Numeric
   identifiers MUST NOT include leading zeroes. Pre-release versions have
   lower precedence than the associated normal version. A pre-release
   indicates the version is unstable and might not satisfy the intended
   compatibility requirements. Examples: `1.0.0-alpha`, `1.0.0-alpha.1`,
   `1.0.0-0.3.7`, `1.0.0-x.7.z.92`, `1.0.0-x-y-z.--`.

10. **Build metadata.** Build metadata MAY be denoted by appending a `+`
    and a series of dot-separated identifiers immediately following the
    patch or pre-release version. Identifiers MUST comprise only ASCII
    alphanumerics and hyphens, MUST NOT be empty. **Build metadata MUST
    be ignored when determining version precedence.** Two versions
    differing only in build metadata have the same precedence. Examples:
    `1.0.0-alpha+001`, `1.0.0+20130313144700`, `1.0.0-beta+exp.sha.5114f85`,
    `1.0.0+21AF26D3----117B344092BD`.

11. **Precedence.**
    1. Calculated by separating into major, minor, patch, pre-release
       identifiers in that order (build metadata is ignored).
    2. Compared left-to-right; major/minor/patch compared numerically.
       Example: `1.0.0 < 2.0.0 < 2.1.0 < 2.1.1`.
    3. When major/minor/patch are equal, a pre-release version has lower
       precedence than a normal version: `1.0.0-alpha < 1.0.0`.
    4. With equal major/minor/patch, pre-release precedence is determined
       by comparing each dot-separated identifier left to right:
       1. Identifiers consisting of only digits are compared numerically.
       2. Identifiers with letters or hyphens are compared lexically in
          ASCII sort order.
       3. Numeric identifiers always have lower precedence than
          non-numeric identifiers.
       4. A larger set of pre-release fields has higher precedence than a
          smaller set, if all preceding identifiers are equal.
       Example: `1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-alpha.beta <
       1.0.0-beta < 1.0.0-beta.2 < 1.0.0-beta.11 < 1.0.0-rc.1 < 1.0.0`.

## BNF grammar

```
<valid semver> ::= <version core>
                 | <version core> "-" <pre-release>
                 | <version core> "+" <build>
                 | <version core> "-" <pre-release> "+" <build>

<version core> ::= <major> "." <minor> "." <patch>
<major>        ::= <numeric identifier>
<minor>        ::= <numeric identifier>
<patch>        ::= <numeric identifier>

<pre-release>  ::= <dot-separated pre-release identifiers>
<dot-separated pre-release identifiers> ::= <pre-release identifier>
        | <pre-release identifier> "." <dot-separated pre-release identifiers>

<build>        ::= <dot-separated build identifiers>
<dot-separated build identifiers> ::= <build identifier>
        | <build identifier> "." <dot-separated build identifiers>

<pre-release identifier> ::= <alphanumeric identifier> | <numeric identifier>
<build identifier>       ::= <alphanumeric identifier> | <digits>

<alphanumeric identifier> ::= <non-digit>
                            | <non-digit> <identifier characters>
                            | <identifier characters> <non-digit>
                            | <identifier characters> <non-digit> <identifier characters>

<numeric identifier>   ::= "0" | <positive digit> | <positive digit> <digits>
<identifier characters>::= <identifier character> | <identifier character> <identifier characters>
<identifier character> ::= <digit> | <non-digit>
<non-digit>            ::= <letter> | "-"
<digits>               ::= <digit> | <digit> <digits>
<digit>                ::= "0" | <positive digit>
<positive digit>       ::= "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
<letter>               ::= "A" .. "Z" | "a" .. "z"
```

## Official regex

ECMAScript / PCRE / Python / Go compatible, numbered capture groups
(cg1=major, cg2=minor, cg3=patch, cg4=prerelease, cg5=buildmetadata):

```
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

With named groups (PCRE / Python / Go only):

```
^(?P<major>0|[1-9]\d*)\.(?P<minor>0|[1-9]\d*)\.(?P<patch>0|[1-9]\d*)(?:-(?P<prerelease>(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+(?P<buildmetadata>[0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

## FAQ excerpts (load `references/decision-guide.md` for fuller treatment)

- **How do I know when to release 1.0.0?** If your software is being used in
  production, it should probably already be 1.0.0. If you have a stable API
  on which users depend, you should be 1.0.0. If you worry a lot about
  backward compatibility, you should probably already be 1.0.0.
- **Won't I end up at 42.0.0?** That's the point — incompatible changes are
  expensive, so you'll think before introducing them.
- **Accidental breaking change in a patch release?** Don't mutate releases.
  Ship a corrective release that restores compatibility. If the broken
  behavior is now load-bearing for users, consider a MAJOR.
- **Deprecation procedure**: (1) update docs, (2) ship a MINOR release with
  the `@deprecated` marker. At least one MINOR must contain the deprecation
  before the next MAJOR removes the symbol.
- **Is `v1.2.3` a SemVer?** No. The leading `v` is a convention (git tags),
  the SemVer is `1.2.3`.

## License

Creative Commons CC BY 3.0. Reproduced here for offline reference; canonical
source: <https://semver.org/spec/v2.0.0.html>.
