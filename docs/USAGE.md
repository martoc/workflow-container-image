# Usage

## Overview

This repository provides reusable GitHub Actions workflows for building and publishing container images. The workflows support multi-platform builds (arm64, amd64) and multiple container registries.

## Available Workflows

### default.yml

Builds and pushes container images to Docker Hub.

**Inputs:**

| Input | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `lfs` | boolean | No | `false` | Enable Git LFS for checkout |
| `platforms` | string | No | `linux/arm64,linux/amd64` | Target platforms for build |

**Required Secrets:**

- `DOCKER_USERNAME` - Docker Hub username
- `DOCKER_PASSWORD` - Docker Hub password or access token

**Example:**

```yaml
name: Build Container

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    permissions:
      contents: write
    uses: martoc/workflow-container-image/.github/workflows/default.yml@v0
    secrets: inherit
```

### gcp.yml

Builds and pushes container images to GCP Artifact Registry.

**Inputs:**

| Input | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `region` | string | Yes | - | GCP region |
| `repository_name` | string | Yes | - | Artifact Registry repository name |
| `gcp_project_id` | string | Yes | - | GCP project ID |
| `workload_identity_provider` | string | Yes | - | Workload Identity Federation provider |
| `service_account` | string | Yes | - | GCP service account email |
| `platforms` | string | No | `linux/arm64,linux/amd64` | Target platforms for build |
| `lfs` | boolean | No | `false` | Enable Git LFS for checkout |

**Optional Secrets:**

- `go_github_username` - GitHub username for Go private modules
- `go_github_token` - GitHub token for Go private modules

**Example:**

```yaml
name: Build Container

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

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
      workload_identity_provider: projects/123/locations/global/workloadIdentityPools/pool/providers/provider
      service_account: sa@my-project.iam.gserviceaccount.com
    secrets: inherit
```

## What Each Workflow Does

1. **Checkout** - Clones the repository with tags
2. **Tag** - Calculates semantic version using `martoc/action-tag`
3. **Container Build** - Builds multi-platform images using `martoc/action-container-build`
4. **Release** - Creates GitHub release (non-PR only) using `martoc/action-release`
