# Issue Comment GitHub Action

[![Build & Test](https://github.com/badsyntax/github-action-issue-comment/actions/workflows/test.yml/badge.svg?branch=master)](https://github.com/badsyntax/github-action-issue-comment/actions/workflows/test.yml)

Comment on a GitHub Issue using an optional Handlebars template or body string. No need to manually find an existing comment to update, just supply a custom comment ID and the Action will track the comment automatically.

Features:

- Create comment
- Update comment
- Delete comment

## Getting Started

```yml
name: 'Comment On Pull Request'

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  deploy:
    name: 'Render'
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2

      - uses: badsyntax/github-action-issue-comment@master
        name: Comment on Pull Request With Template
        if: github.event_name == 'pull_request'
        with:
          action: 'create-clean', # one of "create", "update", "delete", or "create-clean"
          template: '.github/pr-comment-template.hbs'
          id: example
          token: ${{ secrets.GITHUB_TOKEN }}
          issue-number: ${{ github.event.pull_request.number }}
          template-inputs: |
            {
              "firstName": "Bob",
              "lastName": "Marley"
            }

      - uses: badsyntax/github-action-issue-comment@master
        name: Comment on Pull Request With Body
        with:
          action: 'create-clean'
          id: example2
          issue-number: ${{ github.event.pull_request.number }}
          body: 'Test comment'
          token: ${{ secrets.GITHUB_TOKEN }}
```

## Action Inputs

| Name              | Description                                                                                                                                                                                                                            | Example                                                                         |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `action`          | Action, one of `update`, `create`, `delete`, or `create-clean`'. The `update` action will first create a comment if it doesn't exist, else update it. `create-clean` will first delete the existing comment, then create a new comment | `update`                                                                        |
| `template`        | The path to the handlebars template file, if not using the `body` option (optional)                                                                                                                                                    | `./.github/pr-comment-template.hbs`                                             |
| `body`            | The comment body, if not using the `template` option (optional)                                                                                                                                                                        | "a message"                                                                     |
| `template-inputs` | A JSON object of key value pairs (required only if using the `template` option) (can include newlines)                                                                                                                                 | `{"key":"value"}`                                                               |
| `token`           | GITHUB_TOKEN (issues: write, pull-requests: write) or a repo scoped PAT                                                                                                                                                                | `${{ secrets.GITHUB_TOKEN }}`                                                   |
| `issue-number`    | The GitHub issue number                                                                                                                                                                                                                | `${{ github.event.pull_request.number }}` or `${{ github.event.issue.number }}` |

## Related Projects

- [badsyntax/github-action-render-template](https://github.com/badsyntax/github-action-render-template)

## Support

- 👉 [Submit a bug report](https://github.com/badsyntax/github-action-issue-comment/issues/new?assignees=badsyntax&labels=bug&template=bug_report.md&title=)
- 👉 [Submit a feature request](https://github.com/badsyntax/github-action-issue-comment/issues/new?assignees=badsyntax&labels=enhancement&template=feature_request.md&title=)

## License

See [LICENSE.md](./LICENSE.md).
