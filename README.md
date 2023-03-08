## CODEOWNERS Extension

This workflow builds upon the CODEOWNERS functionality to detect whether the PR author is on a team which owns the files they are changing.

For this to function, a PAT must be created as a REPO (or ORG) Actions secret, called TOKEN. This PAT must have read access to the ORG and the reusable-workflows repository. This relies upon a reusable workflow that can be called in this way: 

```yaml
name: Codespaces Check
on:
  pull_request:
    branches: [ "main" ]
jobs:
  caller:
    uses: boxboat/reusable-workflows/.github/workflows/codeowners-check.yml@main
    secrets: inherit
```

There is a hard requirement on using Teams instead of individual users, but this can easily be extended to support users as well. Attempts are made to provide useful logging, but the general functionality is that this will function as a check, as part of a larger workflow that is triggered by a PR being opened. If the committer who submits the PR is not an owner of the files they change, or if the files they change are not tracked by the CODEOWNERS file (master copy located in this repo's 'meta' folder), an exit code of 1 will be registered and the job will fail, with a log explaining why.
