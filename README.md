# renovate-config

Contains account-wide Renovate bot presets. They can be referenced in other repositories by referencing them in
the `extends` array of the `renovate.json` configuration as follows:

## Default preset ([default.json](./default.json))

```json
{
  "extends": [
    "local>ThorstenSauter/renovate-config"
  ]
}
```

## Named presets, e.g. `foo.json`

```json
{
  "extends": [
    "local>ThorstenSauter/renovate-config:foo"
  ]
}
```
