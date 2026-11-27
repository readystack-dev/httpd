[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Fhttpd-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/httpd) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/httpd)](https://hub.docker.com/r/readystack/httpd) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/httpd) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

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

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/httpd/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
