# actions
- Testing diferent Github actions workflows for automatic release using Semantic-release

# workflow badge
![CI](https://github.com/albertvila/actions/workflows/CI/badge.svg)
![Release](https://github.com/albertvila/actions/workflows/Release/badge.svg)
![Lint PR](https://github.com/albertvila/actions/workflows/Lint%20PR/badge.svg)

# How to configure it
- Add a Lint PR workflow to check the PR title contains a mandatory flag (feat, fix, ...)
- It can work with protected branches, but then you need a personal access token or a Github App
- You need to configure the repository settings with the merge option 'Squash all commits'
- Install an extension or use Redefined Github with the properly sync-pr-commit-title enabled

- If I do not find a way to modify the "Merge pull request #.." automatic commit log we need to remove the Squash Commit options on the repo in order to work (then not sure the previous Lint PR workflow has sense)
    - Using the redefined github to change the PR commit message (but does not work), if not there is a chrome extension

## Protected branches

## Using a Peronal Access Token
- Generate a new Personal Access token
- While checking out code add the persist-credential configuration
```
      - uses: actions/checkout@v2
        with:
          persist-credentials: false
```
- Add the token into your Settings > Secrets section
- Configure the Semantic Release action to use your previous generated token

## Use a Github App (recomended)
- Create a new Github App under your setttings
- Grant at least the following rights
    Read/Write on the Administration part (to overwrite the protected branches rules)
    Read/Write on the Contents part (to be able to push)
    Read/Write on the Issues part
    Read on the Metadata
- Install the app on your acount and configure it for all repos or just a few ones
- Download the pem file and transform it using the command
`cat private-key.pem | base64 -w 0 && echo`
- Add 2 secrets on your Settings > Secrets section, the AUTOMATION_APP_ID with the id for your newly generated app and the AUTOMATION_APP_SECRET with the content of the base64 command
- Change the workflow to use the previous secrets with
```
      - name: Generate token
        id: generate_token
        uses: tibdex/github-app-token@v1.0.2
        with:
          app_id: ${{ secrets.AUTOMATION_APP_ID }}
          private_key: ${{ secrets.AUTOMATION_APP_SECRET }}

      - name: Semantic Releaser
        uses: cycjimmy/semantic-release-action@v2.5.3
        with:
          extra_plugins: |
            @semantic-release/changelog
            @semantic-release/git
        env:
          GITHUB_TOKEN: ${{ steps.generate_token.outputs.token }}
```
