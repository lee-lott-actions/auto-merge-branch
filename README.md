# GitHub Action

Description of what the GitHub Action does.

## Features
- Feature #1
- Feature #2
- Feature #3

## Inputs
| Name          | Description                                           | Required | Default |
|---------------|-------------------------------------------------------|----------|---------|
| `input-1`     | Description of input-1.                               | Yes      | N/A     |
| `input-2`     | Description of input-2.                               | Yes      | N/A     |
| `input-3`     | Description of input-3.                               | Yes      | N/A    |

## Outputs
| Name           | Description                                                   |
|----------------|---------------------------------------------------------------|
| `result`       | Result of the action ("success" or "failure").                |
| `error-message`| Error message if the action fails.                            |

## Usage
1. **Add the Action to Your Workflow**:# Auto Merge Branch Action

This GitHub Action automatically merges one branch into another branch in your repository using the GitHub REST API and a set of helper actions. It creates a pull request from the source branch to the target branch, comments on it, and then completes the merge—helpful for keeping long-lived branches in sync or automating promotion flows.

---

## Features

- Creates a pull request to merge changes from `source-branch` (head) into `target-branch` (base).
- Comments and approves the pull request automatically.
- Merges the pull request if all previous steps succeed.
- Provides clear status output (`success` or `failure`).
- Suitable for scheduled sync, release promotion, branch updating, and more.

---

## Inputs

| Name            | Description                                       | Required |
|-----------------|---------------------------------------------------|----------|
| `repo-name`     | The name of the repository.                       | Yes      |
| `org-name`      | The name of the GitHub organization.              | Yes      |
| `source-branch` | The branch you want to merge from (head).         | Yes      |
| `target-branch` | The branch you want to merge into (base).         | Yes      |
| `token`         | GitHub token with access to pull requests.        | Yes      |

---

## Outputs

| Name     | Description                                                                    |
|----------|--------------------------------------------------------------------------------|
| `result` | Result of the attempt to update PR status: `"success"` or `"failure"`          |

---

## Usage

**Example Workflow:**

```yaml
name: Auto Merge Release into Main

on:
  workflow_dispatch:
  schedule:
    - cron: '0 6 * * 1-5' # every weekday at 6am UTC

jobs:
  auto-merge-branches:
    runs-on: ubuntu-latest
    steps:
      - name: Auto Merge dev into main
        uses: lee-lott-actions/auto-merge-branch@v1
        with:
          repo-name: 'your-repo-name'
          org-name: 'your-org-name'
          source-branch: 'dev'
          target-branch: 'main'
          token: ${{ secrets.GITHUB_TOKEN }}
```

---

## How It Works

1. **Create Pull Request**: Opens a PR from `source-branch` to `target-branch` if one does not exist.
2. **Comment on PR**: Adds a comment approving the PR as part of synchronization.
3. **Merge PR**: Merges the PR using the standard "merge" method.
4. **Status Output**: Sets the result output (`success` or `failure`) and logs an explanatory message in the workflow run.

---

## Notes

- The action assumes that the `la-actions/create-pull-request`, `la-actions/merge-pull-request`, and `la-actions/set-pull-request-review-status` actions are available in your marketplace.
- The token provided must have permissions to create and merge pull requests, typically `repo` or `contents` scopes.
- If an error occurs at any stage, the result will be set as `failure`, and you can check the logs for details.

---

## Common Use Cases

- **Promoting code from one environment branch to another** (e.g., staging → main).
- **Keeping release branches up-to-date** with ongoing development.
- **Automated branch sync for hotfix or long-lived support branches**.

---

**Author:** Louisiana Office of Technology Services

---
