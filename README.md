---------
⚠️ This repository isn’t maintained anymore.
---------

# GitHub Actions for Jira

The GitHub Actions for [Jira](https://www.atlassian.com/software/jira) to create and edit Jira issues.

- Automatically transition an issue to done when a pull request whose name contains the issue key is merged
- Automatically create a new Jira issue when a GitHub issue is created
- Automatically add a comment to a Jira issue when a commit message contains the issue key
- Automatically create a Jira issue for each `// TODO:` in code

## Actions

- [`Login`](https://github.com/NTTSH-Engineering/gajira-login) - Log in to the Jira API
- [`CLI`](https://github.com/NTTSH-Engineering/setup-jira) - Wrapped [go-jira](https://github.com/Netflix-Skunkworks/go-jira) CLI for common Jira actions
- [`Find issue key`](https://github.com/NTTSH-Engineering/jira-find-issue-key) - Search for an issue key in commit message, branch name, etc. This issue key is then saved and used by the next actions in the same workflow
- [`Create`](https://github.com/NTTSH-Engineering/jira-create-issue) - Create a new Jira issue
- [`Transition`](https://github.com/NTTSH-Engineering/jira-issue-transition) - Transition a Jira issue
- [`Comment`](https://github.com/NTTSH-Engineering/jira-add-comment) - Add a comment to a Jira issue
- [`TODO`](https://github.com/NTTSH-Engineering/jira-issue-from-todo) - Create a Jira issue for each TODO comment in committed code

## Usage
An example workflow to transition issue on `push`:

```
on:
  push

name: Test Transition Issue

jobs:
  test-transition-issue:
    name: Transition Issue
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@master

    - name: Login
      uses: NTTSH-Engineering/gajira-login@master
      env:
        JIRA_BASE_URL: ${{ var.JIRA_BASE_URL }}
        JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
        JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}

    - name: Find Issue Key
      uses: ./
      with:
        from: commits

    - name: Transition issue
      uses: NTTSH-Engineering/gajira-transition@master
      with:
        issue: ${{ steps.create.outputs.issue }}
        transition: "In Progress"
```
