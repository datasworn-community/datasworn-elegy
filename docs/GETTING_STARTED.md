# Getting Started

This checklist covers local Elegy content builds.

## 1. Install Tools

Install Bun 1.3 or newer:

```sh
bun --version
```

Then install dependencies:

```sh
bun install
```

## 2. Check Package Metadata

The package metadata lives in `datasworn.config.yaml`:

- `repository.url`: your repository URL.
- `packages[0].id`: the Datasworn package ID, currently `elegy`.
- `packages[0].source`: the matching source directory, currently
  `source_data/elegy`.
- `packages[0].packageName`: the npm package name.
- `packages[0].description`: your package description.
- `packages[0].license`: your content license.

Keep package IDs lowercase with underscores, such as `elegy`. Use npm package
names with hyphens, such as `@datasworn-community/elegy`.

## 3. Choose Dependencies

Elegy references IDs from both Ironsworn Classic and Starforged:

```yaml
dependencies:
  - classic
  - starforged
publishDependencies:
  - id: classic
    packageName: "@datasworn-community/ironsworn-classic"
    schemaLine: "0.2"
  - id: starforged
    packageName: "@datasworn-community/starforged"
    schemaLine: "0.2"
```

Keep both official packages in `devDependencies` so local builds can validate
cross-package references.

## 4. Build and Validate

```sh
bun run build
bun run validate
```

Commit the regenerated `generated-datasworn/` files with your source changes.
Those files are the raw JSON surface for users who browse or download content
directly from GitHub.

The build fails if:

- source YAML is invalid;
- referenced Datasworn IDs cannot be resolved;
- package versions do not match the configured schema line;
- required package metadata, such as `repository`, is missing.

## 5. Prepare to Publish

When the package is ready:

1. Confirm `packageName` is the package you intend to publish.
2. Confirm `repository.url` points at this repository.
3. Confirm the license and provenance docs are accurate.
4. Read [Publishing](PUBLISHING.md).
