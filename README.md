# CI/CD Github Actions and Workflows

Reusable GitHub Actions workflow for automated npm package deployment with built-in versioning and publishing.

## Features

- **Automated Publishing**: Seamlessly publish to npm registry
- **Version Management**: Integrated with Changesets for semantic versioning
- **Customizable**: Configurable build, test, and publish commands
- **Reusable**: Call from any repository with custom parameters
- **Secure**: Uses OIDC for secure token management
- **Provenance**: Verifiable supply-chain using [npm provenance](https://docs.npmjs.com/generating-provenance-statements)

## Quick Start

### Continuous Integration

```yaml
name: Pull Request (CI)
on:
  push:
    branches-ignore: [main] # feature branches only

jobs:
  ci:
    uses: amerani/cicd/.github/workflows/ci-npm.yml@main
    secrets: inherit
```

### Continuous Deployment

```yaml
name: Release (CD)
on:
  push:
    branches: [main] # main branch only

jobs:
  cd:
    uses: amerani/cicd/.github/workflows/cd-npm.yml@main
    with:
      node-version: 24
      build: 'npm run build:prod'
      test: 'npm run test:ci'
      publish: 'npm run release'
    secrets:
      GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Configuration

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `node-version` | ❌ | `24` | Node.js version to use |
| `build` | ❌ | `npm run build` | Build command |
| `test` | ❌ | `npm run test` | Test command |
| `publish` | ❌ | `npm run publish` | Publish command |

## Required Secrets
The following environment variables must be set in the consuming repository. 

- `GH_TOKEN`: GitHub token for repository access
- `NPM_TOKEN`: npm (granular) authentication token

## What It Does

1. **Setup**: Checks out code and configures Node.js
2. **Install**: Runs `npm ci` for clean dependency installation
3. **Build**: Executes your build command
4. **Test**: Runs your test suite
5. **Publish**: Uses Changesets to handle versioning and publishing
6. **Annotations**: Creates a git tag and Github release

---
