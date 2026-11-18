# axiom-cli — Axiom binary distribution

Public distribution channel for Axiom release bundles (productization contract §1b).
The Axiom engine itself lives at [Archright/Axiom](https://github.com/Archright/Axiom);
this repository carries the published release assets and the reusable verification
adapter.

## Releases

Every release tag `vX.Y.Z` is immutable and carries four assets:

| Asset | Purpose |
| --- | --- |
| `axiom-bundle-vX.Y.Z-<platform>.tar.gz` | the `ax` binary bundle (`ax` at bundle root) |
| `SHA256SUMS` | asset integrity (`sha256sum -c SHA256SUMS`) |
| `manifest.json` | version / sourceCommit / buildSha / binarySha256 |
| `attestation-bundle.jsonl` | offline attestation input |

The release is produced by `release-ship` in Archright/Axiom through the
`axiom-cli-release-authority` GitHub App; the tag is pinned to this repository's
`main` tip at publish time.

## Verifying a bundle (customer side)

```sh
gh release download vX.Y.Z --repo Archright/axiom-cli
sha256sum -c SHA256SUMS
gh attestation verify axiom-bundle-vX.Y.Z-<platform>.tar.gz \
  --bundle attestation-bundle.jsonl --repo Archright/Axiom --signer-repo Archright/Axiom
./ax --version
```

Only the customer's own `GITHUB_TOKEN` is needed; no Archright API access is
required for verification.

## Verification adapter

`.github/workflows/ax-verify.yml` is the reusable `workflow_call` adapter that
verifies an external project against a published bundle (checksums, attestation,
manifest binding, `ax prove`, canonical digest, engineBuild guard, zero-footprint
assertions). See the workflow header for the exact contract and exit semantics.

## License

SAL 1.0 (`LICENSE`); third-party notices in `THIRD-PARTY-NOTICES.md` and
`LICENSES/`.