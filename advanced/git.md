# Git, GitHub, and GitHub Actions for Python Developers

## Table of Contents

1. Introduction
2. Git Fundamentals Revisited
3. Advanced Git Concepts
4. Git Branching Strategies
5. Commit Standards and History Management
6. Advanced Git Workflows
7. Git Internals
8. GitHub for Professional Development
9. Pull Requests and Code Review
10. Managing Large Python Projects
11. GitHub Security and Dependency Management
12. Semantic Versioning and Releases
13. GitHub Actions Fundamentals
14. Building Python CI Pipelines
15. Advanced GitHub Actions Techniques
16. Testing Python Applications in CI
17. Linting, Formatting, and Type Checking
18. Docker Integration with GitHub Actions
19. Publishing Python Packages Automatically
20. Deployment Pipelines
21. Monorepos and Multi-Project Pipelines
22. Secrets Management
23. Performance Optimization in CI/CD
24. Self-Hosted Runners
25. Reusable Workflows
26. GitOps and Infrastructure Automation
27. Real-World Python Project Example
28. Common Pitfalls and Best Practices
29. Recommended Repository Structure
30. Final Thoughts

---

# 1. Introduction

Modern Python development requires much more than writing application logic. Professional software engineering workflows rely heavily on:

- Version control with Git
- Collaboration through GitHub
- Continuous Integration/Continuous Deployment (CI/CD)
- Automated testing pipelines
- Security scanning
- Deployment automation

This tutorial focuses on advanced practical usage for Python developers.

By the end, you will understand:

- Advanced Git workflows
- Professional repository management
- GitHub collaboration techniques
- CI/CD automation with GitHub Actions
- Automated packaging and deployment
- Secure software delivery pipelines

---

# 2. Git Fundamentals Revisited

## Installing Git

### Linux

```bash
sudo apt install git
```

### macOS

```bash
brew install git
```

### Windows

Download from:

```text
https://git-scm.com/
```

---

## Configuring Git

```bash
git config --global user.name "John Doe"
git config --global user.email "john@example.com"
```

Enable useful settings:

```bash
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.editor vim
git config --global color.ui auto
```

View configuration:

```bash
git config --list
```

---

## Creating a Repository

```bash
mkdir python-project
cd python-project
git init
```

---

## .gitignore for Python

```gitignore
__pycache__/
*.py[cod]
*.so
.Python
venv/
.env
.pytest_cache/
.mypy_cache/
.coverage
htmlcov/
dist/
build/
*.egg-info/
.idea/
.vscode/
.DS_Store
```

---

# 3. Advanced Git Concepts

## The Three Git Trees

Git operates using three major areas:

| Area | Description |
|---|---|
| Working Directory | Your files |
| Staging Area | Files prepared for commit |
| Repository | Permanent commit history |

---

## Git Object Model

Git stores everything as objects:

| Object | Purpose |
|---|---|
| Blob | File contents |
| Tree | Directory structure |
| Commit | Snapshot metadata |
| Tag | Named reference |

Inspect objects:

```bash
git cat-file -p HEAD
```

---

## Detached HEAD

```bash
git checkout <commit_hash>
```

You are now outside any branch.

Create a branch if you want to preserve changes:

```bash
git switch -c experiment
```

---

# 4. Git Branching Strategies

## Feature Branch Workflow

Common for Python applications.

```text
main
 ├── feature/login
 ├── feature/payments
 └── bugfix/api-timeout
```

Workflow:

```bash
git checkout -b feature/authentication
```

Commit changes:

```bash
git add .
git commit -m "feat: add JWT authentication"
```

Push branch:

```bash
git push origin feature/authentication
```

---

## Git Flow

Uses:

- main
- develop
- feature/*
- release/*
- hotfix/*

Example:

```bash
git checkout develop
git checkout -b feature/caching
```

---

## Trunk-Based Development

Preferred in high-velocity engineering teams.

Principles:

- Short-lived branches
- Frequent merges
- Continuous deployment
- Heavy automation

---

# 5. Commit Standards and History Management

## Conventional Commits

Examples:

```text
feat: add OAuth login
fix: resolve memory leak in worker
refactor: simplify caching layer
docs: update API examples
test: add integration tests
```

Benefits:

- Automatic changelogs
- Semantic versioning
- Easier code review

---

## Interactive Rebase

Rewrite history safely before merge.

```bash
git rebase -i HEAD~5
```

Commands:

| Command | Meaning |
|---|---|
| pick | Keep commit |
| squash | Merge commits |
| reword | Change message |
| drop | Remove commit |

---

## Squashing Commits

```bash
git rebase -i main
```

Result:

```text
Before:
- fix typo
- update docs
- adjust tests

After:
- improve authentication module
```

---

## Cherry Picking

```bash
git cherry-pick <commit_hash>
```

Useful for:

- Hotfixes
- Selective backports
- Moving fixes across branches

---

# 6. Advanced Git Workflows

## Stashing Changes

```bash
git stash
```

List stashes:

```bash
git stash list
```

Restore:

```bash
git stash pop
```

Named stash:

```bash
git stash push -m "experimental optimization"
```

---

## Bisecting Bugs

Find problematic commits efficiently.

```bash
git bisect start
git bisect bad
git bisect good v1.0
```

Git performs binary search over commits.

---

## Recovering Deleted Commits

```bash
git reflog
```

Restore:

```bash
git checkout <commit_hash>
```

---

# 7. Git Internals

## Viewing Commit Graph

```bash
git log --oneline --graph --decorate --all
```

---

## Understanding References

Git references:

```text
HEAD
refs/heads/main
refs/tags/v1.0
```

---

## Packfiles

Git optimizes storage using packfiles.

Run garbage collection:

```bash
git gc
```

---

# 8. GitHub for Professional Development

## Creating a GitHub Repository

```bash
git remote add origin https://github.com/user/project.git
git push -u origin main
```

---

## Repository Settings

Important settings:

- Branch protection rules
- Required reviews
- Required status checks
- Restrict force pushes
- Require signed commits

---

## Branch Protection

Recommended for production branches.

Enable:

- Pull request reviews
- Passing CI checks
- Linear history
- Conversation resolution

---

## CODEOWNERS

Automatically request reviewers.

```text
# .github/CODEOWNERS

* @team-leads
api/* @backend-team
frontend/* @frontend-team
```

---

# 9. Pull Requests and Code Review

## Writing Good Pull Requests

Template:

```markdown
## Summary
Describe changes.

## Motivation
Explain why.

## Testing
- [x] Unit tests
- [x] Integration tests

## Screenshots
If applicable.
```

---

## Reviewing Code Professionally

Focus on:

- Correctness
- Readability
- Security
- Performance
- Maintainability

Avoid:

- Personal criticism
- Nitpicking style already automated

---

# 10. Managing Large Python Projects

## Recommended Structure

```text
project/
├── src/
│   └── app/
├── tests/
├── docs/
├── scripts/
├── .github/
│   └── workflows/
├── pyproject.toml
├── README.md
└── Dockerfile
```

---

## Using pyproject.toml

Example:

```toml
[project]
name = "advanced-api"
version = "0.1.0"
description = "Advanced Python API"
requires-python = ">=3.11"

[tool.pytest.ini_options]
testpaths = ["tests"]

[tool.black]
line-length = 88

[tool.ruff]
line-length = 88
```

---

# 11. GitHub Security and Dependency Management

## Dependabot

Enable automatic dependency updates.

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
```

---

## Secret Scanning

GitHub can detect:

- API keys
- Tokens
- Credentials

Never commit:

```text
.env
credentials.json
private.pem
```

---

## Signed Commits

Using GPG:

```bash
git config --global commit.gpgsign true
```

---

# 12. Semantic Versioning and Releases

## Semantic Versioning

Format:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
2.5.1
```

Meaning:

| Change | Increment |
|---|---|
| Breaking change | MAJOR |
| New feature | MINOR |
| Bug fix | PATCH |

---

## Creating Tags

```bash
git tag -a v1.0.0 -m "Initial release"
git push origin v1.0.0
```

---

# 13. GitHub Actions Fundamentals

## What is GitHub Actions?

GitHub Actions is a CI/CD platform integrated into GitHub.

You can automate:

- Testing
- Building
- Linting
- Deployment
- Packaging
- Notifications

---

## Workflow Structure

```yaml
name: Python CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

---

## Workflow Components

| Component | Purpose |
|---|---|
| Workflow | Entire automation |
| Job | Independent task |
| Step | Single action |
| Action | Reusable unit |
| Runner | Execution environment |

---

# 14. Building Python CI Pipelines

## Installing Dependencies Efficiently

```yaml
- name: Cache pip
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
```

---

## Matrix Testing

Test multiple Python versions.

```yaml
strategy:
  matrix:
    python-version: ["3.10", "3.11", "3.12"]
```

Complete example:

```yaml
name: Test Matrix

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - run: pip install -r requirements.txt

      - run: pytest
```

---

# 15. Advanced GitHub Actions Techniques

## Conditional Execution

```yaml
if: github.ref == 'refs/heads/main'
```

---

## Environment Variables

```yaml
env:
  APP_ENV: production
```

---

## Job Outputs

```yaml
jobs:
  build:
    outputs:
      image_tag: ${{ steps.meta.outputs.tag }}
```

---

## Reusable Composite Actions

```yaml
name: Setup Python Environment

runs:
  using: composite
  steps:
    - uses: actions/setup-python@v5
      with:
        python-version: '3.12'
```

---

# 16. Testing Python Applications in CI

## Pytest Integration

Install:

```bash
pip install pytest pytest-cov
```

Workflow:

```yaml
- name: Run tests
  run: |
    pytest \
      --cov=src \
      --cov-report=xml \
      --cov-report=term
```

---

## Upload Coverage

Using Codecov:

```yaml
- name: Upload coverage
  uses: codecov/codecov-action@v4
```

---

## Integration Tests with PostgreSQL

```yaml
services:
  postgres:
    image: postgres:16
    env:
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
      POSTGRES_DB: testdb
    ports:
      - 5432:5432
```

---

# 17. Linting, Formatting, and Type Checking

## Ruff

Install:

```bash
pip install ruff
```

Workflow:

```yaml
- name: Run Ruff
  run: ruff check .
```

---

## Black

```yaml
- name: Check formatting
  run: black --check .
```

---

## MyPy

```yaml
- name: Type checking
  run: mypy src
```

---

## Full Quality Pipeline

```yaml
name: Quality Checks

on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - run: pip install -r requirements-dev.txt

      - run: ruff check .

      - run: black --check .

      - run: mypy src

      - run: pytest
```

---

# 18. Docker Integration with GitHub Actions

## Building Docker Images

```yaml
- name: Build image
  run: docker build -t app:latest .
```

---

## Logging Into Docker Hub

```yaml
- name: Login to Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

---

## Pushing Images

```yaml
- name: Push image
  run: |
    docker tag app:latest user/app:latest
    docker push user/app:latest
```

---

# 19. Publishing Python Packages Automatically

## Build Package

```yaml
- name: Build package
  run: |
    pip install build
    python -m build
```

---

## Publish to PyPI

```yaml
- name: Publish
  uses: pypa/gh-action-pypi-publish@release/v1
  with:
    password: ${{ secrets.PYPI_API_TOKEN }}
```

---

## Full Release Workflow

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - run: pip install build

      - run: python -m build

      - uses: pypa/gh-action-pypi-publish@release/v1
        with:
          password: ${{ secrets.PYPI_API_TOKEN }}
```

---

# 20. Deployment Pipelines

## Deploy to AWS EC2

```yaml
- name: Deploy via SSH
  uses: appleboy/ssh-action@v1.0.3
  with:
    host: ${{ secrets.SERVER_HOST }}
    username: ubuntu
    key: ${{ secrets.SERVER_SSH_KEY }}
    script: |
      cd /app
      git pull
      docker compose up -d --build
```

---

## Deploy to Kubernetes

```yaml
- name: Set kubectl context
  uses: azure/setup-kubectl@v4

- name: Deploy
  run: kubectl apply -f k8s/
```

---

# 21. Monorepos and Multi-Project Pipelines

## Monorepo Structure

```text
repo/
├── services/
│   ├── api/
│   ├── worker/
│   └── billing/
├── shared/
└── libs/
```

---

## Path-Based Triggers

```yaml
on:
  push:
    paths:
      - 'services/api/**'
```

---

# 22. Secrets Management

## GitHub Secrets

Store secrets in:

```text
Settings → Secrets and variables → Actions
```

Use secrets:

```yaml
${{ secrets.API_TOKEN }}
```

---

## Best Practices

- Rotate credentials regularly
- Use short-lived tokens
- Never print secrets in logs
- Use OIDC where possible

---

# 23. Performance Optimization in CI/CD

## Dependency Caching

```yaml
- uses: actions/cache@v4
```

---

## Parallel Jobs

```yaml
jobs:
  lint:
  test:
  security:
```

Independent jobs run simultaneously.

---

## Fail Fast

```yaml
strategy:
  fail-fast: true
```

---

# 24. Self-Hosted Runners

## Why Use Them?

Benefits:

- Faster builds
- Custom hardware
- GPU support
- Internal network access

---

## Security Considerations

Avoid running untrusted pull requests on self-hosted runners.

---

# 25. Reusable Workflows

## Shared Workflow

```yaml
# .github/workflows/test.yml

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest
```

---

## Reuse Workflow

```yaml
jobs:
  call-workflow:
    uses: org/repo/.github/workflows/test.yml@main
```

---

# 26. GitOps and Infrastructure Automation

## GitOps Principles

Infrastructure state is stored in Git.

Changes happen through:

- Pull requests
- Review workflows
- Automated deployment

---

## Example Flow

```text
Developer → Git Push → CI Pipeline → Deployment → Production
```

---

# 27. Real-World Python Project Example

## Production-Ready FastAPI Workflow

```yaml
name: FastAPI CI/CD

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: ["3.11", "3.12"]

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: testdb
        ports:
          - 5432:5432

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install -r requirements-dev.txt

      - name: Run linters
        run: |
          ruff check .
          black --check .
          mypy src

      - name: Run tests
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/testdb
        run: pytest

  docker:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - run: docker build -t myapp .

      - run: |
          docker tag myapp user/myapp:latest
          docker push user/myapp:latest
```

---

# 28. Common Pitfalls and Best Practices

## Avoid Large Commits

Prefer:

```text
Small, focused commits
```

Instead of:

```text
Massive mixed-feature commits
```

---

## Never Force Push Shared Branches

Dangerous:

```bash
git push --force
```

Safer:

```bash
git push --force-with-lease
```

---

## Keep CI Fast

Recommendations:

- Cache dependencies
- Parallelize jobs
- Run minimal work
- Avoid unnecessary containers

---

## Protect Secrets

Never hardcode:

```python
API_KEY = "secret"
```

Use environment variables.

---

# 29. Recommended Repository Structure

## Enterprise Python Backend

```text
backend/
├── src/
│   ├── api/
│   ├── services/
│   ├── repositories/
│   ├── models/
│   └── core/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── alembic/
├── scripts/
├── docs/
├── .github/
│   ├── workflows/
│   └── CODEOWNERS
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
└── README.md
```

---

# 30. Final Thoughts

The most important long-term skills are:

1. Writing clean commit history
2. Automating everything possible
3. Maintaining fast feedback loops
4. Enforcing quality through CI
5. Designing reproducible deployment pipelines

As projects grow, strong automation and repository discipline become essential engineering multipliers.

---

# Suggested Next Topics

- Kubernetes for Python Developers
- Terraform and Infrastructure as Code
- Advanced Docker for Python
- Secure Supply Chain Engineering
- Observability and Monitoring
- Distributed Systems Deployment
