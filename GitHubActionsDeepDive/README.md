# GitHub Action Deep Dive
Continuous integration, delivery, and deployment are quickly going from nice-to-haves to must-haves in any software development project. GitHub Actions is emerging as a fast-growing CI/CD solution — it's simple to get started, has strong integration with third-party tools and cloud providers, and has a robust, engaged community. This course will examine GitHub Actions’ approach to CI/CD — developing workflows, managing jobs, and securing resources. It will demonstrate how to leverage other GitHub services with Actions. It will go step-by-step through setting up a CI/CD workflow for an AWS Lambda function. Finally, learners will build their own GitHub Actions workflow to deploy a static site to an S3 bucket.

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
