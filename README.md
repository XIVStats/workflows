# XIVStats Shared Workflows

This is a set of shared workflows for use with GitHub actions that can be reused across the XIVStats organization and elsewhere.

For further detail on this GitHub feature, [check out the GitHub documentation on the topic](https://docs.github.com/en/actions/using-workflows/sharing-workflows-secrets-and-runners-with-your-organization), for [an example of how to invoke a shared workflow please see this example](https://github.com/XIVStats/lodestone/blob/be77fe34796988b308969f115dfe30c0164fd059/.github/workflows/build.yml#L23-L25).

## Workflow Descriptions

### Typescript NPM Module Build

#### Path

`xivstats/workflows/.github/workflows/ts-npm-build.yml@main`

#### Required Secrets

  * `CODECOV_TOKEN`: token for interfacing with the codecov code analysis platform

#### What does it do?

For each of the currently supported Node.js versions this workflow will:
1. Checkout your repo
2. Install dependencies via npm
3. Execute unit tests via `test:ci:unit` script
4. Execute integration tests via `test:ci:integration` script
5. Publish a test report
6. Execute the lint task via `lint` script
7. Execute a format check via the `format:check` script
8. Upload code coverage
9. Execute build via the `build` script

#### Requirements

This workflow assumes you have npm scripts as below specified in `package.json` at the root of your project
* `test:ci:unit`
* `test:ci:integration`
* `lint`
* `format`
* `build`

### TypeScript NPM Release

#### Path

`xivstats/workflows/.github/workflows/ts-npm-release.yml@main`

#### Required Secrets

* `GH_PUSH_TOKEN`: access token with permissions to push back to github
* `NPM_TOKEN`: token for publishing to npmjs registry

#### What does it do?

For the current LTS node version, this workflow will:
1. Checkout your repo
2. Install dependencies via npm
3. Execute build via the `build` script
4. Setup the CI's git config to behave as `ReidWeb Automation`
5. Prepare a new version of the package, generate a changelog update and publish a release on github.
6. Publish the new package verson to GitHub packages
7. Commit the changes to the changelog to a commit on a branch.
8. Push the changes to a new remote branch on GitHub.
10. Release a new version of the package to npmjs registry.
9. Submit a pull-request (which will be automatically approved).

#### Requirements

This workflow assumes you have npm scripts as below specified in `package.json` at the root of your project
* `build`

This workflow assumes you have setup the according secrets to allow for publishing and pushing.

### Auto-approve

#### Path

`xivstats/workflows/.github/workflows/autoapprove.yml@main`

#### What does it do?

This workflow automatically approves any pull requests coming from releases (as above) made by `reidweb-automation` or any pull requests coming in from dependabot.

This allows for the trivial tasks of finalizing release changelog updates and merging security dependency updates to be completely automated.
