# gh-pages-action

[![DeepWiki](https://img.shields.io/badge/DeepWiki-suzuki--shunsuke%2Fgh--pages--action-blue.svg?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAyCAYAAAAnWDnqAAAAAXNSR0IArs4c6QAAA05JREFUaEPtmUtyEzEQhtWTQyQLHNak2AB7ZnyXZMEjXMGeK/AIi+QuHrMnbChYY7MIh8g01fJoopFb0uhhEqqcbWTp06/uv1saEDv4O3n3dV60RfP947Mm9/SQc0ICFQgzfc4CYZoTPAswgSJCCUJUnAAoRHOAUOcATwbmVLWdGoH//PB8mnKqScAhsD0kYP3j/Yt5LPQe2KvcXmGvRHcDnpxfL2zOYJ1mFwrryWTz0advv1Ut4CJgf5uhDuDj5eUcAUoahrdY/56ebRWeraTjMt/00Sh3UDtjgHtQNHwcRGOC98BJEAEymycmYcWwOprTgcB6VZ5JK5TAJ+fXGLBm3FDAmn6oPPjR4rKCAoJCal2eAiQp2x0vxTPB3ALO2CRkwmDy5WohzBDwSEFKRwPbknEggCPB/imwrycgxX2NzoMCHhPkDwqYMr9tRcP5qNrMZHkVnOjRMWwLCcr8ohBVb1OMjxLwGCvjTikrsBOiA6fNyCrm8V1rP93iVPpwaE+gO0SsWmPiXB+jikdf6SizrT5qKasx5j8ABbHpFTx+vFXp9EnYQmLx02h1QTTrl6eDqxLnGjporxl3NL3agEvXdT0WmEost648sQOYAeJS9Q7bfUVoMGnjo4AZdUMQku50McDcMWcBPvr0SzbTAFDfvJqwLzgxwATnCgnp4wDl6Aa+Ax283gghmj+vj7feE2KBBRMW3FzOpLOADl0Isb5587h/U4gGvkt5v60Z1VLG8BhYjbzRwyQZemwAd6cCR5/XFWLYZRIMpX39AR0tjaGGiGzLVyhse5C9RKC6ai42ppWPKiBagOvaYk8lO7DajerabOZP46Lby5wKjw1HCRx7p9sVMOWGzb/vA1hwiWc6jm3MvQDTogQkiqIhJV0nBQBTU+3okKCFDy9WwferkHjtxib7t3xIUQtHxnIwtx4mpg26/HfwVNVDb4oI9RHmx5WGelRVlrtiw43zboCLaxv46AZeB3IlTkwouebTr1y2NjSpHz68WNFjHvupy3q8TFn3Hos2IAk4Ju5dCo8B3wP7VPr/FGaKiG+T+v+TQqIrOqMTL1VdWV1DdmcbO8KXBz6esmYWYKPwDL5b5FA1a0hwapHiom0r/cKaoqr+27/XcrS5UwSMbQAAAABJRU5ErkJggg==)](https://deepwiki.com/suzuki-shunsuke/gh-pages-action) [![License](http://img.shields.io/badge/license-mit-blue.svg?style=flat-square)](https://raw.githubusercontent.com/suzuki-shunsuke/gh-pages-action/main/LICENSE) | [action.yaml](action.yaml)

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

- `${{secrets.GITHUB_TOKEN}}`: This can't trigger new workflow runs.
- GitHub App Installation access token
- :thumbsdown: GitHub Personal Access Token: This can't create verified commits

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
        uses: suzuki-shunsuke/gh-pages-action@6229a3f8d89389da5c234aab3b37ba51a7fee3f6 # v0.0.1
        with:
          publish_dir: build
          destination_dir: docs
```

### GitHub Access token

`${{github.token}}` is used by default.
You can also pass a GitHub access token or a pair of GitHub App ID and private key.

```yaml
- uses: suzuki-shunsuke/gh-pages-action@6229a3f8d89389da5c234aab3b37ba51a7fee3f6 # v0.0.1
  with:
    app_id: ${{secrets.APP_ID}}
    app_private_key: ${{secrets.APP_PRIVATE_KEY}}
```
