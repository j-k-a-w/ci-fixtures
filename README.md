# ci-fixtures

Prebuilt **MongoDB Community Server** binaries, mirrored as GitHub Release
assets for use as continuous-integration and sandbox fixtures.

## Why this exists

Test and sandbox tooling often needs a real `mongod` binary at runtime. A common
way to get one is to download it on demand from MongoDB's official distribution
host. Some CI runners and ephemeral cloud sandboxes cannot reach that host
(network egress is restricted), but they can reach GitHub.

This repository bridges that gap. Each release mirrors an official, unmodified
MongoDB build as a downloadable asset, so any environment that can reach GitHub
can obtain a `mongod` binary without depending on MongoDB's CDN.

## What's published

Binaries are attached to **GitHub Releases**, one release per MongoDB version,
tagged `v<version>`. Nothing binary is committed to the git tree itself; the
assets live on the releases.

| Release  | Asset                                       | Platform                  | Size    |
| -------- | ------------------------------------------- | ------------------------- | ------- |
| `v7.0.3` | `mongodb-linux-x86_64-ubuntu2204-7.0.3.tgz` | Linux x86_64 (Ubuntu 22.04) | ~81 MiB |

Each asset is the standard MongoDB Community Server archive as published by
MongoDB, repackaged unchanged. The contents (the `mongod` executable and its
companion files) are byte-for-byte the official build.

## How it's intended to be used

The assets are ordinary MongoDB tarballs, so any tool or script that can take a
download URL for a `mongod` archive can point at a release asset here instead of
the official host. The typical pattern is a binary-provisioning tool (for
example, an in-memory MongoDB test harness) configured with an override download
URL for the matching version. Beyond supplying the URL, no special handling is
required.

Asset URLs follow GitHub's standard release-download form:

```
https://github.com/j-k-a-w/ci-fixtures/releases/download/v<version>/<asset-name>
```

## Integrity

Release assets are served over HTTPS from this public repository. GitHub records
a SHA-256 digest for each asset, which can be verified after download. For
`v7.0.3`:

```
sha256  5a69e72aaba0b35dc091801240513dbae6ac7d632d49e1fd3005391cc98c1919
        mongodb-linux-x86_64-ubuntu2204-7.0.3.tgz
```

Note that GitHub release assets do not carry the `<archive>.md5` sidecar file
that MongoDB's official host provides, so tools expecting that sidecar should
have their MD5 check disabled and rely on the SHA-256 digest above instead.

## Adding a version or platform

1. Download the official Community Server archive for the target version and
   platform from MongoDB.
2. Create a release tagged `v<version>` and attach the archive unchanged, keeping
   MongoDB's original filename.
3. Record the platform and SHA-256 digest in the table above.

Only versions and platforms that consumers actually need are published, so the
set of releases is deliberately small.

## Provenance and licensing

These binaries are official MongoDB Community Server builds, redistributed
unmodified for convenience. They are not produced or endorsed by MongoDB, Inc.
MongoDB Community Server is distributed under the Server Side Public License
(SSPL); the upstream licensing terms apply to the binaries in these releases.
See <https://www.mongodb.com/community/licensing> for details.
