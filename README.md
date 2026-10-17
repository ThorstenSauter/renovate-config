# renovate-config

Contains account-wide Renovate bot presets. Reference them from the `extends` array of a repository's
`renovate.json` configuration as shown below.

## Default preset ([default.json](./default.json))

```json
{
  "extends": [
    "local>ThorstenSauter/renovate-config"
  ]
}
```

What it does:

- Builds on `config:best-practices` with a daily schedule, semantic commits, vulnerability alerts and no PR rate
  limiting. That includes pinning Docker images to digests, pinning npm devDependencies, weekly lock file
  maintenance and flagging abandoned packages on the Dependency Dashboard.
- Pins GitHub Actions to commit SHAs with a full-version comment (e.g. `# v1.2.3`).
- Waits 3 days (`minimumReleaseAge`) before proposing a new release, except for update types that carry no release
  timestamp (replacements, lock file maintenance, pins) and for trusted publishers:
  - own `ThorstenSauter/**` actions and `ghcr.io/thorstensauter/**` images
  - `Aspire.*`, `Azure.*` and `Microsoft.*` NuGet packages
  - `@azure/*`, Next.js and React npm packages
- Groups updates by ecosystem: Docker, GitHub Actions, non-major npm, non-major NuGet, and Terraform providers with
  TFLint plugins. Major updates still get their own PRs.
- Labels every PR `dependencies` plus an ecosystem label (`docker`, `github_actions`, `npm`, `.NET`, `terraform`), and
  adds `breaking` to major updates.

## Named presets, e.g. `foo.json`

```json
{
  "extends": [
    "local>ThorstenSauter/renovate-config:foo"
  ]
}
```
