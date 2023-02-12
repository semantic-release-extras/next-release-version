# next-release-version

[![Build Status]](https://github.com/semantic-release-action/next-release-version/actions/workflows/release.yml)

[build status]: https://github.com/semantic-release-action/next-release-version/actions/workflows/release.yml/badge.svg?event=push

**next-release-version** is a GitHub Action that calculates the next
[semantic-release] version number (if any).

[semantic-release]: https://github.com/semantic-release/semantic-release

## Use

Invoke this action, then reference the step's output:

```yaml
---
jobs:
  compile-release-binaries:
    runs-on: ubuntu-latest
    steps:
      # Invoke the action
      - id: get-next-version
        uses: semantic-release-action/next-release-version@v3
      # Reference the step's output
      - if: steps.get-next-version.outputs.new-release-published == 'true'
        run: ./compile-release-binaries ${{ needs.get-next-version.outputs.new-release-version }}
```

If you need to reference the outputs from multiple jobs, run this action in its own job:

```yaml
jobs:
  get-next-version:
    name: Get next release version
    runs-on: ubuntu-latest
    outputs:
      new-release-published: ${{ steps.get-next-version.outputs.new-release-published }}
      new-release-version: ${{ steps.get-next-version.outputs.new-release-version }}
    steps:
      - id: get-next-version
        uses: semantic-release-action/next-release-version@v3

  do-the-thing:
    name: Zhu Li, do the thing
    runs-on: ubuntu-latest
    if: needs.get-next-version.outputs.new-release-published == 'true'
    needs:
      - get-next-version

    steps:
      - run: ./do-the-thing ${{ needs.get-next-version.outputs.new-release-version }}
```

[Here] is a complete workflow you can use as a reference.

[here]: https://github.com/semantic-release-cargo/semantic-release-cargo/blob/master/.github/workflows/release.yml

## Outputs

This workflow exports the following outputs:

| Output                  | Description                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `new-release-published` | Indicates whether a new release will be published. The value is a string, either `"true"` or `"false"`.           |
| `new-release-version`   | If a new release will be published, the version of the new release. The value is a string, for example `"1.3.0"`. |
