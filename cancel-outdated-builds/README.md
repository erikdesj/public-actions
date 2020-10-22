# cancel-outdated-builds

Forked from https://github.com/rust-lang/simpleinfra/tree/master/github-actions/cancel-outdated-builds.

The `cancel-outdated-builds` action cancels the current build if a new commit is
pushed on the same branch.

## Usage

To use the action add this to your workflow:

```yaml
- uses: erikdesj/public-actions/cancel-outdated-builds@main
  with:
    github_token: "${{ secrets.github_token }}"
```

By default the action will check the GitHub API every minute. To use a custom
check interval you can add the `check_every_seconds` input:

```yaml
- uses: erikdesj/public-actions/cancel-outdated-builds@main
  with:
    github_token: "${{ secrets.github_token }}"
    check_every_seconds: 120 # 2 minutes
```

By default the action will cancel jobs regardless of how long they have been running.
This can lead to starvation.
For example, if your build takes an hour but commits are merged every 30 minutes,
then every job will get cancelled before it finishes.
You can add the `max_age_minutes` input to avoid cancelling jobs that are close to completion:

```yaml
- uses: erikdesj/public-actions/cancel-outdated-builds@main
  with:
    github_token: "${{ secrets.github_token }}"
    max_age_minutes: 15
```

## Development

The action is written in NodeJS 12, and you can install the dependencies with:

```
npm install
```

GitHub Actions requires all the dependencies to be committed, so before
creating a commit you need to run:

```
npm run build
```

The command will bundle everything in `dist/index.js`. That file will need to
be committed.
