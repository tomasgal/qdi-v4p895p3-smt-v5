# BIOS files

This directory is organized by provenance so the three 64 KiB ROM images can be preserved and verified independently.

```text
bios/
├── original/
│   ├── README.md
│   ├── OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin
│   ├── TMS27C512@DIP28-486-qdi-v4p895p3-smt-v5.zip
│   └── decomposed/
│       ├── input.rom
│       ├── amibody.00
│       ├── amibody.01
│       └── amibody.02
├── archive/
│   ├── README.md
│   ├── qdi_v4p895p3_smt_v5_bios.bin
│   └── decomposed/
│       ├── input.rom
│       ├── amibody.00
│       ├── amibody.01
│       └── amibody.02
└── qdi-official/
    ├── README.md
    ├── P895V14.ROM
    └── decomposed/
        ├── input.rom
        ├── amibody.00
        ├── amibody.01
        └── amibody.02
```

All three canonical ROM images are now materialized in the repository. In each directory, the canonical raw ROM and `decomposed/input.rom` reference the same Git blob and are therefore byte-for-byte identical. The `amibody.*` files are the AmiDeco 0.31e extraction artifacts used for the documented comparison.

## Canonical images

### [`original/OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin`](original/OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin)

Verified physical dump from the motherboard documented in this repository.

```text
OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994
40-2004-428003-00101111-072594-OPTI802
```

The original chip is a TI TMS27C512 with AMI sticker `AC0410599`. It was read twice using an XGecu T48 / Xgpro 13.16 with the `TMS27C512 @ DIP28` profile. Both reads were 65,536 bytes and bit-identical.

The external-submission package [`TMS27C512@DIP28-486-qdi-v4p895p3-smt-v5.zip`](original/TMS27C512@DIP28-486-qdi-v4p895p3-smt-v5.zip) contains this verified dump together with a manifest describing its provenance, acquisition, firmware identity, and checksum.

See [`original/decomposed/`](original/decomposed/) for the AmiDeco input copy and extracted modules.

### [`archive/qdi_v4p895p3_smt_v5_bios.bin`](archive/qdi_v4p895p3_smt_v5_bios.bin)

The filename is intentionally the same as the file published in the VOGONS Vintage Driver Library under the title **QDI V4P895P3 SMT v5 AMIBIOS WINBIOS**.

Download source: [VOGONS Vintage Driver Library landing page](https://vogonsdrivers.com/getfile.php?fileid=1699&menustate=0).

```text
OPTi895P3 GREEN PC IVN5.2 27 May, 1995
40-2207-428003-00101111-072594-OPTI802
```

See [`archive/decomposed/`](archive/decomposed/) for the AmiDeco input copy and extracted modules.

### [`qdi-official/P895V14.ROM`](qdi-official/P895V14.ROM)

The ROM from QDI's `P895V14.ZIP`. The preserved QDI archive describes the package as:

```text
V4P895/SMT BIOS version 1.4
```

Download sources:

- [direct `P895V14.ZIP`](https://ftpmirror.infania.net/sites/ct_treiber_service/treiber/qdi/bios/p895v14.zip)
- [preserved QDI BIOS archive index](https://ftpmirror.infania.net/sites/ct_treiber_service/html/qdi/bios/files.htm)

```text
OPTi895GRN GREEN IVN1.4 22, Nov, 1995
40-2214-428003-00101111-072594-OPTI802
```

The `GRN` internal label differs from the `P3` label in the earlier builds, but the official package targets the V4P895/SMT family, its code is very closely related to the archived V5 image, and it is independently reported working on V4P895P3/SMT V5.0.

See [`qdi-official/decomposed/`](qdi-official/decomposed/) for the AmiDeco input copy and extracted modules.

The external sources above were reachable when checked on **2026-09-13**. Verify independently downloaded copies against [`../CHECKSUMS.sha256`](../CHECKSUMS.sha256) before use.

## Analysis

See [`../docs/bios-analysis.md`](../docs/bios-analysis.md) for module hashes, similarity measurements, CPU-identification evidence, and interpretation, and [`../docs/reproducing-analysis.md`](../docs/reproducing-analysis.md) for reproducible extraction commands.

## Checksums

See [`../CHECKSUMS.sha256`](../CHECKSUMS.sha256).
