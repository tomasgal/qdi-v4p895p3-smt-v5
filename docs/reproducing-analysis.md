# Reproducing the BIOS analysis

The analysis was performed under Linux/WSL using **AmiDeco 0.31e** plus standard Unix tools and Python 3.

The repository already preserves the exact decomposition artifacts used for the documented comparison under:

- [`bios/original/decomposed/`](../bios/original/decomposed/)
- [`bios/archive/decomposed/`](../bios/archive/decomposed/)
- [`bios/qdi-official/decomposed/`](../bios/qdi-official/decomposed/)

Each preserved `input.rom` is the same Git blob as the corresponding canonical raw BIOS image. The commands below therefore reproduce the stored artifacts from the canonical files.

## 1. List the BIOS structure

```bash
amideco ROMFILE.bin -l
```

For all three images, AmiDeco reports AMI'94, release/core date 25 July 1994, and a three-section POST / Setup Server / Runtime scheme.

## 2. Extract each image in its own directory

```bash
mkdir -p analysis/original analysis/vogons analysis/p895v14

cp bios/original/OPTi895P3_GREEN_PC_IVN5.0_1994-12-07.bin analysis/original/input.rom
cp bios/archive/qdi_v4p895p3_smt_v5_bios.bin analysis/vogons/input.rom
cp bios/qdi-official/P895V14.ROM analysis/p895v14/input.rom

(cd analysis/original && amideco input.rom -x)
(cd analysis/vogons  && amideco input.rom -x)
(cd analysis/p895v14 && amideco input.rom -x)
```

The resulting `input.rom`, `amibody.00`, `amibody.01`, and `amibody.02` can be compared directly with the corresponding files under `bios/*/decomposed/`.

## 3. Inspect CPU-related strings by module

```bash
for d in original vogons p895v14; do
    echo "===== $d ====="
    for f in amibody.00 amibody.01 amibody.02; do
        echo "--- $f ---"
        strings -a -td "analysis/$d/$f" |
        grep -Eai 'AuthenticAMD|Am486|5x86|P24T|P24D|486DX4|Cyrix|CPU Clock|Main Processor' || true
    done
done
```

## 4. Extract internal vendor identities

```bash
for d in original vogons p895v14; do
    echo "===== $d ====="
    strings -a -td "analysis/$d/amibody.02" |
    grep -E 'OPTi895|40-[0-9]{4}-428003'
done
```

## 5. Sequence-aware binary similarity

Direct `cmp` results are difficult to interpret when inserted bytes shift the rest of a module. The following script measures matching sequences while allowing insertions/deletions:

```python
from pathlib import Path
from difflib import SequenceMatcher

pairs = [
    ("original", "vogons"),
    ("vogons", "p895v14"),
    ("original", "p895v14"),
]

for a, b in pairs:
    print("\n" + "=" * 72)
    print(f"{a} VS {b}")
    print("=" * 72)

    for fn in ("amibody.00", "amibody.01", "amibody.02"):
        x = (Path("analysis") / a / fn).read_bytes()
        y = (Path("analysis") / b / fn).read_bytes()

        sm = SequenceMatcher(None, x, y, autojunk=False)
        blocks = [m for m in sm.get_matching_blocks() if m.size]
        matched = sum(m.size for m in blocks)
        similarity = matched / max(len(x), len(y)) * 100

        print(
            f"{fn}: {len(x)} / {len(y)} bytes, "
            f"{matched} matching-sequence bytes, "
            f"{similarity:.2f}%"
        )
```

## 6. Verify the physical dump

The original TMS27C512 was read twice. For any repeat dump:

```bash
sha256sum dump1.bin dump2.bin
cmp dump1.bin dump2.bin
```

Expected SHA-256 for the preserved original:

```text
7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097
```

For the repository copies, verify all three canonical images with:

```bash
sha256sum --check CHECKSUMS.sha256
```
