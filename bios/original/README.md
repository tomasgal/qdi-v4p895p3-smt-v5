# Original physical BIOS dump

Expected ROM file in this directory:

`OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin`

- Source: physical BIOS ROM from the documented QDI V4P895P3/SMT V5.0 motherboard
- Device: Texas Instruments TMS27C512, DIP-28, 64 KiB
- Acquisition: two independent reads with XGecu T48 / Xgpro 13.16 using the `TMS27C512 @ DIP28` profile; both reads were bit-identical
- Size: 65,536 bytes
- SHA-256: `7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097`
- Runtime identity: `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994`
- POST ID: `40-2004-428003-00101111-072594-OPTI802`

See [`../README.md`](../README.md) and [`../../docs/bios-analysis.md`](../../docs/bios-analysis.md) for provenance and analysis.
