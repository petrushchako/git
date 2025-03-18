# GitHub Action Deep Dive
Continuous integration, delivery, and deployment are quickly going from nice-to-haves to must-haves in any software development project. GitHub Actions is emerging as a fast-growing CI/CD solution — it's simple to get started, has strong integration with third-party tools and cloud providers, and has a robust, engaged community. This course will examine GitHub Actions’ approach to CI/CD — developing workflows, managing jobs, and securing resources. It will demonstrate how to leverage other GitHub services with Actions. It will go step-by-step through setting up a CI/CD workflow for an AWS Lambda function. Finally, learners will build their own GitHub Actions workflow to deploy a static site to an S3 bucket.

> [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)

<br>

> Demo project:<br>
> https://github.com/linuxacademy/content-github-actions-deep-dive-lesson

<br>

### Course Outline
- **CHAPTER 1**<br>**Introduction**
    - Course Introduction/About the Author
    - A Note about Pricing
- **CHAPTER 2**<br>**What Is GitHub Actions?**
    - Understanding Workflows, Jobs, and Actions
    - Introducing Community Actions
    - Getting Running with Runners
    - `Hands-on Lab` Setting Up a Custom GitHub Actions Runner
    - Securing GitHub Actions
    - Designing Workflows and Pipelines
- **CHAPTER 3**<br>**Building Your Workflow**
    - Introducing Your Microservice
    - Building Your Code
    - Storing Your Artifact
    - `Hands-on Lab` Creating a Release with GitHub Actions
    - Uploading to AWS
    - Deploying Your Function
- **CHAPTER 4**<br>**Enhancing Your Workflow**
    - Catching Errors Sooner: Code Quality Checks
    - Setting Up Non-Production Environments
    - Testing Before Production
    - Adding Documentation
    - `Hands-on Lab` Deploying Documentation to GitHub Pages
    - Reviewing the Workflow


<br><br><br>

## Introduction
## Course Introduction/About the Author
This course will provide a hands-on deep dive into GitHub Actions, focusing on practical workflow design and automation for modern applications.

### Key Topics Covered
- **Introduction to GitHub Actions**: Understanding what GitHub Actions are and how they fit into CI/CD pipelines.
- **Managing GitHub Actions Runners**: Exploring both GitHub-hosted runners and self-hosted runners.
- **Effective Workflow Design**: Best practices and principles for designing efficient and maintainable workflows.
- **Hands-On Workflow Example**: Creating a workflow to deploy an AWS Lambda function, from code to deployment.
- **Iterative Enhancements**: Adding checks, tests, and improvements to make the workflow robust and production-ready.

### About the Instructor
- **Instructor**: Wes Coffay (pronounced like "coffee")
- **Background**: Cloud Engineer with a focus on public sector projects.
- **Specialties**: 
    - Pipeline design
    - Automated cloud governance

### Why GitHub Actions?
GitHub Actions is a powerful automation tool that simplifies development workflows, improves deployment processes, and reduces pain points in application delivery. This course aims to equip you with the skills to make the most of GitHub Actions for your own projects.



<br><br><br>



## Pricing and Free Tier in GitHub Actions

### No Sandbox, But a Free Tier
Unlike AWS, GCP, or Azure, **GitHub Actions does not offer a dedicated sandbox environment**. However, GitHub provides a **free tier** (though they don’t officially call it that), which includes:
- **Free storage allowance**
- **Monthly workflow minutes allowance**

### Monthly Free Minutes
For **GitHub Free accounts**, you get:
- **2000 minutes per month** for running workflows.
- These minutes apply to **GitHub-hosted runners**.

### Different Runner Costs
Not all runner minutes count equally:
- **Linux Runners**: 1 minute = 1 minute from your allowance.
- **Windows Runners**: 1 minute = 2 minutes from your allowance.
- **Mac Runners**: 1 minute = 10 minutes from your allowance.

### Practical Note
- During course development, the instructor **never exceeded the 2000 free minutes**.
- Even if you re-run every workflow multiple times, it’s **very unlikely** you’ll hit the limit.

### No Credit Card Required
- **GitHub Free accounts do not require a credit card** to sign up.
- When you hit the free minute limit, **jobs simply stop running until the next month**.
- This removes the risk of unexpected bills, providing **peace of mind** compared to traditional cloud services.

### Key Takeaway
You can safely complete this course using a **GitHub Free account**, without worrying about surprise charges.



<br><br><br>



## Chapter 2<br>What Is GitHub Actions?

<br>

## Understanding Workflows, Jobs, and Actions
### What is GitHub Actions?
- **GitHub Actions** is a **Continuous Integration (CI)** solution.
- It **executes workflows triggered by events** (like pushes) to automate tasks such as building, testing, and deploying.
- It is a **managed service**, so GitHub handles the infrastructure (unless you use self-hosted runners).
- **Community Actions** are reusable solutions published by GitHub and third parties for common tasks (like checking out code, uploading artifacts, etc.).

### Key Terms
#### Workflow
- **A collection of jobs triggered by an event**.
- Conceptually, a **CI pipeline**.
- Defined in **YAML files**.

#### Runners
- **Compute machines where jobs execute**.
- Can be:
    - **GitHub-hosted** (managed by GitHub)
    - **Self-hosted** (fully managed by you)

#### Jobs
- **A set of steps that run on a single runner**.
- Workflows can have **one or more jobs**.
- Example: a workflow could contain **build**, **test**, and **deploy** jobs.

#### Steps
- **The smallest unit of execution in a job**.
- Steps can:
    - Execute a **command**.
    - Run a **script**.
    - Use a **JavaScript file**.
    - Use a **Docker container**.
    - Leverage a **community action**.

#### Example Workflow Structure
- Workflows start with a **name**.
- `on`: Specifies **triggers** (e.g., push to repository).
- Jobs have:
    - **Names**.
    - **Runners** (OS selection like `ubuntu-latest`).
    - **Steps** (each with optional names and actions/commands).
- Jobs can have **dependencies** using the `needs` key (e.g., a test job depends on the build job).

### Common CI/CD Terms
#### Continuous Integration (CI)
- In this course, **CI refers to any post-commit pipeline** that automatically executes after a code change.

#### Environment
- **A dedicated infrastructure stage** (e.g., dev, test, production).
- Each environment should **mirror the others as closely as possible**.

#### Gitflow
- **A development workflow** where:
    - Branches map to specific **environments**.
    - Example: 
        - `main` branch maps to **production**.
        - `dev` branch maps to **development**.
        - `release` branch maps to **testing**.

#### Typical Gitflow Lifecycle
1. **Fork main to create a development branch**.
2. Developers create **feature branches** from development.
3. Changes are committed and a **pull request** is made into the development branch.
4. After review and approval, the code moves to the **development environment**.
5. Once tested, code merges back to **main** for production.
6. New release branches fork from main for future versions.



<br><br><br>



## Introducing Community Actions
### What are Community Actions?
- **Prepackaged steps for GitHub Actions**.
- Provide **reusable templates** for common tasks.
- Created by:
    - **GitHub**.
    - **Third-party vendors**.
    - **Community members**.
- Often simplify **interactions with third-party tools or services**.
- **Open source** - free to use, fork, and customize.

### Finding Community Actions
- **GitHub Marketplace**:
    - Filter by **publisher, category, and keywords**.
    - Focus only on **actions** (not apps).
    - As of recording, **over 9,200 actions** available.
    - Filter for **Verified Creators** for additional confidence.
- **Google Search**:
    - Search “GitHub Actions” + **your use case** for strong results.
    - Can be a faster way to discover lesser-known actions.

### Exploring the GitHub Marketplace
- Marketplace contains **actions for various categories**, including:
    - **CI/CD**.
    - **Cloud platforms**.
    - **Container management (Docker)**.
- Each action’s page includes:
    - **Description**.
    - **Latest release info**.
    - **Sample usage examples**.

### Examples of Community Actions
#### 1. Checkout Action (by actions)
- **Open source** with multiple contributors.
- **Primary purpose**: Check out repository code into the runner.
- **Core action for most workflows**.

#### 2. Packer GitHub Action (by HashiCorp)
- **Simplifies Packer setup and execution in GitHub Actions**.
- Without this action, you would:
    - Download Packer.
    - Configure it on the runner.
    - Run the Packer commands.
- With this action, all setup is handled — you focus only on **managing your Packer code**.

#### 3. Jira Issue Action (by Atlassian)
- **Created by Atlassian** (adds credibility).
- **Simplifies creating Jira issues from GitHub Actions workflows**.
- Example of **integrating external services into your workflows**.



<br><br><br>



## Getting Running With Runners
### What is a Runner?
- A **runner** is a **virtual machine or compute instance** where jobs from your workflows **are executed**.
- Can be:
    - **GitHub-managed runners**.
    - **Custom runners** (managed by you).

### What is a Custom Runner?
- A **custom runner** is a VM in **your own environment** where jobs are run.
- Supported environments: **Linux, Windows, Mac**.
- Can run in:
    - **Cloud**.
    - **Data center**.
    - **Local workstation**.
- **Security responsibility is yours** when using custom runners.
- Analogy:
    - **Custom runner = AWS EC2 instance** (you manage the infrastructure).
    - **GitHub-managed runner = AWS Lambda function** (managed for you).


### Why Use a Custom Runner?
#### 1. **GitHub Enterprise (Self-Hosted GitHub)**
- In **GitHub Enterprise**, companies host their **own GitHub instance**.
- Code resides in **private infrastructure**, not github.com.
- Requires **custom runners** since private GitHub instances **cannot use GitHub-managed runners**.

#### 2. **Security Needs**
- Control **what compute instances have access** to sensitive secrets.
- If deploying to **production**, you may want **only your trusted compute instances** to handle deployments.
- Need **greater visibility and monitoring** than GitHub-managed runners offer.

#### 3. **Cost Management**
- GitHub Actions provides a **free tier**, but for:
    - **Large projects**.
    - **Long-running builds**.
    - Costs add up quickly.
- Cloud providers offer:
    - **Sustained use discounts**.
    - **Reserved instance pricing**.
- Using **existing cloud instances** can save money.

#### 4. **Ephemeral Runners for Cost Optimization**
- You can use a **GitHub-managed runner** to:
    - Spin up a **custom runner**.
    - Run long jobs on the **custom runner**.
    - **Tear down** the runner after the job completes.
- This **avoids GitHub billing** for the long-running job.



<br><br><br>



## Securing GitHub Actions
In this lesson, we explore key security considerations when using GitHub Actions, including:
- **Untrusted inputs**: Preventing malicious code injection.
- **Community actions**: Best practices for secure usage.
- **Custom runners**: Security risks and mitigation.
- **General security**: Best practices for repositories and workflows.

### Untrusted Inputs
- Workflows can be triggered by various events (e.g., pull requests, comments).
- Public contributions may introduce **malicious code**.
- Protect secrets and infrastructure when working with **public repositories**.
- Ensure workflows do not **automatically run** on untrusted PRs.
- Reference GitHub’s [untrusted inputs security guide](https://securitylab.github.com/resources/github-actions-untrusted-input/) for more details.

### Community Actions
- Treat **community actions** like any open-source tool:  
  **Trust, but verify** (or rather, don’t trust at all).  
- GitHub **organization settings** allow restricting action usage:
  - Approve **specific** actions or **trusted providers**.
  - Fork actions to maintain a **trusted state** before updating.

### Custom Runners Security
- **Risk**: Public repositories can trigger **untrusted** code execution on self-hosted runners.
- **Mitigation Strategies**:
  - **Use GitHub-hosted runners** for public repos.
  - Limit **runner permissions** based on the **principle of least privilege**.
  - Separate runners per **repository** to prevent cross-repo contamination.

### General GitHub Security Practices
- **Repository Visibility**:  
  - **Public** repos increase accessibility but expand the attack surface.
  - Keep repos **private** if public access is unnecessary.
- **Branch Protections**:  
  - Enforce **protected branches** (e.g., `main`, `develop`, release branches).
  - Prevent direct **pushes** or **merges** to key branches.
- **Pull Request Approvals**:  
  - Require multiple **reviewers** to prevent unauthorized changes.
  - Avoid direct deployment from PRs without **approval checks**.
- **Secrets Management**:  
  - Limit workflow access to **only necessary secrets**.
  - Store secrets securely using **GitHub Secrets Manager**.


<br><br><br>


## **Designing Workflows and Pipelines**
In this lesson, the focus is on understanding key concepts in designing workflows and pipelines. While this section is more theoretical, it builds the foundation for upcoming hands-on lessons.

#### **Key Concepts**
- **Best Practices**: 
  - Best practices are useful but should be adapted to your organization's context.
  - They often reflect the practices of high-performing organizations, like Google or Netflix, which may not align with your team's resources or needs.
  - Iteration is crucial — workflows should evolve like codebases, requiring updates and improvements over time.

- **CI Pyramid**: 
  - Best practices are the foundation.
  - Adding organizational context helps tailor these practices to your team's strengths and constraints.
  - Iteration helps refine workflows over time.

- **Adding Value with CI**: 
  - Focus on improving:
    - **Faster iteration** — enabling rapid experimentation and MVP development.
    - **Fewer human errors** — automation reduces mistakes from manual steps.
    - **Reduced toil** — repetitive manual tasks can be replaced with automated processes.

- **Managing Pipelines at Scale**: 
  - Large organizations may centralize CI ownership in a DevOps or shared services team.
  - Managing thousands of pipelines can be challenging; solutions include:
    - **Pipeline Templates**: Standardized templates (via YAML or private repos) for common frameworks like Java, Node, or .NET.
    - **Empowering SMEs**: Allowing subject matter experts in application teams to manage their own pipelines.

- **Pipeline Structure**: 
  - **Small Jobs**: Flexible, parallelized steps with isolated tools per job; easier to resume from failures.
  - **Large Jobs**: Minimized artifact passing and reduced configuration duplication; suitable for teams migrating from Bash/PowerShell scripts.

- **Branching Strategies**: 
  - Tailor branches to fit your release cycle and organizational needs.
  - Consider:
    - **Main/Dev branches** for simple workflows.
    - **Feature branches** for individual development.
    - **Release branches** for major deployments.
  - Protect key branches to avoid accidental direct deployments.



<br><br><br>



## Building Your Workflow
### Lesson Structure Overview
- **Repository**<br>In the next two sections, we will build out a workflow from our starter repo.
- **Branches**<br>There is a branch corresponding to the end state of each lesson in case of issues.
- **Follow Along**<br>You can follow along in a Cloud Sandbox (any AWS environment) or watch and take notes.

### The Microservice

```python
def lambda_handler (event, context):
    """Lambda function wrapper
    Args:
        event: trigger event dict
        context: lambda methods and properties
    Returns:
        string: greating response
    """
    print('Starting function\n---------------')

    if event["input"] == "Hello":
        return "World"
    else:
        raise
```

### Forking the Repository
Fork the repository to start adding your personal changes.
> [Repo for the lesson link](github.com/linuxacademy/content-github-actions-deep-dive-lesson)



<br><br><br>



## Building Your Code
- This lesson covers how to trigger workflows and set up workflow execution in GitHub Actions.
- Topics include:
  - Creating a workflow file
  - Workflow validation requirements
  - Building and compiling actions
  - Environment variables in workflows
  - Steps required to execute a build job
  - Updating the workflow

### **Workflow Triggers**
- Workflows in GitHub Actions can be triggered in multiple ways:
  - **Push**: Executes workflows when code is pushed to the repository.
  - **Pull Request**: Triggers workflows when a pull request is opened.
  - **Branch/File Filtering**: Limits execution to specific branches or files.
  - **Scheduled Execution**: Uses a CRON expression to run workflows at specific times.
  - **Other Events**: Comments, issues being opened, and more.

For this lesson, the **push** action is used as the trigger.

### **Creating a Workflow File**
- Workflows are stored in `.github/workflows/`
- A new workflow file `action.yaml` is created.
- **Workflow Name**: `"Deploy my Lambda Function"`
- **Trigger**: Runs on push to the `main` branch.

### **Building in Workflows**
- Many programming languages require building before execution.
- Python does not require building for AWS Lambda, but bundling libraries is useful for:
  - Reducing latency
  - Streamlining auditing
  - Running in private VPCs without internet access
- **Pip** (Python package manager) is used to install dependencies.

### **Environment Variables in Workflows**
- Available during workflow execution to provide metadata:
  - **Repository Info**: Name, URL
  - **Event Info**: Trigger type, user who initiated it
  - **Commit Details**
  - **Job and Runner Information**
- Accessed using `env.<VARIABLE_NAME>` syntax.

### **Build Job Steps**
1. **Check out the code**: Uses `actions/checkout` to fetch the repository.
2. **Set up Python**: Uses `actions/setup-python` with Python 3.8.
3. **Install Dependencies**:
   - Navigates to the `function` directory.
   - Updates Pip.
   - Installs dependencies from `requirements.txt` (if it exists) using:
     ```sh
     pip install -r requirements.txt -t .
     ```
4. **Create a Zip Bundle**:
   - Zips all files in `function/` into a package named after the commit SHA:
     ```sh
     zip -r ../lambda_function_${{ github.sha }}.zip .
     ```
5. **Archive the Artifact**:
   - Uses `actions/upload-artifact` to store the zip file for later workflow steps.

    ```yaml
    name: Deploy my Lambda Function

    on: 
      push:
        branch:
          - main

    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v2
          - name: Set up python
            uses: actions/setup-python@v2
            with:
              python-version: 3.8
          - name: Install libraries
            run: |
                cd function
                python -m pip install --upgrade pip
                if [ -f requirements.txt ]; then pip install -r requirements.txt -t .; fi
          - name: Create zip bundle
            run: |
                cd function
                zip -r ../${{ github.sha }}.zip .
          - name: Archive artifact
            uses: actions/upload-artifact@v2
            with:
              name: zipped-bundle
              path: ${{ github.sha }}.zip
    ```


### **Validating the Workflow Execution**
- The workflow successfully runs after committing `action.yaml` to `main`.
- In the **Actions Tab**, the execution log shows:
  - Code checkout
  - Python setup
  - Dependency installation
  - Zip creation
  - Artifact storage
- The artifact can be downloaded and inspected.

### **Final Cleanup**
- Deleted `init.yaml` (placeholder file) to prevent unnecessary workflow executions.

### **Summary**
- Created a workflow file in `.github/workflows/`
- Explored various GitHub Actions triggers
- Configured a **build job** with:
  - Code checkout
  - Python setup
  - Dependency installation
  - Zip packaging
  - Artifact storage
- Successfully executed the workflow and verified the results.

> If any issues occur, refer to the `lesson-build` branch in the repository.



<br><br><br>


## **Storing Artifacts in GitHub Actions**
Now that we've built our code and generated an artifact, let's explore what to do with it.

### **GitHub Packages Overview**
GitHub Packages is a service that allows you to store, manage, and distribute packages directly within GitHub. It supports:
- **Container images (Docker)**
- **Ruby Gems**
- **Node modules (npm)**
- **Maven and Gradle dependencies (Java)**
- **NuGet packages (.NET)**

For other artifact types, such as Python packages, GitHub Packages does not provide direct support. However, artifacts can be stored as **release assets** within a repository.

### **Publishing Artifacts in GitHub Packages**
To publish an artifact in GitHub Packages, you need to:
1. Build the package.
2. Authenticate the GitHub runner to GitHub Packages.
3. Push the package using an appropriate action.

For example, to push a **Docker image** to GitHub Container Registry (GHCR):
- Set the **registry name** to `GHCR`.
- Use the **GitHub token** for authentication.
- Use the **build and push** community action.

  ```yaml
  env:
    REGISTRY: ghcr.io
    IMAGE_NAME: ${{github.repository}}

  jobs:
    build-and-push-image:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Log in to the Container registry
        uses: docker/login-action
        with: 
          registry: ${{env.REGISTRY}}
          username: ${{github.actor}}
          password: ${{secrets.GITHUB_TOKEN}}

      - name: Build and push Docker image
        uses: docker/build-push-action
        with:
          content: .
          push: true
  ```

### **Why We Won’t Use GitHub Packages**
Our artifact is a **Python-based Lambda function packaged as a ZIP file**, which:
- **Isn’t a library** that would be imported into other projects.
- **Needs to be deployed via AWS Lambda**, which requires it to be stored in **S3**.

Instead of storing it as a release asset in GitHub, we will **upload it directly to S3**, skipping an unnecessary step.

> **Alternative Solutions**
> - Sonartype Nexus
> - Jfrog Artifactory
> - Cloud Storage Buckets (S3)


<br><br><br>


## **Uploading Artifacts to AWS from GitHub Actions**  
### **Key Topics**  
- Using **GitHub Secrets** for AWS credentials  
- Configuring the **AWS CLI** in a GitHub workflow  
- Adding an **upload job** to store the artifact in S3  

### **Using GitHub Secrets for AWS Access**  
GitHub Secrets are **encrypted environment variables** that help keep sensitive credentials secure. Secrets can be:  
- **Repository-level** (specific to a repo)  
- **Environment-level** (requires approval for access)  
- **Organization-level** (shared across multiple repos)  

To authenticate GitHub Actions with AWS:  
1. **Create an IAM user**:
   1. Navigate to AWS IAM
   2. User name: arbitrary name (e.g. `github_actions_user`)
   3. AWS Access Type: [x] Access type: `Programmatic access`
   4. Attach existing policies directly:
      - `AWSLambda_FullAccess`
      - `AmazonS3FullAccess`
    > Once you create a user, you will presented with `Access key ID` and `Secret access key`. **Copy those values for next step**.
2. **Store the AWS credentials** in GitHub as repository secrets:  
   1. In GitHub repository go to `Settings` and select `Secrets`
   2. In **Actions secrets** section, select `New repository secret`
   3. You will be presented with **key** and **value** input boxes (each secret created separately):
      ||Secret key|Secret value|
      |---|---|---|
      |1.|`AWS_ACCESS_KEY_ID`|<aws_iam_user_access_key_id>|
      |2.|`AWS_SECRET_ACCESS_KEY`|<aws_iam_user_secret_access_key>|

### **Workflow Steps for Uploading to S3**  
The workflow consists of **three steps**:  
1. **Download the artifact** from a previous build job.  
2. **Configure AWS credentials** using a pre-built GitHub Action.  
3. **Upload the artifact to S3** using the AWS CLI.  

#### **Example GitHub Actions Workflow (action.yaml)**  
  ```yaml
  . . .

  jobs:
    build:
      . . .

    upload:
      runs-on: ubuntu-latest
      needs: build
      steps:
        - name: Download artifact
          uses: actions/download-artifact@v3
          with:
            name: zipped-bundle

        - name: Configure AWS credentials
          uses: aws-actions/configure-aws-credentials@v2
          with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            aws-region: us-east-1

        - name: Upload to S3
          run: aws s3 cp github.sha.zip s3://my-bucket-name/
  ```

### Option 2: Do it yourself
  ```yaml
  - name: Download AWS CLI
    run: curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  - name: Unzip AWS CLI
    run: unzip awscliv2.zip
  - name: Install AWS CLI
    run: sudo ./aws/install
  - name: Configure AWS CLI
    run: |
        export AWS_ACCESS_KEY_ID=${{ secrets.AWS_ACCESS_KEY_ID }}
        export AWS_SECRET_ACCESS_KEY=${{ secrets.AWS_SECRET_ACCESS_KEY }}
        export AWS_DEFAULT_REGION=${{ AWS_REGION }}
  ```

### **Execution and Verification**  
1. **Commit the workflow file** in `.github/workflows/action.yaml`.  
2. **Trigger the workflow** by pushing a new commit.  
3. **Monitor execution in the GitHub Actions tab**.  
4. **Check the S3 bucket** in AWS to verify the uploaded artifact.  


<br><br><br>


## Enhancing Your Workflow

## Catching Errors Sooner: Code Quality Checks
### What is Static Code Analysis?
- Runs checks on raw/uncompiled code.
- Can detect:
  - Syntax errors (e.g., missing parentheses).
  - Style issues.
  - Version inconsistencies.
  - Security vulnerabilities.
- **Fails fast**: Identifies issues before build and deployment, saving time and cost.

### Adding a Linting Job to GitHub Actions
1. **Checkout Code**: Pulls repository content into the Runner.
2. **Set Up Python**: Configures Python on the Runner.
3. **Install Flake8**: A Python linting tool.
4. **Run Lint Checks**:
   - `flake8 . --select=E9,F63,F7,F82` (Errors that fail the workflow)
   - `flake8 . --exit-zero` (Warnings that don’t fail the workflow)

    ```yaml
    jobs:
      lint:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v2
          - name: Set up python
            uses: actions/setup-python@v2
            with:
              python-version: 3.8
          - name: Install linting libraries
            run: |
              cd function
              pip install flake8
          - name: Lint with flake8
            run: |
                flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics # Strict checks
                flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics # Info-only checks
    ```


#### Ensuring Lint Runs Before Build
- Defined a new job called `lint` in the workflow.
- Used `needs: lint` in the `build` job to enforce execution order.

### Results & Takeaways
- Linting job **failed**, preventing the build, upload, and deploy jobs from running.
- Error log pointed to the missing parenthesis in the function.
- After fixing and committing changes, the workflow passed, and the Lambda function worked correctly.



<br><br><br>



## Setting Up Non-Production Environments
In this section explores the limitations of static testing and compares it to functional testing. We then discuss setting up testing environments in GitHub Actions workflows and how to create ephemeral testing environments for dynamic testing.  

### Static vs. Functional Testing  
- **Static testing** evaluates code without executing it, checking syntax, formatting, and isolated issues. Quickly evaluates code with unit tests and syntax checks.
- **Functional testing** tests built or compiled code for expected outputs and performance issues.  
- Functional testing requires a dedicated environment, as testing in production can disrupt users, and testing in development can lead to conflicts.  

||**Static**|**Functional**|
|---|---|---|
|**Tests**|Raw code|Built or compiled code|
|**Detects**|**Unit tests**: Issues in isolation<br>**Linters**: Syntax and formatting errors|Deviations from expected output|
|**Resources**|Checks execute on runner|Needs non-prod/testing environment|


### Benefits of Ephemeral Testing Environments  
- **Data centers** require maintaining dedicated test environments.  
- **Cloud environments** allow ephemeral testing environments that are spun up on demand and destroyed after testing.  
- **Infrastructure as Code (IaC)** ensures consistency across environments.  

### Creating an Ephemeral Testing Environment  
1. **Configure AWS CLI** in GitHub Actions.  
2. **Create a test Lambda function** using the AWS CLI, sourcing code from an S3 bucket.  
3. **Insert a placeholder (sleep 30s)** to observe the test function in AWS.  
4. **Destroy the test function** using `aws lambda delete-function`.  

### Workflow Integration  
- The testing job runs **after the upload step** but **before the deploy step** to prevent deploying faulty code.  
- `if: always()` ensures cleanup even if a step fails, preventing leftover resources.  
- Update `needs` in the deploy job from `upload` to `test` to ensure the sequence is correct.  

  > **Verify AWS CLI installation**
  >   ```yaml
  >   - name: Check AWS CLI version
  >     run: aws --version
  >   ```

  ```yaml
  test:  
    runs-on: ubuntu-latest  
    needs: upload  
    steps:  
      - name: Configure AWS credentials  
        uses: aws-actions/configure-aws-credentials@v1  
        with:  
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
          aws-region: us-east-1  
      - name: Create test function  
        run: |  
            aws lambda create-function --function-name test-function \  
              --code S3Bucket=YOUR_S3_BUCKET,S3Key=${{ github.sha }}.zip \  
              --handler lambda_function.lambda_handler --runtime python3.8 \  
              --role arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/my-lambda-role  
      - name: Wait 30 seconds  
        run: sleep 30  
      - name: Destroy test function  
        if: ${{ always() }}  
        run: aws lambda delete-function --function-name test-function
  ```

#### Create a Test Lambda Function details
  ```yaml
  - name: Create test function
    run: |
        aws lambda create-function --function-name test-function \
          --code S3Bucket=YOUR_S3_BUCKET,S3Key=${{ github.sha }}.zip \
          --handler lambda_function.lambda_handler --runtime python3.8 \
          --role arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/my-lambda-role
  ```
  - Runs an AWS CLI command to create a Lambda function.
  - `--function-name test-function`: Names the Lambda function `test-function`.
  - `--code S3Bucket=YOUR_S3_BUCKET,S3Key=${{ github.sha }}.zip`:
    - `S3Bucket=YOUR_S3_BUCKET`: Specifies the S3 bucket containing the function code.
    - `S3Key=${{ github.sha }}.zip`: Fetches the code package named after the current commit SHA. This ensures the function uses the latest uploaded code.
  - `--handler lambda_function.lambda_handler`: Defines the entry point function inside the Lambda script.
  - `--runtime python3.8`: Specifies the Python 3.8 runtime for execution.
  - `--role arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/my-lambda-role`:
    - Assigns the IAM role needed for Lambda execution.
    - `AWS_ACCOUNT_ID` is stored as a secret for security and portability.



<br><br><br>


## Testing Before Production
### **Using Matrices in GitHub Actions**
- **Matrix** allows running multiple test cases with different values.
- Each test runs in **parallel** with a different matrix value.
- Example: A matrix for OS (`ubuntu`, `windows`, `macos`) and Python versions (`3.4`, `3.8`) would generate **8 jobs**.

    ```yaml
    - matrix:
      os: [ubuntu-18.04, ubuntu-20.04, Windows, MacOS]
      python: [3.4, 3.8]
    - runs-on: ${{ matrix.os }}
    - uses: actions/setup-python@v2
      with: 
        python-version: ${{ matrix.python }}
    ```
    ||**python 3.4**|**python 3.8**|
    |Ubuntu-18|Job-1|Job-5|
    |Ubuntu-20|Job-2|Job-6|
    |Windows|Job-3|Job-7|
    |MacOs|Job-4|Job-8|

### **Implementing Testing in the Workflow**
1. **Define a matrix under `strategy` in the test job**  
   - Example: Inputs `"Hello"` and `"Hi"` sent to the Lambda function.  
   
2. **Modify function names to avoid conflicts**  
   - Since multiple jobs create functions, unique names prevent namespace collisions.  

3. **Invoke the test function**  
   - AWS Lambda is invoked with a **test payload** matching the matrix input.
   - The expected output should not contain "error."

4. **Handle errors and debug issues**  
   - The first test failed because the function only handled `"Hello"`, returning `"World"`.  
   - Added logic to handle `"Hi"`, returning `"Hi There"`.  
   - Re-ran tests, and both cases passed successfully.

```yaml
  test:  
    runs-on: ubuntu-latest  
    needs: upload
    strategy:
      matrix: ["Hello", "Hi"]
    steps:  
      - name: Configure AWS credentials  
        uses: aws-actions/configure-aws-credentials@v1  
        with:  
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}  
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}  
          aws-region: us-east-1  
      - name: Create test function  
        run: |  
            aws lambda create-function --function-name test-function-${{ matrix.input }} \  
              --code S3Bucket=YOUR_S3_BUCKET,S3Key=${{ github.sha }}.zip \  
              --handler lambda_function.lambda_handler --runtime python3.8 \  
              --role arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/my-lambda-role
      - name: Invoke test function
        run: |
            aws lambda invoke --function-name test-function-${{ matrxi.input }} \
              --payload $(echo "{\"input\": \"$${{ matrix.input }}\"}" | base64) \
              --output json out
            if grep -q "Error" out; then
              exit 1
            fi
      - name: Wait 30 seconds  
        run: sleep 30  
      - name: Destroy test function  
        if: ${{ always() }}  
        run: aws lambda delete-function --function-name test-function-${{ matrix.input }}
  ```


### **Benefits of Using Matrices**
- **Scales testing efficiently** with minimal changes.
- **Easier to add new test cases** by modifying the matrix.
- **Supports multiple configurations** (e.g., OS, runtime versions).



<br><br><br>


## **Adding Documentation**
This lesson explores how **Continuous Integration (CI)** can do more than just building and deploying code. Specifically, it demonstrates how to automate **documentation deployment** using **GitHub Pages**.


### **CI Can Automate More Than Builds**
- CI/CD pipelines can automate tasks beyond building and deploying code.
- Examples of automation:
  - **Updating tickets** (e.g., closing JIRA tickets upon deployment).
  - **Triggering notifications** (e.g., sending emails or messages).
  - **Workflow analytics** (e.g., analyzing bottlenecks in execution time).
  - **Queue-based event triggers** (e.g., sending messages to a queue for further actions).

### **Introduction to GitHub Pages**
- **GitHub Pages** is a free hosting service for **static websites** (similar to AWS S3).
- It supports:
  - **Plain HTML**
  - **JavaScript frameworks**
  - **Static site generators** like **Jekyll** or **MkDocs**.
- Uses a specific branch (e.g., `gh-pages`) to serve static content.

#### **Use Cases for GitHub Pages**
- Hosting **project documentation** (e.g., with MkDocs or Sphinx).
- **Test result dashboards** (e.g., publishing HTML test reports).
- **Roadmaps & project status pages**.

### **Setting Up the Documentation Workflow**

```sh
repository(lesson-docs)
├── .github
│    └── workflows
│         └── action.yaml
├── docs
│   ├── about.md
│   └── index.md
├── README.md
└── mkdocs.yaml

```

1. **Checkout the Repository**
   - Clone the `lesson-docs` branch containing markdown files and YAML configurations.
   - `mkdocs.yaml`
      ```yaml
      site_name: My Docs
      nas:
        - Home: index.md
        - About: about.md
      theme: readthedocs
      ```


2. **Required Files for Documentation**
   - `mkdocs.yaml`: Configuration file defining documentation structure and themes.
   - `docs/`: Contains markdown (`.md`) files for content.
3. **GitHub Actions Workflow for Deployment**
   - **Runs on a Linux runner** after deployment is complete.
   - **Two steps:**
     - **Checkout the code**.
     - **Deploy Docs** using a **community action** (needs a config file & GitHub token).
   - `.github/workflows/actions.yaml`
  
      ```yaml
      docs:
        runs-on: ubuntu-latest
        needs: deploy
        steps:
          - name: Checkout code
            uses: actions/checkout@v2
          - name: Deploy docs
            uses: mhausenblas/mkdocs-deploy-gh-pages@master
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
              CONFIG_FILE: mkdocs.yaml
      ```

### **Merging & Deploying the Documentation**
1. **Merge `lesson-docs` branch** into `main`.
2. **Resolve merge conflicts** if necessary.
3. **Push the changes** to trigger the GitHub Actions workflow.
4. **Monitor the workflow execution** under the **Actions** tab.
5. **Configure GitHub Pages**:
   - Set `gh-pages` as the source branch.
   - Select **root** as the folder.
   - Wait for the website to be published.



<br><br><br>



## Reviewing the Workflow
### Key Takeaways
- **Overview of Achievements:**  
  - Built a workflow from a blank file to a robust CI/CD solution.
  - Implemented multiple levels of testing to ensure quality and prevent bad deploys.
  - Integrated automation beyond build and deployment.

### **1. Static Quality Checks**
- Implemented a **linter** to fail fast and catch issues early.
- Helps maintain **code hygiene** (e.g., enforcing formatting standards).
- Performs **dependency checks** to prevent insecure or outdated dependencies.

### **2. Ephemeral Testing Environments**
- **Infrastructure as Code (IaC)** ensures production-like environments for testing.
- Reduces inconsistencies between testing and production.
- **Cost-efficient:** Environments exist only when needed, avoiding ongoing cloud costs.

### **3. Parallel Testing with Matrices**
- Used **matrix strategies** to run test cases simultaneously.
- Increased **flexibility and scalability** of tests without affecting existing cases.
- Stacked matrices can test across **OS, language versions, container images, and cloud environments**.

### **4. Automating Documentation Updates**
- Documentation is integrated into the workflow using GitHub Actions.
- Reduces manual effort and ensures documentation stays up-to-date.
- GitHub Pages can be leveraged for public visibility.

### **Final Thoughts**
- **Fail fast** with static quality checks.
- **Ephemeral environments** provide cost savings and accurate testing.
- **Matrices enhance scalability** and test coverage.
- **Workflows go beyond CI/CD**—they can automate documentation and other development tasks.


    ```yaml
    name: Deploy my Lambda Function

    # trigger on all push events to main branch
    on: 
      push:
        branches:
          - main

    jobs:

      lint:
        runs-on: ubuntu-latest
        steps: 
          - name: Check out code
            uses: actions/checkout@v2
          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: 3.8
          - name: Install linting libraries
            run: |
                cd function
                pip install flake8
          - name: Lint with flake8
            run: |
                # Select identifies which errors should cause the job to fail
                flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
                # The exit zero flag will show errors as warnings and not fail the run
                flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics

      build:
        runs-on: ubuntu-latest
        needs: lint
        steps:
          - name: Check out code
            uses: actions/checkout@v2
          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: 3.8
          - name: Install libraries
            run: |
                cd function
                python -m pip install --upgrade pip
                if [ -f requirements.txt ]; then pip install -r requirements.txt -t .; fi
          - name: Create zip bundle
            run: |
                cd function
                zip -r ../${{ github.sha }}.zip .
          - name: Archive artifact
            uses: actions/upload-artifact@v2
            with:
              name: zipped-bundle
              path: ${{ github.sha }}.zip
      
      upload:
        runs-on: ubuntu-latest
        needs: build
        steps:
          - name: Download artifact
            uses: actions/download-artifact@v2
            with:
              name: zipped-bundle
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: us-east-1
          - name: Upload to S3
            run: aws s3 cp ${{ github.sha }}.zip s3://<YOUR BUCKET NAME HERE>/${{ github.repository }}/${{ github.sha }}.zip

      nonprod:
        runs-on: ubuntu-latest
        needs: upload
        strategy:
          matrix:
            input: ["Hello", "Hi"]
        steps:
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: us-east-1
          - name: Create test function
            run: |
                aws lambda create-function --function-name test-function-${{ matrix.input }} \
                  --code S3Bucket=<YOUR BUCKET NAME HERE>,S3Key=${{ github.repository }}/${{ github.sha }}.zip \
                  --handler lambda_function.lambda_handler --runtime python3.8 \
                  --role arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/<YOUR ROLE NAME HERE>
          - name: Invoke test function 
            run: |
                aws lambda invoke --function-name test-function-${{ matrix.input }} \
                  --payload $(echo "{\"input\": \"${{ matrix.input }}\"}" | base64) \
                  --output json out 
                if grep -q "Error" out; then
                  exit1
                fi
          - name: Destroy test function
            if: ${{ always() }}
            run: |
                aws lambda delete-function --function-name test-function-${{ matrix.input }}

      deploy:
        runs-on: ubuntu-latest
        needs: upload
        steps:
          - name: Configure AWS credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: us-east-1
          - name: Update function code
            run: |
                aws lambda update-function-code \
                  --function-name <YOUR FUNCTION NAME HERE> \
                  --s3-bucket <YOUR BUCKET NAME HERE> \
                  --s3-key ${{ github.repository }}/${{ github.sha }}.zip \
                  --publish

      docs:
        runs-on: ubuntu-latest
        needs: deploy
        steps:
          - name: Check out code
            uses: actions/checkout@v2
          - name: Deploy docs
            uses: mhausenblas/mkdocs-deploy-gh-pages@master
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
              CONFIG_FILE: mkdocs.yaml
    ```
