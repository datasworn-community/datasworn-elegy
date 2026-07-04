# Publishing

This repository includes three workflow callers:

- `.github/workflows/build.yml`
- `.github/workflows/release.yml`
- `.github/workflows/experimental-release.yml`

They call the shared content workflows from:

<https://github.com/datasworn-community/.github>

The release jobs use the shared Datasworn Community content workflows.

## Stable Releases

Stable releases run on pushes to `main`, or manually through the Release
workflow. The workflow builds generated package artifacts, compares them against
the latest published package on the same schema line, and publishes only changed
packages.

Package versions use the Datasworn schema line:

```text
0.2.0
```

For schema line `0.2`, content packages publish as `0.2.x`.

## Experimental Releases

Apply the `release_experimental` label to a pull request to publish a canary.

The experimental workflow also listens for:

- label removal;
- pull request close;
- pull request merge.

Those events let the shared workflow clean up canary dist-tags.

## npm Setup

Before the first stable publish:

1. Make sure `packageName` in `datasworn.config.yaml` belongs to the publishing scope.
2. Configure publishing credentials for this repository.

The Datasworn Community org is moving toward npm trusted publishing with GitHub
Actions provenance. If you use your own scope, configure npm trusted publishing
for this repository and workflow, or provide a scoped `NPM_TOKEN` secret.

For GitHub Actions trusted publishing, register the caller workflow file in npm:

```text
.github/workflows/release.yml
```

## Before Publishing

Run:

```sh
bun install
bun run build
bun run validate
```

Inspect `dist/packages/elegy/package.json` and confirm:

- `name` is correct;
- `version` is on the schema line;
- `repository.url` points at this repository;
- `license` is correct;
- `private` is absent or false.
