# CLAUDE.md — build notes for verifiable-coldchain

Part 4 of the macula-demos OT series. Build after parts 2 and 3 are done; it reuses part 1's
freezer sim and gateway.

## The one claim

Signed reading history that an auditor verifies offline, without Macula. Each reading in the
export carries its time, value and signer, and the verifier script proves them without any
mesh, station, or Macula software.

## Open decisions (resolve before/at start of build)

1. **Signature format**: the verifier must NOT need Macula. Use a standard, widely available
   primitive (e.g. Ed25519 via a common library/CLI) rather than Macula's own scheme, so the
   verifier dependency stays tiny and standard.
2. **Log structure**: append-only, hash-chained (each entry chains the previous) so one
   signature over the head covers the whole history. Decide the export format (JSONL plus a
   detached signature, or a single signed bundle) and the "no gaps" check.
3. **Key handling**: which key signs (the gateway's own key, or a dedicated signing key),
   where it lives, and how the auditor learns the public key out of band — shown in the demo
   as a short "key delivery" beat, since the export alone is not self-authenticating.
4. **Tamper probes**: the demo must show at least one forged or modified export being
   detected by the verifier.
5. **Scope honesty**: this proves offline verification of the signed log, NOT archival
   completeness, sensor-level integrity, or anything about the controller's own audit.
   Part 1's boundary discipline applies.

## Carry-over rules (from part 1)

Same fixture caveats, boot-time operator allowlist, in-memory command guard, sealed payload
vs visible routing metadata, honest `WHAT_THIS_PROVES.md` — all carry over unchanged.

## Do not

- Do not weaken or change the mesh-side policies (sealing, callers, command guard).
- Do not let the verifier depend on Macula, the station, or the mesh — offline-only is the
  claim. A standard crypto library is fine; `macula_*` code is not.
- Do not touch the sibling repos.
