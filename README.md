# Satis - composer repository
Used for building your own composer repository using [composer/satis](https://github.com/composer/satis).

## Inputs

| Input        | Description                                                                                             | Required                     |
|--------------|---------------------------------------------------------------------------------------------------------|------------------------------|
| token        | GitHub token that will be used for composer when invoked by satis.<br/>Currently auth is static for "github-oauth" | Yes                          |
| satis_config | Path to your satis configuration file                                                                   | No.<br/>Default = `./satis.json` |

## Outputs
None

## Usage
**Note:** Your `satis.json` **must** contain `"output-dir": ""`

Example `.github/workflow/composer-build.yaml`
```yaml
# eg. Can be used to host your internal composer package registry on github pages.
name: Composer repository

on:
  - push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: dev-this/satis-build@v1
        with:
          token: ${{ GITHUB_TOKEN }} # App/OAuth token, PAT
      - env:
          GIT_EMAIL: bot@github.com
          GIT_NAME: cool-bot
        run: |
          git config user.name $GIT_NAME
          git config user.email $GIT_EMAIL
          git add docs/
          git commit -m "Re-built repository assets"
          git push
```

Example `satis.json`
```json
{
  "name": "my-cool/repository",
  "homepage": "https://where-are-you/",
  "repositories": [
    {
      "packagist.org": false
    },
    {
      "type": "vcs",
      "url": "https://github.com/my-cool-org/the-repository"
    }
  ],
  "require-all": true,
  "output-dir": "docs"
}
```
