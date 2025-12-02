# Dependabot Auto-Merge

A GitHub Action that automatically merges Dependabot pull requests based on semantic versioning update types.

## Features

- Automatically merges Dependabot PRs for patch, minor, or major updates
- Configurable update type threshold
- Works with `pull_request_target` event for security
- Simple and minimal implementation

## Usage

### Basic Setup

Add this workflow to your repository (`.github/workflows/dependabot-auto-merge.yml`):

```yaml
name: Dependabot Auto-Merge

on:
  pull_request_target:
    types:
      - opened
      - synchronize
      - reopened
      - ready_for_review

permissions:
  contents: write
  pull-requests: write

jobs:
  dependabot:
    runs-on: ubuntu-latest
    if: github.event.pull_request.user.login == 'dependabot[bot]'
    steps:
      - uses: your-username/dependabot-auto-merge@v1
        with:
          update-type: 'patch'  # Options: patch, minor, major
```

### Update Type Options

- `patch` (default): Auto-merge only patch version updates (e.g., 1.0.0 → 1.0.1)
- `minor`: Auto-merge minor and patch updates (e.g., 1.0.0 → 1.1.0 or 1.0.1)
- `major`: Auto-merge all updates including major versions (e.g., 1.0.0 → 2.0.0)

### Example: Auto-merge Minor and Patch Updates

```yaml
- uses: your-username/dependabot-auto-merge@v1
  with:
    update-type: 'minor'
```

## How It Works

1. The action fetches Dependabot metadata to determine the update type
2. It checks if the update type matches your configured threshold
3. If it matches, it automatically merges the PR using GitHub CLI

## Requirements

- GitHub Actions must have `contents: write` and `pull-requests: write` permissions
- The workflow must use `pull_request_target` event (not `pull_request`) for security reasons
- The PR must be created by `dependabot[bot]`

## License

MIT
