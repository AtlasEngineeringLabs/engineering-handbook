# GitHub CLI Cheat Sheet

A practical reference for the GitHub CLI (`gh`) used throughout the **Atlas Engineering Labs Academy**.

---

# GitHub CLI Workflow

```text
                    Start Work
                        │
                        ▼
                 gh auth status
                        │
                        ▼
                  git switch main
                        │
                        ▼
                git pull origin main
                        │
                        ▼
       git switch -c feature/<feature-name>
                        │
                        ▼
              Make your code changes
                        │
                        ▼
                  git status
                        │
                        ▼
      git commit -m "type(scope): message"
                        │
                        ▼
                  git push
                        │
                        ▼
               gh pr create
              ▼
                gh pr view
                        │
                        ▼
                gh pr diff
                        │
                        ▼
              Review Pull Request
                        │
                        ▼
               gh pr merge
                        │
                        ▼
                git switch main
                        │
                        ▼
              git pull origin main
                        │
                        ▼
             Ready for next task
```

---

# Authentication

## Login to GitHub

```bash
gh auth login
```

Authenticates GitHub CLI with your GitHub account.

Choose:

- GitHub.com
- SSH
- Login with browser

---

## Check Auth
gh auth status
```

Displays:

- Logged-in user
- Authentication method
- Git protocol
- Token status

Useful for verifying GitHub CLI is correctly configured.

---

# Repository Commands

## View Current Repository

```bash
gh repo view
```

Displays information about the current repository.

Example:

```
AtlasEngineeringLabs/engineering-playground
```

---

## View Repository in Browser

```bash
gh repo view --web
```

Opens the current repository in your default web browser.

---

## Edit Repository Settings

```bash
gh repo edit
```

Updates repository settings.

Example:

```bash
gh repo edit --default-branch main
```

Changes the repository's default branch.

---

# Pull Requests

## Create a Pull Request

```bash
gh pr create
```

Creates a Pull Request interactively.

GitHub CLI will ask for:

- Base branch
- Feature branch
- Title
- Description

---

## Create a Pull Request (Non-Interactive)

```bash
gh pr create \
  --title "docs(terraform): update guide" \
  --body "Updated Terraform installation guide."
```

Creates a Pull Request directly from the command line.

---

## View a Pull Request

```bash
gh pr view
```

Displays details about the current Pull Request.

---

## View a Specific Pull Request

```bash
gh pr view 1
```

Displays Pull Request #1.

Useful for reviewing:

- Status
- Description
- Reviewers
- Branches

---

## Open Pull Request in Browser

```bash
gh pr view --web
```

Opens the Pull Request in your browser.

---

## View Pull Request Diff

```bash
gh pr diff
```

Displays the file differences for the current Pull Request.

---

## View a Specific Pull Request Diff

```bash
gh pr diff 1
```

Displays the changes for Pull Request #1.

---

## Checkout a Pull Request

```bash
gh pr checkout 5
```

Checks out Pull Request #5 locally.

Useful when reviewing another engineer's work.

---

## Merge a Pull Request

```bash
gh pr merge
```

Merges the current Pull Request.

---

## Merge Using Squash

```bash
gh pr merge --squash
```

Squashes all commits into a single commit before merging.

This is the preferred merge strategy used throughout the academy.

---

## Close a Pull Request

```bash
gh pr close 3
```

Closes Pull Request #3 without merging.

---

# Issues

## List Issues

```bash
gh issue list
```

Displays all open issues.

---

## View an Issue

```bash
gh issue view 5
```

Displays Issue #5.

---

## Create an Issue

```bash
gh issue create
```

Creates a new GitHub issue.

---

## Close an Issue

```bash
gh issue close 5
```

Closes Issue #5.

---

# Releases

## List Releases

```bash
gh release list
```

Displays all repository releases.

---

## Create a Release

```bash
gh release create v1.0.0
```

Creates a new GitHub release.

---

# GitHub Actions

## List Workflows

```bash
gh workflow list
```

Displays all GitHub Actions workflows.

---

## Run a Workflow

```bash
gh workflow run "Terraform CI"
```

Triggers a workflow manually.

---

## List Workflow Runs

```bash
gh run list
```

Displays recent workflow runs.

---

## Watch a Workflow Run

```bash
gh run watch
```

Streams the progress of a running workflow.

---

## View Workflow Logs

```bash
gh run view
```

Displays logs from a workflow run.

---

# Repository Health

## View Repository Summary

```bash
gh repo view
```

Displays repository information.

---

## View Pull Requests

```bash
gh pr list
```

Lists all open Pull Requests.

---

## View Issues

```bash
gh issue list
```

Lists all open GitHub Issues.

---

# Daily Commands

The commands you'll use most often:

```bash
gh auth status
gh repo view
gh pr create
gh pr view
gh pr diff
gh pr view --web
gh pr merge --squash
gh issue list
gh workflow list
gh run list
```

---

# Engineering Best Practices

Before opening a Pull Request:

- ✅ Review your local changes (`git diff`)
- ✅ Review staged changes (`git diff --staged`)
- ✅ Ensure `git status` is clean
- ✅ Use Conventional Comages
- ✅ Push to a feature branch
- ✅ Create a descriptive Pull Request
- ✅ Review the Pull Request before merging
- ✅ Prefer **Squash and Merge**
- ✅ Delete the feature branch after merging

---

# Future GitHub CLI Topics

As the academy progresses, this handbook will be expanded to include:

- Repository Templates
- GitHub Projects
- Labels
- Milestones
- Discussions
- Security Alerts
- Dependabot
- Codespaces
- GitHub Packages
- Container Registry
- Repository Rulesets
- Branch Protection Rules
- GitHub Apps
- Organization Management
- Secrets Management
- Environment Configuration
