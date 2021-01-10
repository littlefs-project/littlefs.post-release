## littlefs post-release scripts

This repo contains the post-release scripts for [littlefs]. When a release is
made in littlefs, [@geky-bot] will trigger a [repository_dispatch] event on this
repo, running any GitHub workflows registered to the event.

This can be used to automate the process of merging new littlefs releases into
dependent repos.

These workflows are ran in a separate repo to make it easier to patch the
workflowss without needing to merge every change into the littlefs's default
branch. (This also avoid chicken-and-egg problems related to releases
updating their own workflows.)

[littlefs]: https://github.com/littlefs-project/littlefs
[repository_dispatch]: https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#repository_dispatch
[@geky-bot]: https://github.com/geky-bot

## Adding your own workflow

Dependency management can be a pain, so feel free to create a PR to add your own
dependency update workflow.

The best place to start would be to copy [littlefs-fuse's workflow] and use that
as a starting point. Separate workflows run independently, so if one workflow
fails it doesn't risk halting the whole release process. We can also manually
run workflows that are registered to the [workflow_dispatch] event if you ask
nicely.

You can find the documentation to GitHub's workflow syntax
[here][workflow-syntax].

In addition, there are a couple of [GitHub secrets] to help create PRs:

- `BOT_TOKEN` - This is a [personal access token (PAT)] for [@geky-bot]

  Note that you probably want to use this over GitHub's GITHUB_TOKEN
  because GITHUB_TOKEN does not trigger other workflows. This has the odd
  effect of causing CI to not run on any generated PRs.

- `BOT_USER` - The bot's GitHub username, i.e. `geky-bot`

- `BOT_EMAIL` - An email to use for Git commits, i.e `bot@geky.net` (not a
  real email)

[littlefs-fuse's workflow]: .github/workflows/littlefs-fuse.yml
[workflow_dispatch]: https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#workflow_dispatchhttps://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows#workflow_dispatch
[workflow-syntax]: https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions
[GitHub secrets]: https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets
[personal access token (PAT)]: https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token
[@geky-bot]: https://github.com/geky-bot

## Workflow statuses

| Dependency | Workflow | Status |
|:--|:--|:--|
| [littlefs-fuse](https://github.com/littlefs-project/littlefs-fuse) | [littlefs-fuse.yml](.github/workflows/littlefs-fuse.yml) | [![littlefs-fuse](https://github.com/littlefs-project/littlefs.post-release/workflows/littlefs-fuse/badge.svg)](https://github.com/littlefs-project/littlefs.post-release/actions?query=workflow%3Alittlefs-fuse) |
