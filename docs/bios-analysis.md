# AMI BIOS analysis

## Method

All three 65,536-byte images were analyzed with **AmiDeco 0.31e (Linux)**.

For each image:

```text
AMI'95 hook not found..Turning to AMI'94

AMIBIOS information:
Released        : 25 July 1994

This Scheme Usually Contains:
        POST
        Setup Server
        Runtime
Total Sections  : 3
```

AmiDeco extracted three files named `amibody.00`, `amibody.01`, and `amibody.02`. This documentation treats them in AmiDeco's reported order as:

- `.00` — POST / setup-definition area
- `.01` — Setup Server / configuration-reporting and CPU-identification area
- `.02` — Runtime

The temporary extracted files are not stored in the repository; hashes and reproducible extraction commands are provided below.

## Image identities

| Name in this repository | Raw size | Raw SHA-256 | Vendor Runtime string |
|---|---:|---|---|
| `OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin` | 65,536 | `7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097` | `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994` |
| `qdi_v4p895p3_smt_v5_bios.bin` | 65,536 | `d188089f4f069b78027db0d180c046899d38fc29dca38813ad9c935a8c7d6b17` | `OPTi895P3 GREEN PC IVN5.2 27 May, 1995` |
| `P895V14.ROM` | 65,536 | `64349df495153cdb4e88e8f1e042e61322d410c7167a7bd048d159adb22e2281` | `OPTi895GRN GREEN IVN1.4 22, Nov, 1995` |

All three contain the AMI core date `07/25/94`. This is the AMI core date, not the final QDI vendor-build date.

## Extracted module sizes and hashes

| Firmware | Module | Size | SHA-256 |
|---|---|---:|---|
| Physical IVN5.0 | `.00` | 26,504 | `d9804d3228e5b1da715a2ee9a5feaf5afea8b3247964df6c6c1eb7e533949ff6` |
| Physical IVN5.0 | `.01` | 17,974 | `cc6d30dcc2e9a13e1a3adc58be7753babae08936f109732c2c38dfb3882c090c` |
| Physical IVN5.0 | `.02` | 32,768 | `0173f9e283add1ca4e47c3a8a32c7cd75044da91e358a379888e26b2e56be59a` |
| VOGONS IVN5.2 | `.00` | 26,166 | `086be0e6c36355b7aca344ebbd42d6126de835abf1fad9c346d69e77a4b3d28a` |
| VOGONS IVN5.2 | `.01` | 18,128 | `00b90a8f3bd7c7338a7a8925d012a2b57529bf4ab61765dbc71d017a091aae8a` |
| VOGONS IVN5.2 | `.02` | 32,768 | `a92e1baeccb5f545b504e44868b46742c190dc7a36d9b834d628498447e82859` |
| QDI P895V14 | `.00` | 26,166 | `c9c7bf9011f620def301a43c53b4941cccae40ae46d7b7b8dcabb53fdcd46098` |
| QDI P895V14 | `.01` | 18,394 | `a0a47d40b6be1ed507bd1cbab6acfd38cab6bb0c815f7fb4354723693312b7e1` |
| QDI P895V14 | `.02` | 32,768 | `1176b3eca822926ee3fba4400f6a4751674f74b77990a44c9622b5088310831e` |

## Sequence-aware similarity

A simple byte-at-the-same-offset comparison is misleading when a module contains inserted or removed data, because every following byte shifts. A Python `difflib.SequenceMatcher` comparison was therefore also used.

| Comparison | `.00` POST | `.01` Setup Server | `.02` Runtime |
|---|---:|---:|---:|
| Physical IVN5.0 ↔ VOGONS IVN5.2 | **96.48%** | **94.24%** | **99.25%** |
| VOGONS IVN5.2 ↔ P895V14 | **99.47%** | **93.04%** | **96.18%** |
| Physical IVN5.0 ↔ P895V14 | **96.34%** | **90.93%** | **96.02%** |

Two relationships are especially informative:

- the **VOGONS IVN5.2 POST** and **P895V14 POST** are nearly the same generation: 26,028 of 26,166 bytes participate in matching sequences, **99.47%** similarity;
- the **physical IVN5.0 Runtime** and **VOGONS IVN5.2 Runtime** are exceptionally close: 32,523 of 32,768 bytes participate in matching sequences, **99.25%** similarity.

This makes the VOGONS IVN5.2 build look like an evolutionary bridge: its Runtime remains extremely close to the physical IVN5.0 build while its POST is already almost identical to P895V14.

This is a structural inference from binary similarity, not proof of release chronology.

## CPU identification and support evidence

### Physical IVN5.0

The Setup Server contains CPU-name strings including:

```text
P24D
486DX4
P24T
```

and the large generic AMI 486 identification table (`486DX`, `486DX2`, `486SX`, SLC/DLC variants, Plus variants, etc.).

Runtime contains:

```text
CyrixInstead
```

but no plain `AuthenticAMD` string.

No explicit `5x86` or `5x86-P75` name is present.

### VOGONS IVN5.2

Compared with the physical dump, this build adds evidence of AMD-specific identification:

```text
Am486DX4
486DX4-Plus
AuthenticAMD
```

Runtime contains both:

```text
CyrixInstead
AuthenticAMD
```

No explicit `5x86` / `5x86-P75` string was found.

### QDI P895V14

P895V14 contains the strongest explicit AMD 5x86 evidence:

```text
Am486DX4
5x86
5x86-P75
486DX4-Plus
AuthenticAMD
```

There are also code-adjacent strings such as:

```text
f=5x86XuF
f=P24Tu
cAMDu
```

These short mixed code/string sequences should not be interpreted as user-visible text, but their presence near the CPU changes is consistent with altered CPU-detection logic.

### Interpretation

The strings demonstrate that later builds know more AMD-specific CPU identities, but they do **not by themselves prove** every low-level initialization operation (write-back cache controls, model-specific register handling, multiplier behavior, etc.). Some important CPU logic is machine code with no readable string.

However, the string progression is consistent with the independent practical report that P895V14 makes an AMD 5x86-133 work on V4P895P3/SMT V5.0.

## Functionality visible in the extracted BIOS

The AMI'94 setup and POST strings expose a surprisingly detailed feature set.

### Boot and basic POST

Visible strings include functionality for:

- system keyboard test;
- memory test above 1 MB;
- memory-test tick sound;
- parity checking;
- extended BIOS RAM area;
- `Wait For "F1" If Any Error`;
- boot Num Lock state;
- floppy seek at boot;
- configurable boot sequence;
- configurable boot-up CPU speed;
- numeric coprocessor test;
- Weitek processor support;
- Turbo switch;
- password checking.

### Cache and memory/chipset tuning

Visible setup items include:

- external cache;
- internal cache;
- internal-cache write-back;
- cache read cycle;
- cache write wait state;
- DRAM burst cycle;
- memory write wait state;
- hidden refresh;
- slow refresh;
- parity check;
- two configurable non-cacheable blocks;
- cacheability controls for C000–E000 regions;
- BIOS/video shadowing.

The Setup Server also contains the explicit diagnostic message:

```text
CACHE MEMORY BAD, DO NOT ENABLE CACHE!
```

This is useful when diagnosing this board because it confirms that the BIOS contains an active cache-test/error path rather than merely exposing cache toggles.

### ISA / AT-bus controls

Strings expose:

- AT cycle wait state;
- AT BUS clock selection;
- AT BUS clock control;
- keyboard reset control.

### IDE and storage

The BIOS contains setup text for:

- primary/secondary controller 32-bit transfer;
- per-device LBA mode;
- HDD auto-detection;
- user-defined HDD parameters;
- IDE power-down;
- boot-sector virus protection.

The physical IVN5.0 build additionally exposes explicit strings such as:

```text
IDE MODE
On-Board Floppy
On-Board HD
Drive 0
Drive 1
Block Xfer
LBA Mode
Read Ahead
sectors per int
Serial-1 Addr
Serial-1 FIFO
Serial-2 Addr
Serial-2 FIFO
Parallel Addr
Parallel Mode
```

Some of these literal strings disappear or are reorganized in the later images. Their absence as strings does not necessarily mean the hardware function itself was removed.

### Power management / “Green PC”

Visible setup functionality includes:

- Power Management Mode Select;
- System Timeout;
- IDE Power Down;
- keyboard I/O activity monitor;
- floppy I/O activity monitor;
- hard-disk I/O activity monitor;
- video I/O activity monitor;
- video-memory activity monitor.

This matches the vendor identity strings containing `GREEN PC` / `GREEN`.

### Setup and configuration UI

The firmware contains separate menu labels for:

- Standard Setup;
- Advanced Setup;
- Chipset Setup;
- Peripheral Setup;
- Power Management Setup;
- Virus Protection;
- password configuration.

### Runtime errors and protections

Runtime strings include:

- diskette boot failure;
- invalid boot diskette;
- drive not ready;
- system halted;
- ROM/current password prompts;
- reboot-required prompt;
- invalid compressed BIOS;
- boot-sector write warning;
- possible-virus confirmation;
- parity error.

The Setup Server also reports:

- CMOS configuration/time errors;
- keyboard errors;
- serial/parallel/floppy/HDD resource conflicts;
- NVRAM checksum/inoperational errors;
- expansion-board-not-ready;
- fail-safe timer NMI errors;
- boot input/output device not found.

## Internal POST identifiers

The decompressed Runtime contains the following base IDs:

```text
Physical IVN5.0 : 40-2004-428003-00101111-072594-OPTI802
VOGONS IVN5.2  : 40-2207-428003-00101111-072594-OPTI802
QDI P895V14    : 40-2214-428003-00101111-072594-OPTI802
```

The `072594` field corresponds to 25 July 1994 and matches the AMI core date. It should not be confused with the later QDI vendor-build dates in the Runtime identity strings.

A photographed boot screen from the physical board shows the original BIOS POST identifier with an `-H` suffix rendered by the running firmware.

## What is not yet proven

This analysis does not yet establish:

- the exact instruction-level changes responsible for Am5x86 initialization;
- whether all setup strings correspond to visible menu items on every hardware configuration;
- whether hidden AMI setup entries are gated by chipset/board flags;
- the precise meaning of every numeric field in the AMI POST identifier.

Those questions would require deeper disassembly or an AMI-specific setup-table editor in addition to AmiDeco/string analysis.
