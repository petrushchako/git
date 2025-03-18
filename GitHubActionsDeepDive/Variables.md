# Commonly Used Variables in GitHub Actions

GitHub Actions provides built-in variables that can be used within workflows. These variables are available in the `${{ github }}`, `${{ runner }}`, `${{ secrets }}`, and other contexts.

<br>

## 1. **GitHub Context Variables (`github.*`)**
These variables provide metadata about the workflow run, repository, and event.
| Variable | Description |
|----------|-------------|
| `github.repository` | The full name of the repository (`owner/repo`) |
| `github.repository_owner` | The repository owner (`owner`) |
| `github.ref` | The branch or tag ref that triggered the workflow |
| `github.sha` | The commit SHA that triggered the workflow |
| `github.actor` | The username of the person who triggered the workflow |
| `github.event_name` | The event that triggered the workflow (`push`, `pull_request`, etc.) |
| `github.run_id` | The unique ID for the workflow run |
| `github.run_number` | The unique run number for the workflow |
| `github.workflow` | The name of the workflow |
| `github.action` | The name of the action currently running |
| `github.job` | The name of the job |
| `github.event_path` | The path to the event payload file |

<br>

## 2. **Runner Context Variables (`runner.*`)**
These variables provide information about the environment where the job is running.
| Variable | Description |
|----------|-------------|
| `runner.os` | The operating system of the runner (`Linux`, `Windows`, `macOS`) |
| `runner.arch` | The architecture of the runner (`X86`, `X64`, `ARM`) |
| `runner.name` | The name of the runner instance |
| `runner.temp` | The temporary directory on the runner |
| `runner.workspace` | The workspace directory where the repository is checked out |

<br>

## 3. **Job Context Variables (`job.*`)**
These variables provide information about the current job.
| Variable | Description |
|----------|-------------|
| `job.status` | The status of the current job (`success`, `failure`, `cancelled`) |

<br>

## 4. **Steps Context Variables (`steps.*`)**
These variables store the output of previous steps in a job.
| Variable | Description |
|----------|-------------|
| `steps.<step_id>.outputs.<output_name>` | Access outputs from a previous step |

<br>

## 5. **Secrets Context Variables (`secrets.*`)**
These variables store encrypted secrets.
| Variable | Description |
|----------|-------------|
| `secrets.GITHUB_TOKEN` | A built-in authentication token for GitHub API requests |
| `secrets.<secret_name>` | Any user-defined secret stored in the repository |

<br>

## 6. **Environment Context Variables (`env.*`)**
These variables allow access to environment variables.
| Variable | Description |
|----------|-------------|
| `env.<variable_name>` | Any custom environment variable defined in the workflow |

<br>

## 7. **Matrix Context Variables (`matrix.*`)**
These variables provide access to matrix job values.
| Variable | Description |
|----------|-------------|
| `matrix.<variable_name>` | Access matrix-defined variables |

<br>

## 8. **Strategy Context Variables (`strategy.*`)**
These variables provide access to the job's strategy settings.
| Variable | Description |
|----------|-------------|
| `strategy.job-index` | The index of the current job in a matrix |

<br><hr>

### **Example Usage**
```yaml
jobs:
  example-job:
    runs-on: ubuntu-latest
    steps:
      - name: Print GitHub Variables
        run: |
          echo "Repository: ${{ github.repository }}"
          echo "Triggered by: ${{ github.actor }}"
          echo "Running on: ${{ runner.os }}"
