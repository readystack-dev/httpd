# Apache httpd 2.4.71 — ReadyStack release `2.4.71-CE-ubuntu24.04-r4`

Generated 2026-11-20 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/httpd:2.4.71-CE-ubuntu24.04-r4` (linux/amd64). Index digest `sha256:7478ac3f1c413147a7cc2e00bd182c504bb3779c11e68decb832534b9484f597` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:84f2147f2f4ad749a3ad8f2d53aeb53db12cfcafd74c0aa7a74ac25a0cc45a78` [`TARGET_DIGEST.txt`].
- **Upstream:** Apache httpd 2.4.71 built from source at `2.4.71` of https://github.com/apache/httpd — upstream notes: https://github.com/apache/httpd/releases/tag/2.4.71 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `httpd`.
- **Build:** `source-rebuild-c`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `2.4.71-CE-ubuntu24.04-r3`.

## What changed in this release

- Upstream version unchanged (2.4.71); ReadyStack rebuild.
- Apache httpd 2.4.71 is unchanged from the release this supersedes (2.4.71-CE-ubuntu24.04-r3); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:9c7f200c07e3…), which its maintainers updated after 2.4.71-CE-ubuntu24.04-r3 was built on sha256:2198e4c65076…. Settings, ports and data volumes are those of 2.4.71-CE-ubuntu24.04-r3: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `httpd` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/httpd:2.4.71-CE-ubuntu24.04-r4` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** httpd runs as a non-root user on port 8080; it serves upstream's default page until you mount your own site.
- **Before exposing this service:** mod_ssl is built but not loaded: the shipped configuration serves plain HTTP only.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:84f2147f2f4ad749a3ad8f2d53aeb53db12cfcafd74c0aa7a74ac25a0cc45a78`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 2 of 2 use cases passed (2 authored) against the running image [`docs/validation/test-results.json`].
- The cases are `GET /` (200), `GET /index.html` (200); the suite checks nothing else.

## Known issues

- None found (reviewed by held semantic review (Claude) on 2026-11-20).

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/httpd:2.4.71-CE-ubuntu24.04-r4
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

