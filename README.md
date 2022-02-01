# cwl-upload-conformance-badges
It is a custom Github Action for uploading CWL conformance badges to specified repository.

## Example

```yaml
jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      ...

      - name: Run conformance tests
        id: run-conformance
        uses: tom-tan/cwl-run-conformance-tests@v1.0.0
        with:
          cwlVersion: v1.0
          runner: your-runner
          timeout: 30
          skip-python-install: true

      - name: Save badges
        if: success() && github.event_name == 'push' # upload badges when this action is invoked by `push` event
        uses: tom-tan/cwl-upload-conformance-badges@v1.0.0
        with:
          cwlVersion: v1.0
          runner-name: your-runner
          badgedir: ${{ steps.run-conformance.outputs.badgedir }}
          repository: ${{ github.repository_owner }}/conformance
          upload-default-branch: true
          token: ${{ secrets.CONFORMANCE_TOKEN }}
```

## Input parameters

| Parameters | Required | Default | Description |
|---|---|---|---|
| `cwlVersion` | true | - | target CWL version |
| `runne-namer` | true | - | name of CWL runner |
| `badgedir` | true | - | full path to the directory that stores conformance badges |
| `repository` | true | - | repository (in the form of `owner/repo`) to store badges |
| `upload-default-branch` | false | false | whether uploading the result of HEAD in the default branch |
| `token` | true | - | token to commit the repository specified in the `repository` field |

This action has no output parameters.
