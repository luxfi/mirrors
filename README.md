# luxfi/mirrors

Image mirror from `ghcr.io/hanzoai/*` to `ghcr.io/luxfi/*`.

## Workflow

`mirror.yml` runs on `workflow_dispatch` and every 6h. For each image in the matrix, it pulls from `ghcr.io/hanzoai/{image}:latest` and pushes to `ghcr.io/luxfi/{image}:latest` using `skopeo copy --all` to preserve all architectures.

Runs on the self-hosted `lux-build-linux-amd64` runner in the luxfi org.

## Mirrored images

| Image | Source | Destination |
|-------|--------|-------------|
| billing | `ghcr.io/hanzoai/billing:latest` | `ghcr.io/luxfi/billing:latest` |
| commerce | `ghcr.io/hanzoai/commerce:latest` | `ghcr.io/luxfi/commerce:latest` |
| console | `ghcr.io/hanzoai/console:latest` | `ghcr.io/luxfi/console:latest` |
| cloud | `ghcr.io/hanzoai/cloud:latest` | `ghcr.io/luxfi/cloud:latest` |
| cloud-api | `ghcr.io/hanzoai/cloud-api:latest` | `ghcr.io/luxfi/cloud-api:latest` |
| ingress | `ghcr.io/hanzoai/ingress:latest` | `ghcr.io/luxfi/ingress:latest` |
| id | `ghcr.io/hanzoai/id:latest` | `ghcr.io/luxfi/id:latest` |
| insights | `ghcr.io/hanzoai/insights:latest` | `ghcr.io/luxfi/insights:latest` |
| iam | `ghcr.io/hanzoai/iam:latest` | `ghcr.io/luxfi/iam:latest` |
| kms | `ghcr.io/hanzoai/kms:latest` | `ghcr.io/luxfi/kms:latest` |

## Required secrets

Set on this repo (`luxfi/mirrors`) under Settings -> Secrets and variables -> Actions:

- `HANZOAI_PULL_TOKEN` -- PAT with `read:packages` scope on `hanzoai` org. Used to pull from `ghcr.io/hanzoai/*`.

The built-in `GITHUB_TOKEN` handles push to `ghcr.io/luxfi/*` (has `packages:write` via workflow permissions).

## Trigger manually

```bash
gh workflow run mirror.yml -R luxfi/mirrors
gh run watch -R luxfi/mirrors
```

## Verify

```bash
crane digest ghcr.io/luxfi/billing:latest
```
