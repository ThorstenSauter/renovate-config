# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Renovate [shareable config preset](https://docs.renovatebot.com/config-presets/) repository for the `ThorstenSauter` GitHub account. There is no application code, build, or test suite: the product is the JSON preset files at the repo root.

- `default.json` is the default preset, consumed by other repos as `"extends": ["local>ThorstenSauter/renovate-config"]`.
- Any additional root-level `foo.json` becomes a named preset, consumed as `local>ThorstenSauter/renovate-config:foo`.
- `.github/renovate.json` makes this repo dogfood its own default preset, so changes to `default.json` affect Renovate's behavior on this repo too, as well as on every consuming repo once merged to `main`.

## Validation

CI (`.github/workflows/renovate-config-validation.yml`) runs `rinchsan/renovate-config-validator` against `*.json` in the repo root on PRs and pushes to `main`. Note that this glob does not cover `.github/renovate.json`.

To validate locally before pushing:

```sh
npx --yes --package renovate@latest -- renovate-config-validator --strict default.json
```

## Structure of `default.json`

- `extends` pulls in the built-in presets (`config:recommended`, daily schedule, semantic commits, vulnerability alerts, no rate limiting).
- `packageRules` are evaluated in order, and later matching rules override earlier ones. The ordering matters for the release-age window: a catch-all rule (`matchPackageNames: ["*"]`) sets `minimumReleaseAge: "3 days"`, and the rules that follow clear it (`null`) for trusted publishers (the owner's own `ThorstenSauter/**` actions and `ghcr.io/thorstensauter/**` images; Microsoft/Azure/Aspire NuGet packages; Azure, Next.js, and React npm packages). Keep new trusted-publisher exemptions after the catch-all rule.
- The remaining rules group updates by ecosystem (Docker, GitHub Actions, npm, NuGet, Terraform/TFLint) and apply labels. Non-major npm/NuGet updates are grouped, and major updates get an extra `breaking` label.

## Conventions

- Commits and PR titles use Conventional Commits with a scope, e.g. `feat(config): ...` and `chore(config): ...`. Renovate's own PRs use `chore(deps): ...`.
- JSON uses 2-space indentation (see `.editorconfig`).
- GitHub Actions are pinned to full commit SHAs with a trailing `# vX.Y.Z` comment, which Renovate keeps updated.
