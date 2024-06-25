# upload-conformance-badges
It is a custom Github Action for uploading CWL conformance badges to a specified repository.

## Example

```yaml
jobs:
  conformance:
    runs-on: ubuntu-latest
    steps:
      ...

      - name: Run conformance tests
        id: run-conformance
        uses: common-workflow-lab/run-conformance-tests@v1
        with:
          cwlVersion: v1.0
          runner: your-runner
          timeout: 30
          skip-python-install: true

      - name: Save badges
        if: success() && github.event_name == 'push' # upload badges when this action is invoked by `push` event
        uses: common-workflow-lab/upload-conformance-badges@v1
        with:
          cwlVersion: v1.0
          runner-name: your-runner
          badgedir: ${{ steps.run-conformance.outputs.badgedir }}
          repository: ${{ github.repository_owner }}/conformance
          upload-default-branch: true
          ssh-key: ${{ secrets.CONFORMANCE_KEY }}
```

## Input parameters

| Parameters | Required | Default | Description |
|---|---|---|---|
| `cwlVersion` | true | - | target CWL version |
| `runner-name` | true | - | name of CWL runner |
| `badgedir` | true | - | full path to the directory that stores conformance badges |
| `repository` | true | - | repository (in the form of `owner/repo`) to store badges |
| `upload-default-branch` | false | false | whether uploading the result of HEAD in the default branch |
| `upload-markdown-reports` | false | false | whether uploading the markdown reports in addition to the conformance badges |
| `ssh-key` | true | - | ssh key to commit the repository specified in the `repository` field. See [deploy keys](https://docs.github.com/en/developers/overview/managing-deploy-keys#deploy-keys) for details |

This action has no output parameters.
