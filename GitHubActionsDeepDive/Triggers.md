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
#### `workflow_dispatch` is a manual trigger for GitHub Actions workflows. It allows users to start a workflow directly from the GitHub UI or via the GitHub API.

### Providing Input to `workflow_dispatch`
You can define inputs for `workflow_dispatch`, which allows users to provide values when triggering the workflow.

#### Example:
    ```yaml
    on:
    workflow_dispatch:
        inputs:
        environment:
            description: "Environment to deploy to"
            required: true
            default: "staging"
            type: choice
            options:
            - staging
            - production
        debug:
            description: "Enable debug mode"
            required: false
            default: "false"
            type: boolean
    ```

#### Triggering via GitHub UI
1. Go to **Actions** tab in your repository.
2. Select the workflow that has `workflow_dispatch`.
3. Click **Run workflow** and provide the necessary inputs.

#### Triggering via GitHub CLI
    ```sh
    gh workflow run <workflow_name> --ref main --field environment=production --field debug=true
    ```

#### Triggering via GitHub API
    ```sh
    curl -X POST -H "Authorization: token YOUR_GITHUB_TOKEN" \
        -H "Accept: application/vnd.github.v3+json" \
        https://api.github.com/repos/OWNER/REPO/actions/workflows/WORKFLOW_FILE.yml/dispatches \
        -d '{"ref":"main","inputs":{"environment":"production","debug":"true"}}'
```


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

#### **How to Trigger the Custom Event**
To manually trigger this workflow, send a POST request to GitHub’s API using `curl`:
```bash
curl -X POST -H "Accept: application/vnd.github.everest-preview+json" \
     -H "Authorization: token YOUR_PERSONAL_ACCESS_TOKEN" \
     https://api.github.com/repos/OWNER/REPO/dispatches \
     -d '{"event_type": "custom-event-name", "client_payload": {"key": "value"}}'
```
- Replace `OWNER/REPO` with your repository details.
- `"custom-event-name"` must match the event type defined in the workflow.
- `client_payload` allows sending extra data that the workflow can use.

#### **Accessing the Payload in the Workflow**
Inside the workflow, you can access the payload using `${{ github.event.client_payload.key }}`.
Example:
```yaml
jobs:
  custom-job:
    runs-on: ubuntu-latest
    steps:
      - name: Read Payload
        run: echo "Received: ${{ github.event.client_payload.key }}"
```

### **Use Cases for Custom Events**
- **Triggering workflows from external CI/CD pipelines.**
- **Starting workflows from another GitHub Action.**
- **Automating deployment steps triggered by external services.**


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
- When using workflow_run to track other workflows in GitHub Actions, the types field allows you to specify when the workflow should trigger. The available types are:
  - `requested` – The workflow is about to start (it has been requested but hasn't started running yet).
  - `completed` – The workflow has finished running, regardless of success or failure.
  - `in_progress` – The workflow is currently running.

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
- If you're using **Jenkins with GitHub** and have integrated it with **GitHub Checks API**, then `check_run` and `status` events can be used to trigger workflows based on Jenkins' job status updates.


### **Example 1: Trigger a Workflow When a Jenkins Job Completes**
#### **Scenario**
You have a **Jenkins pipeline** that runs CI/CD jobs and reports results back to GitHub using commit statuses (`pending`, `success`, `failure`). You want a **GitHub Actions workflow** to run **only after Jenkins finishes its job**.

#### **How It Works**
1. **Jenkins runs a job for a pull request (PR).**  
2. **Jenkins updates the commit status (`status` event in GitHub).**  
3. **GitHub Actions triggers based on this status event.**

#### **Jenkins Configuration**
In your **Jenkinsfile**, you can set commit statuses using GitHub’s API:
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    // Set GitHub status to "pending" at the start
                    sh 'curl -X POST -H "Authorization: token YOUR_GITHUB_TOKEN" \
                        -d "{\\"state\\":\\"pending\\", \\"context\\":\\"Jenkins Build\\"}" \
                        https://api.github.com/repos/YOUR_ORG/YOUR_REPO/statuses/${GIT_COMMIT}'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def status = currentBuild.result == 'SUCCESS' ? 'success' : 'failure'
                    // Update GitHub with the final build status
                    sh 'curl -X POST -H "Authorization: token YOUR_GITHUB_TOKEN" \
                        -d "{\\"state\\":\\"' + status + '\\", \\"context\\":\\"Jenkins Build\\"}" \
                        https://api.github.com/repos/YOUR_ORG/YOUR_REPO/statuses/${GIT_COMMIT}'
                }
            }
        }
    }
}
```

#### **GitHub Actions Workflow to React to Jenkins Job**
```yaml
on:
  status:

jobs:
  after-jenkins:
    runs-on: ubuntu-latest
    steps:
      - name: Check if Jenkins Build Succeeded
        run: echo "Jenkins build completed, proceeding with GitHub Actions workflow!"
```

#### **What Happens?**
- **Jenkins builds the project** and sends a **pending status** to GitHub at the start.
- **When Jenkins finishes**, it sends a **success** or **failure** status.
- **GitHub Actions workflow triggers on `status`**, allowing it to take further steps like:
  - Deploying artifacts
  - Running security scans
  - Notifying teams via Slack

<br>

### **Example 2: Re-run GitHub Actions If Jenkins Job Is Re-triggered**
#### **Scenario**
- Jenkins provides a **re-run option** for failed jobs.
- If a user **manually re-runs Jenkins**, you want to **automatically re-trigger a GitHub Actions workflow**.

#### **GitHub Actions Workflow**
```yaml
on:
  check_run:
    types: [rerequested]

jobs:
  reprocess:
    runs-on: ubuntu-latest
    steps:
      - name: Re-run after Jenkins re-trigger
        run: echo "Jenkins job was re-triggered, running GitHub Actions workflow again!"
```
#### **What Happens?**
1. A Jenkins **build is manually restarted**.
2. Jenkins updates the **check run** with `rerequested`.
3. **GitHub Actions detects the event** and re-runs the workflow.

#### **When Would You Use This?**
- **CI/CD Pipelines:** If Jenkins handles builds but GitHub Actions deploys artifacts.  
- **Quality Gates:** Ensure Jenkins test results are validated before proceeding.  
- **Incident Response:** If a Jenkins failure occurs, auto-trigger a GitHub Actions remediation workflow.  
