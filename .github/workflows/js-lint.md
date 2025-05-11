# JS Lint Re-usable Workflow

A simple reusable workflow to run linting and prettier on JS projects. See [reusing workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows)

Internally it uses `npx` to run eslint and prettier. This means if your js project already has eslint and prettier (which it should) in its dependencies those versions will be used and it will respect any local configurations you have for these tools. If neither are present in the dependencies it will run against the latest versions with default settings or error if there's a problem (like it should).

### Steps:

1. Checks out the current branch (for whatever triggered the calling workflow)
2. Setups node js with the version of node specified in the `engines` field of `package.json`
3. Installs & caches dependencies with `npm ci --ignore-scripts`
4. Runs `npx eslint "**/*.js" --ignore-path .gitignore` by default.
5. Runs `npx prettier "**/*.{js,css,md,yml,json}" --check --ignore-path .gitignore` by default.

### Basic Usage:

```yaml
name: your-workflow

on:
  push:
    branches: [your-branches]

jobs:
  lint:
    uses: fena-co/actions/.github/workflows/js-lint.yml@<tag/ref>
```

### Customising

See [this example](./js-lint.example.yml) for how to specify options.

| Param | Description |
|:------|:------------|
| `lint_command` | The command to run for linting. Defaults to `npx eslint "**/*.js" --ignore-path .gitignore`. This will respect your local eslint config and ignore anything defined in your `.gitignore` e.g. `node_modules`.
| `lint_globs` | A space separated list of file globs to pass to eslint for the files you want it to run over. Defaults to `"**/*.js"` ignored if you pass your own lint command.
| `lint_args` | Additional arguments to pass to eslint. Defaults to `--ignore-path .gitignore` ignored if you pass your own lint command.
| `prettier_command` | The command to run to check the code has been beautified. Defaults to `npx prettier "**/*.{js,css,md,yml,json}" --check --ignore-path .gitignore`. This will respect your local prettier config and ignore anything defined in your `.gitignore` e.g. `node_modules`.
| `prettier_globs` | Prettier file globs to pass for the files you want it to run over (see prettier CLI docs). Defaults to "**/*.{js,css,md,yml,json}" ignored if you pass your own prettier command.
| `prettier_args` | Additional arguments to pass to prettier. Defaults to `--ignore-path .gitignore` ignored if you pass your own lint command.
