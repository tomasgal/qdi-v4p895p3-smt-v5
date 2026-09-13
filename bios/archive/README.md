# Archived community BIOS image

Canonical ROM image:

[`qdi_v4p895p3_smt_v5_bios.bin`](qdi_v4p895p3_smt_v5_bios.bin)

- Source: VOGONS Vintage Driver Library
- Size: 65,536 bytes
- SHA-256: `d188089f4f069b78027db0d180c046899d38fc29dca38813ad9c935a8c7d6b17`
- Runtime identity: `OPTi895P3 GREEN PC IVN5.2 27 May, 1995`
- POST ID: `40-2207-428003-00101111-072594-OPTI802`
- Download: https://vogonsdrivers.com/getfile.php?fileid=1699&menustate=0

The repository copy preserves the filename used by the VOGONS Vintage Driver Library. Verify independently downloaded copies against the SHA-256 above before use.

## AmiDeco decomposition

[`decomposed/`](decomposed/) contains:

- `input.rom` — complete 65,536-byte input image, stored from the same Git blob as the canonical `.bin` above;
- `amibody.00` — POST / setup-definition area;
- `amibody.01` — Setup Server / configuration-reporting and CPU-identification area;
- `amibody.02` — Runtime.

See [`../README.md`](../README.md), [`../../docs/bios-analysis.md`](../../docs/bios-analysis.md), and [`../../docs/reproducing-analysis.md`](../../docs/reproducing-analysis.md) for provenance, analysis, and reproduction steps.
