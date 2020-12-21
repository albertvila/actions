# actions
- Testing diferent Github actions workflows for automatic release using Semantic-release

# workflow badge
![CI](https://github.com/albertvila/actions/workflows/CI/badge.svg)
![Release](https://github.com/albertvila/actions/workflows/Release/badge.svg)
![Lint PR](https://github.com/albertvila/actions/workflows/Lint%20PR/badge.svg)

# How to configure it
- Add a Lint PR workflow to check the PR title contains a mandatory flag (feat, fix, ...)
- It can work with protected branches, but then you need a personal access token
- You need to configure the repository settings with the merge option 'Squash all commits'
- Install an extension or use Redefined Github with the properly sync-pr-commit-title enabled

- If I do not find a way to modify the "Merge pull request #.." automatic commit log we need to remove the Squash Commit options on the repo in order to work (then not sure the previous Lint PR workflow has sense)
    - Using the redefined github to change the PR commit message (but does not work), if not there is a chrome extension
