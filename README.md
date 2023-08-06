# My Renovate config

Here is the config I use for NodeJs monorepo projects with postgres database. You can adjust for your own architecture and organisation by forking this repo and modifying. Change the `extends` property to your forked repository.

## Why ????

Javascript-land moves fast! If you keep on top of package and library updates then it's not a huge problem. If you leave updates for more than a few months it becomes a commplete nightmare for your development team. 

To use in a project: 

## Create a renovate.json file

Use by extending from this e.g. create a `renovate.json` file in the root of your project with the following contents. Change the branch to run on to your main developing branch e.g. main, master or develop.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>darraghoriordan/renovate-config:base"],
  "baseBranches": ["main"]
}
```

## Trigger github actions to run renovate every morning

You can use this in your own projects by adding a renovate github action

I have an example of this here on use-miller: 

https://github.com/darraghoriordan/use-miller/blob/main/.github/workflows/rennovate.yaml

## What will it do?

### major.minor.PATCH
It will automatically upgrade and merge patch verisons of docker images, npm packages, github action helpers. Anything it finds with a patch update basically!

### major.MINOR.patch
It will create a PR for any minor version updates it finds

### MAJOR.minor.patch
It will create a PR for any major version updates it finds. 
- It will ignore postgres major versions in docker.
- It will create separate branches for React, NodeJS and Typescript because upgrading major versions of these breaks things usually.

