# verifiable-coldchain

Part 4 of the macula-demos OT series: the food-safety auditor arrives, takes an export of
months of freezer temperatures, and verifies the whole history offline on a laptop —
without Macula, without the station, without trusting the cold-storage operator.

**Status: planned.** Nothing is built yet. Build notes are in [CLAUDE.md](CLAUDE.md).

## The one claim this demo makes

Every reading the mesh path delivers is signed, and the history is verifiable offline: an
auditor with the export and a small standalone verifier script can prove each reading's
time, value, and provenance without touching the mesh or any Macula software. "You don't
have to trust us" is the story.

## Story

The cold-chain customer's auditor asks for the last 90 days of FZ-01 temperatures. The
operator exports the signed log; the auditor checks it on an air-gapped laptop. The
verifier confirms: the readings are continuous (no gaps), each is signed by the gateway's
key, and a tampered export is detected on the spot.

## Planned layout

```text
apps/
  freezer_controller/   From part 1 (sim + audit)
  ot_gateway/           From part 1 + signed reading log (append-only, hash-chained)
  admin_ui/             From part 1 + export view
verify/                 Standalone verifier script (no Macula dependencies) + tamper probes
deploy/                 compose.yml, station/config.json
docs/                   DEMO_SCRIPT.md, WHAT_THIS_PROVES.md
scripts/                mesh-demo.sh, export.sh, tamper.sh, reset
assets/                 demo.mp4, demo-poster.png (made when the video is asked for)
```

## The series

| # | Repo | One claim |
|---|------|-----------|
| 1 | [freezers-on-mesh](https://github.com/macula-demos/freezers-on-mesh) | A retrofit removes the inbound path and moves authority to the mesh |
| 2 | [warehouses-on-mesh](https://github.com/macula-demos/warehouses-on-mesh) | Federation: sealed cross-site calls with no inbound ports anywhere |
| 3 | [pumps-on-mesh](https://github.com/macula-demos/pumps-on-mesh) | The same gateway pattern behind a real Modbus TCP controller |
| 4 | **verifiable-coldchain** | Signed reading history an auditor verifies without Macula |
| 5 | [two-person-rule](https://github.com/macula-demos/two-person-rule) | Dual control with dynamic enrollment and revocation |

## License

Apache-2.0. See [LICENSE](LICENSE).
