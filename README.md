# GitHub Action for GitHub Push

The GitHub Actions for pushing to GitHub repository local changes authorizing using GitHub token.

With ease:
- update new code placed in the repository, e.g. by running a linter on it,
- track changes in script results using Git as archive,
- publish page using GitHub-Pages,
- mirror changes to a separate repository.

## Usage

### Example Workflow file

An example workflow to authenticate with GitHub Platform:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Create local changes
      run: |
        ...
    - name: Commit files
      run: |
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git commit -m "Add changes" -a
    - name: Pull changes
      run: |
        git pull --rebase
    - name: Push changes
      uses: ad-m/github-push-action@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
```

### Inputs

| name | value | default | description |
| ---- | ----- | ------- | ----------- |
| github_token | string | | Token for the repo. Can be passed in using `${{ secrets.GITHUB_TOKEN }}`. |
| branch | string | 'master' | Destination branch to push changes. |
| force | boolean | false | Determines if force push is used. |
| directory | string | '.' | Directory to change to before pushing. |
| repository | string | '' | Repository name. Default or empty repository name represents current github repository. If you want to push to other repository, you should make a [personal access token](https://github.com/settings/tokens) and use it as the `github_token` input.  |

### Caveats

Please note that a race condition is possible if you try to run workflows of the same repository in parallel and changes are present in one or both of the workflows. In order to avoid this, you should ensure the following line is added to your workflow:

```yaml
- name: Pull changes
  run: |
    git pull --rebase
```

See [the example workflow file](#example-workflow-file).

## License

The Dockerfile and associated scripts and documentation in this project are released under the [MIT License](LICENSE).

## No affiliation with GitHub Inc.

GitHub are registered trademarks of GitHub, Inc. GitHub name used in this project are for identification purposes only. The project is not associated in any way with GitHub Inc. and is not an official solution of GitHub Inc. It was made available in order to facilitate the use of the site GitHub.
