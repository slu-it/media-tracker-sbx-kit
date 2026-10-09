# media-tracker-sbx-kit

Docker Sandboxes v2 mixin kit for the **media-tracker** project. It is kept out of the
application repository so a sandboxed agent cannot modify its own sandbox definition.

Published to GitHub Packages as `ghcr.io/slu-it/media-tracker-sbx-kit:<version>`.

## Layout

```text
media-tracker-sbx-kit/
├── kit/
│   └── spec.yaml                  # the kit (schemaVersion "2", kind: mixin)
└── .github/workflows/publish.yml  # validate on PRs, push to GHCR on vX.Y.Z tags
```

## Develop and test locally

```console
$ sbx kit validate ./kit
$ cd ../media-tracker
$ sbx run claude --name kit-test \
    --kit docker.io/sbx/playwright-kit:latest \
    --kit ../media-tracker-sbx-kit/kit .
$ sbx rm kit-test
```

## Release

1. Bump `version` in `kit/spec.yaml` (for example `2.1.0`).
2. Merge to `master`, then tag and push: `git tag v2.1.0 && git push origin v2.1.0`.
3. CI checks the tag matches the spec version and runs `sbx kit push`.

Never re-push an existing version. Consumers pin exact versions.

## Use the kit (once per developer machine)

Allow the registry as a kit source (the default allows only Docker Hub). The setting
replaces the whole list, so keep Docker Hub in it:

```console
$ sbx settings set kit.allowedSources '["docker.io/","ghcr.io/slu-it/"]'
```
