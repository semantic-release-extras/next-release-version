# next-release-version

**next-release-version** is a GitHub Action that calculates the next [semantic-
release] version number (if any).

[semantic-release]: https://github.com/semantic-release/semantic-release

## Use

Create a [job] that invokes this action:

```yaml
jobs:
  get-next-version:
    name: Calculate next release
    runs-on: ubuntu-latest
    outputs:
      new-release-published: ${{ steps.get-next-version.outputs.new-release-published }}
      new-release-version: ${{ steps.get-next-version.outputs.new-release-version }}

    steps:
      - uses: semantic-release-extras/next-release-version@v1
        id: get-next-version
```

Then reference the job outputs in subsequent jobs:

```yaml
jobs:
  do-the-thing:
    name: Zhu Li, do the thing
    runs-on: ubuntu-latest
    if: needs.get-next-version.outputs.new-release-published == 'true'
    steps:
      - run: ./do-the-thing
```

[Here] is a complete workflow you can use as a reference.

[here]: https://github.com/semantic-release-cargo/semantic-release-cargo/blob/master/.github/workflows/release.yml
[job]: https://docs.github.com/en/actions/using-jobs/using-jobs-in-a-workflow

## Outputs

This Action exports the following outputs:

| Output                  | Description                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `new-release-published` | Indicates whether a new release will be published. The value is a string, either `"true"` or `"false"`.           |
| `new-release-version`   | If a new release will be published, the version of the new release. The value is a string, for example `"1.3.0"`. |
