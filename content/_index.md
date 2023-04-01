---
title: Home
---

Suppose we have the following GitHub workflow:

```yml
jobs:
  add_commit:
    runs-on: ubuntu-latest
    outputs:
      commit_hash: ${{ steps.add_commit.outputs.commit_hash }}
    steps:
      - uses: actions/checkout@v3

      - id: add_commit
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: Add empty commit
          commit_options: --allow-empty
          skip_dirty_check: true
  
  deploy:
    needs: add_commit
    runs-on: ubuntu-latest
    steps:
      # act on changes from add_commit job
      # ...
```

## Scenarios

### defaultCheckout

```yml
deploy:
  needs: add_commit
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
```

### defaultCheckoutAndPull

```yml
deploy:
  needs: add_commit
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
    - run: git pull
```

### checkoutGithubRef

```yml
deploy:
  needs: add_commit
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
      with:
        ref: ${{ inputs.github_ref }}
```

### checkoutCommitHash

```yml
deploy:
  needs: add_commit
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3
      with:
        ref: ${{ needs.add_commit.outputs.commit_hash }}
```

## Test Runs

{{< results-table >}}
