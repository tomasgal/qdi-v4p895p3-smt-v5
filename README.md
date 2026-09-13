# QDI V4P895P3/SMT V5.0

Technical preservation archive for the **QDI V4P895P3/SMT V5.0** Socket 3 / VLB 486 motherboard based on the OPTi 895 chipset family.

This repository focuses on firmware preservation, board identification, CPU-support evolution, and practical ROM-chip compatibility. It contains three 64 KiB AMI WinBIOS images from the same firmware family, including a verified physical dump from the board documented here.

## Board identity

The physical board is marked **`V4P895P3/SMT V5.0`**. Its layout and firmware family match the QDI / Legend documentation for the V4P895P3/SMT family.

The original installed BIOS EPROM carries an AMI sticker:

- `AMIBIOS`
- `AMERICAN MEGATRENDS`
- `486DX ISA BIOS`
- `(C) 1993`
- `AC0410599`

The underlying device was identified by programmer signature as a **Texas Instruments TMS27C512**, a 64 KiB (512 kbit) DIP-28 EPROM.

## Preserved BIOS images

| Repository file | Provenance | Internal runtime identity | Internal POST ID | SHA-256 |
|---|---|---|---|---|
| `bios/original/OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin` | Two independent reads of the physical TMS27C512; bit-identical | `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994` | `40-2004-428003-00101111-072594-OPTI802` | `7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097` |
| `bios/archive/qdi_v4p895p3_smt_v5_bios.bin` | Existing VOGONS Vintage Driver Library image; filename preserved | `OPTi895P3 GREEN PC IVN5.2 27 May, 1995` | `40-2207-428003-00101111-072594-OPTI802` | `d188089f4f069b78027db0d180c046899d38fc29dca38813ad9c935a8c7d6b17` |
| `bios/qdi-official/P895V14.ROM` | QDI `P895V14.ZIP`, described in the QDI archive as `V4P895/SMT BIOS version 1.4` | `OPTi895GRN GREEN IVN1.4 22, Nov, 1995` | `40-2214-428003-00101111-072594-OPTI802` | `64349df495153cdb4e88e8f1e042e61322d410c7167a7bd048d159adb22e2281` |

All three ROMs are **65,536 bytes** and use the AMI core dated **25 July 1994**. The later vendor builds retain that AMI core date while QDI/OPTi-specific code and data continue to evolve.

The original board dump is **not** identical to either previously archived image.

### External download sources

Until all three binary images are materialized in this repository, the two externally preserved ROMs can be obtained from their preservation sources:

- **VOGONS IVN5.2** — [VOGONS Vintage Driver Library landing page](https://vogonsdrivers.com/getfile.php?fileid=1699&menustate=0) for `qdi_v4p895p3_smt_v5_bios.bin`. VOGONS asks users to link/bookmark the landing page rather than a transient direct-file URL.
- **QDI P895V14** — [direct `P895V14.ZIP` archive](https://ftpmirror.infania.net/sites/ct_treiber_service/treiber/qdi/bios/p895v14.zip); the [preserved QDI BIOS index](https://ftpmirror.infania.net/sites/ct_treiber_service/html/qdi/bios/files.htm) identifies it as `V4P895/SMT BIOS version 1.4`.

Both sources were reachable when checked on **2026-09-13**. Verify the downloaded payload against the SHA-256 values above before use.

## Main firmware finding

AmiDeco 0.31e recognizes all three files as **AMI'94** and extracts a three-section scheme: POST, Setup Server, and Runtime.

The firmware progression is particularly visible in CPU-identification strings:

- the physical IVN5.0 dump contains `P24D`, `P24T`, and `486DX4`;
- the VOGONS IVN5.2 image adds `AuthenticAMD`, `Am486DX4`, and `486DX4-Plus`-class identification;
- QDI P895V14 adds explicit **`5x86`** and **`5x86-P75`** strings in addition to `AuthenticAMD` and `Am486DX4`.

`5x86-P75` is especially relevant to AMD Am5x86-133 support. String evidence alone proves recognition/setup support rather than every low-level initialization path, but an independent VOGONS report documents P895V14 successfully enabling an AMD 5x86-133 on this exact motherboard model/revision.

See **[docs/bios-analysis.md](docs/bios-analysis.md)** for module hashes, similarity measurements, strings, and interpretation.

## ROM-chip options

The original ROM is a **TMS27C512** UV-erasable EPROM. For repeated firmware experiments, a **Winbond W27C512** in DIP-28 is a convenient electrically erasable replacement with a compatible read interface. A `W27C512-45Z` is suitable; a faster access-time rating does not make the ROM “too fast” for the motherboard.

See **[docs/rom-chips.md](docs/rom-chips.md)** before substituting devices.

## Repository layout

```text
bios/
  original/       verified physical dump
  archive/        previously published community dump
  qdi-official/   QDI P895V14 release ROM
docs/
  bios-analysis.md
  board-identification.md
  rom-chips.md
  reproducing-analysis.md
photos/
CHECKSUMS.sha256
```

The temporary analysis files produced by AmiDeco (`amibody.*`, shell reports, and grep output) are intentionally not stored. The relevant results are documented and the extraction procedure is reproducible.

## External preservation references

- VOGONS Vintage Driver Library, existing V5 image:
  https://vogonsdrivers.com/getfile.php?fileid=1699&menustate=0
- QDI BIOS archive mirror, direct package (`P895V14.ZIP` = `V4P895/SMT BIOS version 1.4`):
  https://ftpmirror.infania.net/sites/ct_treiber_service/treiber/qdi/bios/p895v14.zip
- QDI BIOS archive index:
  https://ftpmirror.infania.net/sites/ct_treiber_service/html/qdi/bios/files.htm
- VOGONS thread documenting P895V14 on V4P895P3/SMT V5.0 with AMD 5x86-133:
  https://www.vogons.org/viewtopic.php?t=74403
- Board/manual archive:
  https://www.elhvb.com/webhq/models/486vlb3/v4p895v5.htm

## Preservation note

The firmware binaries remain copyrighted works of their respective original rights holders and are included here for historical preservation, hardware restoration, interoperability, and research. Keep a verified backup of the original ROM before experimenting with replacement firmware.
