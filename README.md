# My Renovate config

Here is the config I use for NodeJs monorepo projects with postgres database. You can adjust for your own architecture and organisation by forking this repo and modifying. Change the `extends` property to your forked repository.

## Why use Renovate or update packages regularly?

Javascript-land moves fast! If you keep on top of package and library updates then it's not a huge problem. If you leave updates for more than a few months it becomes a commplete nightmare for your development team.

# How to use Renovate in a project

## 1. Create a renovate.json file

Use by extending from this e.g. create a `renovate.json` file in the root of your project with the following contents. Change the branch to run on to your main developing branch e.g. main, master or develop.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>darraghoriordan/renovate-config:base"],
  "baseBranches": ["main"]
}
```

## 2. Trigger github actions to run renovate every morning

You can use this in your own projects by adding a renovate github action

I have an example of this here on use-miller:

https://github.com/darraghoriordan/use-miller/blob/main/.github/workflows/rennovate.yaml

## 3. Add a secret to your github repo containing a classic github token

The token must have access to "Repo", "Workflow" and "User:read" permissions for the settings I use in renovate.

## Force renovate to run initially

There is a workflow dispatch configured in the github action so you can go to the actions tab and run it manually if you want to force it to run.

## What will Renovate do?

Renovate will create an issue in Github for you that will act as a dashboard for package updates. This issue will be updated each run with work to do or in progress. Sometimes you can tell renovate what to do the next time it runs using the checkboxes in the dashboard.

Renovate will create a number of branches under `renovate/` for you. Renovate will manage these branches and create PRs for you. You can see the PRs in the dashboard issue.

Renovate will automatically create PRs as described below but it is limited to 10 PRs at any one time to prevent overload. The first time it runs will be the noisiest! If the PRs pass all actions successfully then the next time renovate runs, it will try to auto merge any PRs that are ready to merge.

Note: You don't need to manually merge the PRs, renovate will try to do all the the next time it runs. Try running renovate a few times the first time you configure it to get a feel for what it does.

If you don't like what you see then modify the configuration and re-run renovate.

### major.minor.PATCH

It will automatically upgrade and merge patch verisons of docker images, npm packages, github action helpers. Anything it finds with a patch update basically!

### major.MINOR.patch

It will create a PR for any minor version updates it finds

### MAJOR.minor.patch

It will create a PR for any major version updates it finds.

- It will ignore postgres major versions in docker.
- It will create separate branches for React, NodeJS and Typescript because upgrading major versions of these breaks things usually.
