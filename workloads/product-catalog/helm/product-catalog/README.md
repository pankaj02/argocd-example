# Product Catalog Helm Chart

## Environment-Specific Values

This chart uses a hierarchical values structure:

- `values.yaml` — base configuration (defaults to dev settings)
- `values-dev.yaml` — dev overrides (applied on top of `values.yaml`)
- `values-prod.yaml` — prod overrides (applied on top of `values.yaml`)

Helm merges values files in order. The base `values.yaml` is always loaded first. When an environment-specific file is supplied, its keys override the matching keys in `values.yaml` while all other values remain unchanged.

### Supplying Values via Argo CD

In the Argo CD `Application` manifest, use the `helm.valueFiles` field to select an environment:

```yaml
source:
  helm:
    valueFiles:
      - values-prod.yaml
```

If no `valueFiles` is specified, `values.yaml` (dev defaults) is used automatically.
