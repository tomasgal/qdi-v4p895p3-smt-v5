# Original physical BIOS dump

Canonical ROM image:

[`OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin`](OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin)

- Source: physical BIOS ROM from the documented QDI V4P895P3/SMT V5.0 motherboard
- Device: Texas Instruments TMS27C512, DIP-28, 64 KiB
- Acquisition: two independent reads with XGecu T48 / Xgpro 13.16 using the `TMS27C512 @ DIP28` profile; both reads were bit-identical
- Size: 65,536 bytes
- SHA-256: `7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097`
- Runtime identity: `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994`
- POST ID: `40-2004-428003-00101111-072594-OPTI802`

## Preservation package

[`TMS27C512@DIP28-486-qdi-v4p895p3-smt-v5.zip`](TMS27C512@DIP28-486-qdi-v4p895p3-smt-v5.zip) is the package prepared for external BIOS-preservation libraries. It contains the verified physical ROM dump together with a manifest documenting the motherboard, EPROM, acquisition method, firmware identity, and checksum.

## AmiDeco decomposition

[`decomposed/`](decomposed/) contains:

- `input.rom` — complete 65,536-byte input image, stored from the same Git blob as the canonical `.bin` above;
- `amibody.00` — POST / setup-definition area;
- `amibody.01` — Setup Server / configuration-reporting and CPU-identification area;
- `amibody.02` — Runtime.

See [`../README.md`](../README.md), [`../../docs/bios-analysis.md`](../../docs/bios-analysis.md), and [`../../docs/reproducing-analysis.md`](../../docs/reproducing-analysis.md) for provenance, analysis, and reproduction steps.
