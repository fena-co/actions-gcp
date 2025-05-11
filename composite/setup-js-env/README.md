# Setup JS Env Composite Action

A composite action to setup a JS environment on the CI see [composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)

Internally it uses Githubs built in `actions/checkout@v4` and `actions/setup-node@v4`. This composite action simply mimics those behaviours with defaults typical for most JS projects. Assumes bash is available in the calling environment.

### Steps:

1. Checks out the ref that triggered the workflow or the passed in `ref`.
2. Attempts to extract the node version to setup from the `engines.node` field in `package.json`. Defaults to latest long term support version if not found or the passed in `node-version`. (note: `package.json` version always takes precedence).
3. Installs node.
4. Setups up dependency caching for installed modules. Defaults to `npm` as the dependency configuration but you can also specify `yarn` or `pnpm` with a passed in `package-manager`.
5. Installs dependencies using `npm ci --ignore-scripts`. It will not use a different command if you specify a different `package-manager`. You can supply a custom install command with `install-command`.

### Outputs:

1. `node-version`: the version of node the environment was configured with.

### Basic Usage:

```yaml
name: your-workflow

on:
  push:
    branches: [your-branches]

jobs:
  some-job:
    runs-on: ubuntu-latest
    steps:
      - uses: fena-co/actions/composite/setup-js-env@<tag/ref/branch>
```

### Customising

See [this example](../../.github/workflows/setup-js-env.example.yml) for how to specify options.

| Param | Description |
|:------|:------------|
| `ref` | Proxies the Githubs actions/checkout@v4 config. The default is the ref that trigged the containing workflow.
| `fetch-depth` | Proxies the Githubs actions/checkout@v4 config.
| `submodules` | Proxies to Githubs actions/checkout@v4 config.
| `node-version` | The version of node to install. It will automatically attempt to extract the node version from the engines field in package.json and that always takes precedence. Defaults to `lts/*` (latest LTS release).
| `package-manager` | The package manager used to cache project dependencies. One of `npm \| yarn \| pnpm` . Only change this if you are using a custom install-command for these managers. Defaults to npm.
| `install-command` | Overwrite the dependency install command. Defaults to `npm ci --ignore-scripts`.
