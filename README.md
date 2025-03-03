# gh-pages-action

[![License](http://img.shields.io/badge/license-mit-blue.svg?style=flat-square)](https://raw.githubusercontent.com/suzuki-shunsuke/gh-pages-action/main/LICENSE) | [action.yaml](action.yaml)

gh-pages-action is a GitHub Action to publish GitHub Pages.
You can create **verified** commits using GitHub App.

## Why Use gh-pages-action?

Unlike similar actions, **gh-pages-action creates and pushes commits by GitHub API instead of Git commands**.
So you can create **verified commits** using GitHub Actions token `${{github.token}}` or a GitHub App installation access token.

Commit signing is so important for security.

https://docs.github.com/en/authentication/managing-commit-signature-verification

To create verified commits using Git, a GPG key or SSH key is required.
It's bothersome to manage GPG keys and SSH keys properly for automation, so it's awesome that gh-pages-action can create verified commits without them.

## GitHub Access Token

You can use the following things:

- :thumbsup: GitHub App Installation access token: We recommend this
- :thumbsdown: GitHub Personal Access Token: This can't create verified commits
- :thumbsdown: `${{secrets.GITHUB_TOKEN}}`: This can't trigger new workflow runs.

https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow

> When you use the repository's GITHUB_TOKEN to perform tasks, events triggered by the GITHUB_TOKEN, with the exception of workflow_dispatch and repository_dispatch, will not create a new workflow run.

### Required permissions

`contents:write` is required.

## How To Use

```yaml
name: Example
on:
  pull_request: {}
jobs:
  example:
    runs-on: ubuntu-24.04
    permissions:
      contents: write # To create commits
    steps:
      - name: Checkout
        uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
        with:
          persist-credentials: false

      # Build GitHub Pages
      # ...

      - name: Push changes to the remote branch
        uses: suzuki-shunsuke/gh-pages-action@main
        with:
          publish_dir: build
          destination_dir: docs
```

### GitHub Access token

`${{github.token}}` is used by default.
You can also pass a GitHub access token or a pair of GitHub App ID and private key.

```yaml
- uses: suzuki-shunsuke/gh-pages-action@main
  with:
    app_id: ${{secrets.APP_ID}}
    app_private_key: ${{secrets.APP_PRIVATE_KEY}}
```
