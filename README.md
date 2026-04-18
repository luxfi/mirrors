# luxfi/mirrors

Image mirror from `ghcr.io/hanzoai/*` to `ghcr.io/luxfi/*`.

## Workflow

`mirror.yml` runs on `workflow_dispatch` and every 6h. For each image in the matrix, it pulls from `ghcr.io/hanzoai/{image}:latest` and pushes to `ghcr.io/luxfi/{image}:latest` using `skopeo copy --all` to preserve all architectures.

Runs on the self-hosted `lux-build-linux-amd64` runner in the luxfi org.

## Mirrored images

| Source (hanzoai) | Destination (luxfi) |
|------------------|---------------------|
| `ghcr.io/hanzoai/billing:latest` | `ghcr.io/luxfi/billing:latest` |
| `ghcr.io/hanzoai/commerce:latest` | `ghcr.io/luxfi/commerce:latest` |
| `ghcr.io/hanzoai/console:latest` | `ghcr.io/luxfi/console:latest` |
| `ghcr.io/hanzoai/cloud-site:latest` | `ghcr.io/luxfi/cloud:latest` (frontend SPA) |
| `ghcr.io/hanzoai/cloud:latest` | `ghcr.io/luxfi/cloud-api:latest` (backend API) |
| `ghcr.io/hanzoai/ingress:latest` | `ghcr.io/luxfi/ingress:latest` |
| `ghcr.io/hanzoai/id:latest` | `ghcr.io/luxfi/id:latest` |
| `ghcr.io/hanzoai/insights:latest` | `ghcr.io/luxfi/insights:latest` |
| `ghcr.io/hanzoai/iam:latest` | `ghcr.io/luxfi/iam:latest` |
| `ghcr.io/hanzoai/kms:latest` | `ghcr.io/luxfi/kms:latest` |

## Required secrets

Set on this repo (`luxfi/mirrors`) under Settings -> Secrets and variables -> Actions:

- `HANZOAI_PULL_TOKEN` -- PAT owned by `hanzo-dev` with `read:packages` on `hanzoai` AND `write:packages` on `luxfi`. `hanzo-dev` is an org admin on both, so a single PAT handles both pull and push. (Docker credstore only supports one cred per registry host, so both operations must use the same credential.)

## Trigger manually

```bash
gh workflow run mirror.yml -R luxfi/mirrors
gh run watch -R luxfi/mirrors
```

## Verify

```bash
crane digest ghcr.io/luxfi/billing:latest
```
