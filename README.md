[![checks](https://github.com/martoc/workflow-container-image/actions/workflows/tag.yml/badge.svg?branch=main&event=push)](https://github.com/martoc/workflow-container-image/actions/workflows/tag.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# workflow-container-image

Reusable GitHub Actions workflows for building and publishing container images to Docker Hub and GCP Artifact Registry.

## Features

- Multi-platform builds (linux/arm64, linux/amd64)
- Docker Hub and GCP Artifact Registry support
- Automatic semantic versioning with `action-tag`
- GitHub release creation
- Git LFS support

## Quick Start

### Docker Hub

```yaml
jobs:
  build:
    permissions:
      contents: write
    uses: martoc/workflow-container-image/.github/workflows/default.yml@v0
    secrets: inherit
```

### GCP Artifact Registry

```yaml
jobs:
  build:
    permissions:
      contents: write
      id-token: write
    uses: martoc/workflow-container-image/.github/workflows/gcp.yml@v0
    with:
      region: europe-west2
      repository_name: containers
      gcp_project_id: my-project
      workload_identity_provider: ${{ vars.WIF_PROVIDER }}
      service_account: ${{ vars.SERVICE_ACCOUNT }}
    secrets: inherit
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Available Workflows

| Workflow | Registry | Description |
|----------|----------|-------------|
| `default.yml` | Docker Hub | Build and push to Docker Hub |
| `gcp.yml` | GCP Artifact Registry | Build and push to GCP |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
