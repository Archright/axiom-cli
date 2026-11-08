# Third-Party Notices

This file identifies the third-party materials included in or distributed with this repository and the licenses that continue to govern them.

The **Axiom Source-Available License 1.0** (`LicenseRef-Axiom-SAL-1.0`) applies to the original portions of the Covered Work. It does not relicense, and does not apply to, the third-party materials listed below; each such material remains governed by its own license as stated here and as reproduced under `LICENSES/`.

Where a dual license "Apache License 2.0 OR MIT" is indicated, you may use, reproduce, and distribute the material under either the Apache License 2.0 or the MIT license, at your option (the choice model used by the Status ecosystem).

## 1. ed25519 — C implementation (zlib)

- Files: `vendor/ed25519/ed25519/src/*.{c,h}`, `vendor/ed25519/ed25519/test.c`
- Upstream: <https://github.com/orlp/ed25519> (Orson Peters)
- License: **zlib** — © 2015 Orson Peters — text in `LICENSES/Zlib.txt`
- Modifications: differs from upstream master only by const-qualifier changes at two locations (`ge.c:349`, `precomp_data.h:1,46` — upstream drift); otherwise as distributed.

## 2. ed25519 — Nim bindings (MIT)

- Files: `vendor/ed25519/ed25519.nim`, `vendor/ed25519/README.md`, `vendor/ed25519/ed25519.nimble`
- Upstream: <https://github.com/niv/ed25519.nim> (niv)
- License: **MIT** (declared in `ed25519.nimble` as `license = "MIT"`; the upstream package does not ship a license text file) — text in `LICENSES/MIT.txt`
- Modifications: wrapper body is identical to upstream master; `ed25519.nimble` version field changed (0.1.1 → 1.0.0) locally.

## 3. nim-stew — verbatim file and excerpts (Apache-2.0 OR MIT)

- Files:
  - `tests/fixtures/bug_verdict_scope/base36_pinned_subject.nim` — verbatim copy of `stew/base36.nim` from nim-stew **v0.5.0** (byte-identical `diff` 0), with its original license header.
  - `tests/golden/base36_subject/stew/base36.nim` — partial excerpt of `stew/base36.nim` from nim-stew **v0.5.2** (sizing functions, 6 lines), marked "VERBATIM transcription".
  - `tests/negative/base36_mutant_subject/stew/base36.nim` — the same excerpt with an intentional seeded mutation (test fixture).
- Upstream: <https://github.com/status-im/nim-stew> — © Status Research & Development GmbH
- License: **Apache-2.0 OR MIT** — header text preserved in `tests/fixtures/bug_verdict_scope/base36_pinned_subject.nim:1-7`; full texts in `LICENSES/Apache-2.0.txt` and `LICENSES/MIT.txt`.

## 4. nim-libp2p — embedded bytesview copy (Apache-2.0 OR MIT)

- File: `tests/unit/test_libp2p_bytesview_binding.nim` (the `BvSubjectModuleBytes` constant embeds the complete 22-line `libp2p/utils/bytesview.nim`, byte-identical, sha256-verified)
- Upstream: <https://github.com/status-im/nim-libp2p> at commit `a130a7f32219909bdcda14b7f1e64d0804fc832c` (pin: `config/spec/nim_libp2p_bytesview_slice.json`)
- License: **Apache-2.0 OR MIT** — full texts in `LICENSES/Apache-2.0.txt` and `LICENSES/MIT.txt`.

## 5. Vendored build dependencies archive (Apache-2.0 OR MIT)

- File: `experiments/act2-wp-toml-b1-input-integrity/bsec/offline/deps.tar.gz`
- Contents: complete sources of three Status ecosystem packages, renamed to `.nim.vendored` (per-package file counts in the repackaged archive, including `nimblemeta.json` and the two license texts):
  - `faststreams` 0.5.2 (16 files)
  - `serialization` 0.5.4 (12 files)
  - `stew` 0.5.0 (56 files)
- Upstream: <https://github.com/status-im/faststreams>, <https://github.com/status-im/serialization>, <https://github.com/status-im/nim-stew> — © Status Research & Development GmbH
- License: **Apache-2.0 OR MIT** — the repackaged archive ships `LICENSE-APACHE` and `LICENSE-MIT` inside the archive; full texts are also reproduced in `LICENSES/Apache-2.0.txt` and `LICENSES/MIT.txt`.
- Naming note: the inserted Apache-2.0 text is named `LICENSE-APACHE` (resolving the in-file header references), while the upstream repositories name this file `LICENSE-APACHEv2`; the contents are byte-identical to the pinned upstream commits.
- Note: the archive was repackaged (license texts inserted, manifest regenerated) as part of publication preparation; the package sources themselves are unmodified.

## 6. nimcrypto — build-time dependency (MIT)

- Package: `nimcrypto` 0.7.3 (© 2018 Eugene Kabanov) — <https://github.com/cheatfate/nimcrypto>
- License: **MIT** — text in `LICENSES/MIT.txt`
- Distribution note: nimcrypto is not vendored in this repository; it is resolved and fetched by the Nim package manager at build time (pinned in the lock file). The MIT license permits such use and redistribution; this notice records the dependency and its license.

## 7. Springer open-access volume PDF (CC-BY 4.0)

- Files: `docs/spec/SVF-SVC_Software_Verification_Using_SVF.pdf` (the only copy in the current tree). A byte-identical copy under the colon-containing path `docs/spec/SVF-SVC: Software Verification Using SVF.pdf` exists only in earlier git history, before a Windows-safe rename; see the audited inventory entry #21 (history-only).
- Source: volume: **Gurfinkel & Heule (eds.), "Software Verification Using SVF (SVF-SVC)", TACAS 2025 Part III, LNCS 15698**, Springer, 2025 — <https://doi.org/10.1007/978-3-031-90660-2> (open access).
- Relevant chapter: McGowan, Richards, Sui (UNSW), "Software Verification Using SVF", pp. 254–259 — <https://doi.org/10.1007/978-3-031-90660-2_21>.
- License: **Creative Commons Attribution 4.0 International (CC-BY 4.0)** — text in `LICENSES/CC-BY-4.0.txt`
- Changes: none — the file is an unmodified copy of the open-access volume PDF.

## 8. Removed third-party documents (citation references)

Other third-party papers that previously existed as PDF files under `docs/spec/` were removed from this repository for redistribution-right reasons. Bibliographic references (DOI / arXiv / official links) for those papers are recorded in `docs/spec/PDF_REFERENCES.md`. Nothing in that file is licensed under the Axiom Source-Available License; it merely cites publicly identified works.

---

Correction and update policy: the authoritative classification of third-party materials is the audited inventory (`docs/evidence/sal10_publication/B1_third_party_inventory.md`). This notices file must be updated in step with any change to the inventory (1:1 correspondence).
