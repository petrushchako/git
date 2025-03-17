## GitHub Actions Workflow Triggers

GitHub Actions workflows can be triggered by different events. Below is a collection of the most commonly used triggers and some interesting combinations.

### 1. **Push and Pull Request Triggers**
```yaml
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
```
- Runs the workflow on `push` and `pull_request` events targeting the `main` branch.


<br><br><br>


### 2. **Schedule Trigger (Cron Jobs)**
```yaml
on:
  schedule:
    - cron: "0 0 * * 1"
```
- Runs the workflow every Monday at midnight UTC.


<br><br><br>


### 3. **Manual Trigger (Workflow Dispatch)**
```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Target environment'
        required: true
        default: 'staging'
```
- Allows manual triggering with an input parameter.


<br><br><br>


### 4. **Tag and Release Triggers**
```yaml
on:
  push:
    tags:
      - 'v*'
  release:
    types: [created]
```
- Runs the workflow when a new tag starting with `v` is pushed or a release is created.


<br><br><br>


### 5. **Issue and PR Events**
```yaml
on:
  issues:
    types: [opened, closed]
  pull_request:
    types: [labeled, unlabeled]
```
- Runs when an issue is opened or closed, or when a PR receives or loses a label.


<br><br><br>


### 6. **Repository Dispatch for External Triggers**
```yaml
on:
  repository_dispatch:
    types: [custom-event]
```
- Allows triggering workflows from external applications or repositories.


<br><br><br>


### 7. **Workflow Run Completion**
```yaml
on:
  workflow_run:
    workflows: ["Build and Test"]
    types:
      - completed
```
- Runs when the "Build and Test" workflow completes.


<br><br><br>


### 8. **Multiple Event Combinations**
```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    types: [opened, synchronize]
  schedule:
    - cron: "0 12 * * 5"
  workflow_dispatch:
```
- Triggers on push to `main` or `develop`, PR updates, every Friday at noon, or manually.


<br><br><br>


### 9. **Check Run and Status Events**
```yaml
on:
  check_run:
    types: [created, completed]
  status:
```
- Runs when a check run is created or completed, or a commit status changes.

### 10. **Auto-Trigger on PR Review**
```yaml
on:
  pull_request_review:
    types: [submitted]
```
- Runs when a review is submitted on a pull request.
