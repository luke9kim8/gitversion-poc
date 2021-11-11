# Incrementing Semantic Versioning with GitVersion
Proof of concept for packaging repo following semver using gitversion action

## What is Gitversion
GitVersion is a CLI tool that helps you to automatically increment version number following the semantic versioning practice.

## What is GitFlow
In GitFlow, we have `main` as the release branch and `dev` as test/development branch. In GitFlow, developers branch off from the `dev` branch following the name convention `feature/something-blahblah`, `patch/blah-blah`,`bug/something`, etc. Once the branch is complete, devs merge their branches to `dev` and susequently to `main`. 

## What is semantic versioning and why we need it
Semantic version follows this pattern.
```
{major}.{minor}.{patch}+SNAPSHOT...
```
We need semantic versioning because it helps end users and developers to keep track of changes of released software. Semantic versioning indicated how drastic the changes were from the last release.

## Setting up gitversion in GitHub Actions
First, you need to set up a GitVersion.yml file. If you have `gitversion` installed, you can run `gitversion init` in your local and push to remote. The CLI will prompt you with several messages. I suggest reading this article that explains what options to chose. For my project's use case, I went with option 3, which sasy `each merged branch against master will increment the version (mainline mode)`. Afterwards, you should have `GitVersion.yml` in your remote repo.

Second, you need to create a GitHub Actions Workflow. In the workflow, you need to check out repository, install gitversion, and determine the version (template from [DirtySoc](https://github.com/DirtySoc/learn-go-with-tests/blob/master/.github/workflows/github-actions-demo.yml)). 

Finally, you need to include a step in the workflow that will package the repo. This could be uploading to package manager or just zipping it.

## Incrementing version number using gitversion
The goal is to build and increment version number whenever something is pushed to `main` or `develop`. We can increment major, minor, or patch number by including `+semver: major`, `+semver: minor`, and `+semver: patch`. This message can be included anywhere in the commit message. The commit can be anywhere in the commit chain, meaning you can add `+semver: minor` in the commit message for `feature/something` and merging this to dev and then to main will increment the minor number
