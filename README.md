# Deploy preview environment using preevy

## About Preevy

Preevy is a powerful CLI tool designed to simplify the process of creating ephemeral preview environments.
Using Preevy, you can easily provision any Docker-Compose application on AWS using affordable [Lightsail](https://aws.amazon.com/free/compute/lightsail) or [Google Cloud](https://cloud.google.com/compute/) VMs (support for more cloud providers is on the way).

Visit The full documentation here: https://preevy.dev/

## About the preevy-down action

Use this action to stop and delete a preview environment using the Preevy CLI. More information about running Preevy from CI [over here](https://preevy.dev/ci/overview#how-to-run-preevy-from-the-ci).

## Inputs

### `profile-url`

*required*: `true`

The profile url created by the CLI, [as detailed in the docs](https://preevy.dev/ci/overview#how-to-run-preevy-from-the-ci).

### `args` 

*required*: `false`

Optional additional args to the `preevy down` command, see the full reference [here](https://preevy.dev/cli-reference/#preevy-down).


## Example usage

```yaml
name: Teardown Preevy environment
on:
  pull_request:
    types:
      - closed
permissions:
  id-token: write
  contents: read
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: arn:aws:iam::12345678:role/my-role
          aws-region: eu-west-1
      - uses: actions/checkout@v3
      - uses: livecycle/preevy-down-action@latest
        id: preevy
        with:
          profile-url: "s3://preevy-12345678-my-profile?region=eu-west-1"
          docker-compose-yaml-path: "./docker/docker-compose.yaml"
```