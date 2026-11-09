# axiom-cli

Public binary distribution point for the **Axiom verification CLI** — immutable releases of the verified bundle.

> **This repository contains no source code.** The Axiom source stays in its private origin under the **Axiom Source-Available License 1.0 (SAL 1.0)**; only verified release artifacts (bundle, checksums, attestation bundle, manifest) are published here. See [`LICENSE`](LICENSE), [`LICENSES/`](LICENSES/), and [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md).

## What is distributed

Each release publishes a **version-pinned, verified bundle** for **Linux x86_64** (first distribution slice):

| Asset | Purpose |
|---|---|
| bundle `tar.gz` | `ax` binary (placed at the bundle root) + `config/spec/` + `data/verification_scope.lock`, `data/baseline.json`, `data/bug_attribution/` + root anchor `.axiom_root` |
| `SHA256SUMS` | sha256 checksums of the release assets |
| attestation bundle | GitHub release attestation, verifiable offline |
| `manifest.json` | version, source commit, build SHA, binary sha256 |

Release tags are pinned to the `axiom.nimble` version.

## Usage (contract — effective once the first release is published)

> **Status: repository setup stage — the first verification release has not been published yet.** The procedure below is the planned contract, not a live download path.
>
> **현재 상태**: 저장소 세팅 단계 — 첫 검증 릴리즈는 아직 게시되지 않았습니다. 아래 절차는 예정된 계약 서술입니다.

```bash
# 1. Download a release
gh release download vX --repo Archright/axiom-cli

# 2. Verify checksums
sha256sum -c SHA256SUMS

# 3. Verify provenance — offline bundle verification, signer pinned to the Axiom source repo
gh attestation verify <bundle> --bundle <attestation-bundle> --signer-repo Archright/Axiom

# 4. Unpack the bundle
tar -xzf <bundle>.tar.gz

# 5. Run — the bundle root carries the .axiom_root anchor
AXIOM_ROOT=<bundle-root> ax prove ...
```

Execution notes:

- Root resolution: `AXIOM_ROOT` takes priority; when unset, `ax` searches upward from its own location for the `.axiom_root`/`axiom.toml` anchor (first anchor wins). A bundle with `ax` + `.axiom_root` at its root resolves itself.
- State/seeds: set `AXIOM_STATE_DIR` when keeping state outside the bundle (seeds are looked up under `$AXIOM_STATE_DIR/data` first).
- Replay dependencies (4 public repositories) are **not** bundled — they are cloned from their public repos at SHA-pinned commits designated by the catalog.

## Runtime requirements

All dependencies are public-source tools:

| Requirement | Pinned version |
|---|---|
| Nim | 2.2.10 (harness runtime compilation — by design) |
| Z3 | 4.13.3 |
| Lean | 4.33.0 via [elan](https://github.com/leanprover/elan) |
| git | any recent |
| libpcre3 | required (loaded via `dlopen` at startup) |

Platform: **Linux x86_64** (first slice).

## Trust model

- **Immutable releases**: tags and release assets cannot be modified or deleted once published.
- Every release carries a **GitHub release attestation**; provenance is verified offline from the attestation bundle, with the signer pinned to `Archright/Axiom`.
- Customer-side verification = `SHA256SUMS` (sha256) + offline attestation-bundle verification. It requires no access to Archright repositories — only your own GitHub token for `gh`.

## Verification adapter

[`.github/workflows/ax-verify.yml`](./.github/workflows/ax-verify.yml) is a **reusable workflow** (`workflow_call` + `workflow_dispatch`) that verifies an external project with a published release bundle end-to-end on a clean `ubuntu-24.04` runner: it re-verifies the whole integrity chain (`sha256sum -c`, offline attestation, manifest binding, `ax --version`), checks out the pinned subject, provisions the pinned toolchain, runs `ax prove --proposals … --project …` and asserts Zero-Footprint (pre==post subject tree hash) plus a 64-hex `engineBuild` guard against vacuous runs. Call it from a Job in your repository (`uses: Archright/axiom-cli/.github/workflows/ax-verify.yml@<merged-main-sha>`) with `axiom-version`, `subject-repository`, `subject-commit` and a propositions artifact — a machine-readable `verify-record.json` and the full evidence (logs, state, fingerprints, toolchain observations) are returned as the `ax-verify-evidence-<source>-<label>` artifact, plus workflow outputs (`prove-exit`, `canonical-digest`, `engine-build-id`, `verdict-summary`, `zero-footprint-ok`, `runner-image`). Exit semantics: `ax prove` exit 0 (PROVED) and 1 (DISPROVED) are both in-contract completions and keep the workflow green; ≥2 fails closed.

For a `workflow_dispatch` smoke, pass the propositions document inline via `proposals-inline` (a reference example of a propositions JSON — subject pin + specId + target — is preserved in the history of the public `Archright/nim-libp2p` repository, `git show cfed3871:.github/workflows/byo-agent-verify.yml`); the first end-to-end exercise of this adapter is that post-release dispatch smoke, not a pre-publication claim.

## License

Distributed under the **Axiom Source-Available License 1.0 (SAL 1.0)** — [`LICENSE`](LICENSE) (redistribution must carry `LICENSE`, `LICENSES/`, and `THIRD-PARTY-NOTICES.md`; see SAL 1.0 §6.1).

- **Public-source verification is free** (SAL 1.0 §1.1).
- **Private-source verification requires a commercial agreement** (SAL 1.0 §1.2).
