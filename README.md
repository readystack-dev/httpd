# Apache httpd: ReadyStack build kit

The offline build kit for the Docker image `readystack/httpd:2.4.71-CE-ubuntu24.04-r5`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [2.4.71-CE-ubuntu24.04-r5](https://github.com/readystack-dev/httpd/releases/tag/2.4.71-CE-ubuntu24.04-r5)
- Image: [`docker.io/readystack/httpd:2.4.71-CE-ubuntu24.04-r5`](https://hub.docker.com/r/readystack/httpd/tags), manifest digest `sha256:0fb0989c55fe2ad54206b252ac45055cbaa0c9550b529f9fa49e3fd51a201821`
- Product page: https://readystack.dev/products/httpd
- Source and build recipe: https://readystack.dev/products/httpd/recipe
- Build trail: https://readystack.dev/products/httpd/trail/2.4.71-CE-ubuntu24.04-r5
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=httpd

## The image

Single container, Apache httpd compiled from source: the event MPM's parent process in the foreground as PID 1 with its worker processes, all as the non-root user httpd. Serves files from /opt/httpd/htdocs and, with the modules you load, proxies to backends. No database, no sidecar, no state of its own.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-25 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 15 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`httpd`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-25; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/httpd.

Apache httpd is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [2.4.71-CE-ubuntu24.04-r5 release](https://github.com/readystack-dev/httpd/releases/tag/2.4.71-CE-ubuntu24.04-r5): `ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r5.tar.gz` (559.0 MB), SHA-256 `df8206c23d6ca4706f384861e4fb25eb34326402cb39f7d5f0a08dd7eb9b7b83`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r5.rebuild.json.sigstore ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r5.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r5.tar.gz
cd 2.4.71-CE-ubuntu24.04-r5
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r5.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/httpd/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
