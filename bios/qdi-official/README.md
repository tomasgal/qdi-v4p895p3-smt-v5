# QDI official BIOS image

Canonical ROM image:

[`P895V14.ROM`](P895V14.ROM)

- Source: QDI `P895V14.ZIP`
- Archive description: `V4P895/SMT BIOS version 1.4`
- Size: 65,536 bytes
- SHA-256: `64349df495153cdb4e88e8f1e042e61322d410c7167a7bd048d159adb22e2281`
- Runtime identity: `OPTi895GRN GREEN IVN1.4 22, Nov, 1995`
- POST ID: `40-2214-428003-00101111-072594-OPTI802`
- Direct archive: https://ftpmirror.infania.net/sites/ct_treiber_service/treiber/qdi/bios/p895v14.zip
- Preserved QDI BIOS index: https://ftpmirror.infania.net/sites/ct_treiber_service/html/qdi/bios/files.htm

The `GRN` internal label differs from the `P3` label in the earlier builds, but the official package targets the V4P895/SMT family, its code is very closely related to the archived V5 image, and it is independently reported working on V4P895P3/SMT V5.0.

Verify independently downloaded copies against the SHA-256 above before use.

## AmiDeco decomposition

[`decomposed/`](decomposed/) contains:

- `input.rom` — complete 65,536-byte input image, stored from the same Git blob as the canonical `P895V14.ROM` above;
- `amibody.00` — POST / setup-definition area;
- `amibody.01` — Setup Server / configuration-reporting and CPU-identification area;
- `amibody.02` — Runtime.

See [`../README.md`](../README.md), [`../../docs/bios-analysis.md`](../../docs/bios-analysis.md), and [`../../docs/reproducing-analysis.md`](../../docs/reproducing-analysis.md) for provenance, analysis, and reproduction steps.
