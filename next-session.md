# Next session — nn-wfc-patcher-wii

Updated 2026-09-24.

The Wii's client side: WiiLink's `wfc-patcher-wii` rebuilt for openpak.org
(same-length swap for `nintendowifi.net`, so every patched string still
fits). Launcher, gecko codes and RSA-signed payloads, released on
`openpak-v*`; never run on hardware.

Current status 2026-09-24: latest OpenPak tag still `openpak-v0.1.1`
(6fb683b). Since then only CI (release now triggers on `v*.*.*` tags, not
`openpak-v*`) and docs commits.

## Where things stand

- Last release `openpak-v0.1.1` (2026-09-10); no code since, only CI + docs.
  The next release needs a `v*.*.*` tag (ee7bce6).
- Artifacts per release:
  - `openpak-wfc-launcher.dol` — the setup-file launcher (counterpart of
    the Switch's `openpak.nro`); boots a disc with stage 0 applied
  - `openpak-wfc-gecko-codes.zip` — per-game stage-0 codes for USB loaders
    and Riivolution
  - `wfc-payload-openpak.tar.gz` — stage 1 + stage 2; unpack into
    nn-wfc's `payload/`. Stage 2 is RSA-signed: public half committed in
    `include/wwfcPayloadPublicKey.hpp`, private half in the repo secret
    (`WFC_PAYLOAD_PRIVATE_KEY`). The first OpenPak commit (2026-09-10)
    leaked that key; rotated within the minute — PRD §5.
- The Wii still resolves `*.openpak.org` through DNS: point it at
  ../nn-sssl-dns or publish records.
- 2026-09-15 docs pass committed: `CHANGELOG.md`, `docs/`, `prds/` stubs.

## Next steps

1. Verify the release payloads are unpacked into the deployed nn-wfc's
   `payload/` directory.
2. Hardware pass: launcher or gecko code on a real Wii, one game through
   NAS — C10 observed, not inferred.
3. Key discipline: the signing key is a single point of failure for every
   Wii client; no second copy anywhere.

## Pointers

- README (OpenPak header), make.sh, patch/ stage1/ payload/ READMEs
- ../prds/platform-wii-ds-prd.md §5 risks; ../nn-wfc (server),
  ../nn-sssl-dns (DNS)

## Scratch (research and throwaway work)

Decompiles, Ghidra projects, dumps, exefs/romfs extracts, packet captures,
strace and emulator logs, probe harnesses: put them in
`~/REPOS/Openpak/scratch/<topic>`. That folder is a local mount of the media pool,
outside every repository, so nothing in it is committed. Never use `/tmp` (a
shared 15 GB RAM disk) or elsewhere on `/home` for this. Keys and signing
material never go there. Rule: `docs/playbooks/conventions.md` in the workspace.
