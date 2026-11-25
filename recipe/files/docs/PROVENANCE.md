# Provenance — Apache httpd (`2.4.71-CE-ubuntu24.04-r4`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/apache/httpd
- Revision built: `2.4.71` (commit `4554c7436049b6a68bfee7fbae32fc5ced666355`)
- Source tree: `d036c14b10e548c79c0adfc66eb5b5601b7a1b94b3e70de58cb7620e784cb2eb` over 5548 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-c`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795161067` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/httpd:2.4.71-CE-ubuntu24.04-r4` — index digest `sha256:7478ac3f1c413147a7cc2e00bd182c504bb3779c11e68decb832534b9484f597` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:84f2147f2f4ad749a3ad8f2d53aeb53db12cfcafd74c0aa7a74ac25a0cc45a78` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/httpd:2.4.71-CE-ubuntu24.04-r4`

## Pinned inputs, and why

- 21 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 1, pins in the kit's dockerfiles 5, readystack's build engine 6, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r4.trail.json`, published beside this archive and signed with the release key (`ReadyStack-httpd-2.4.71-CE-ubuntu24.04-r4.trail.json.sigstore`), and readable at https://readystack.dev/products/httpd/trail/2.4.71-CE-ubuntu24.04-r4.

