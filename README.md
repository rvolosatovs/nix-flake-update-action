# nix-flake-update-action

This action will:
1. Generate a GitHub App token using https://github.com/actions/create-github-app-token (refer to https://github.com/peter-evans/create-pull-request/blob/d7c27ba1b126e97b7c653dfbca2bd0aa018ba7a4/docs/concepts-guidelines.md#authenticating-with-github-app-generated-tokens for setup instructions)
2. Checkout `flake.nix` and `flake.lock` in the repository using https://github.com/actions/checkout
3. Install Nix using https://github.com/DeterminateSystems/nix-installer-action
4. Update the flake by running `nix flake update` in bash shell
5. Create a pull request using https://github.com/peter-evans/create-pull-request and a token generated in (1.)

Refer to https://github.com/peter-evans/create-pull-request for more thorough documentation - action parameters are almost entirely duplicated from these two actions and passed through verbatim.

# Example

```yml
name: nix-flake-update

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

jobs:
  nix-flake-update:
    runs-on: ubuntu-latest
    steps:
      - uses: rvolosatovs/nix-flake-update-action@v1
        with:
          app-id: ${{ secrets.BOT_APP_ID }}
          private-key: ${{ secrets.BOT_APP_PRIVATE_KEY }}
          assignees: rvolosatovs
          delete-branch: true
          reviewers: rvolosatovs
          signoff: true
```
