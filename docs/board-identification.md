# Board identification and firmware lineage

## Physical identification

The motherboard documented here is physically marked:

```text
V4P895P3/SMT V5.0
```

It is a Socket 3 / VLB board from the QDI / Legend V4P895 family using the OPTi 895 chipset family. The available QDI manual for `VL/ISA-PB486P3` / `V4P895P3/SMT` matches the board layout and describes an AMI WinBIOS with Flash ROM BIOS support.

The installed ROM has the AMI sticker `AC0410599`. Programmer signature `97 85` identified the underlying device as a Texas Instruments TMS27C512.

## Why the board is very likely an authentic QDI V4P895P3/SMT

No single marking is treated as decisive. The identification is supported by the combination of:

1. PCB silkscreen `V4P895P3/SMT V5.0`;
2. physical layout matching the surviving QDI manual;
3. OPTi 895 firmware family;
4. internal Runtime identity `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994`;
5. AMI/OPTi POST identifier `40-2004-428003-00101111-072594-OPTI802`;
6. close firmware relationship to the publicly archived V4P895P3/SMT images.

A period 1:1 clone cannot be excluded purely from software evidence, but a clone reproducing the PCB design, chipset wiring, silkscreen and firmware interface would also be expected to have essentially the same BIOS compatibility requirements.

## Important distinction: PCB revision vs firmware build

Numbers such as **V5.0** on the PCB identify the board revision. They should not automatically be read as the QDI BIOS release number.

The three images in this repository contain their own internal vendor build strings:

| Image | Internal build string | Internal POST ID |
|---|---|---|
| Physical dump | `OPTi895P3 GREEN PC IVN5.0 7 Dec, 1994` | `40-2004-...` |
| VOGONS archive | `OPTi895P3 GREEN PC IVN5.2 27 May, 1995` | `40-2207-...` |
| P895V14 | `OPTi895GRN GREEN IVN1.4 22, Nov, 1995` | `40-2214-...` |

QDI's archive separately identifies `P895V14.ZIP` as **V4P895/SMT BIOS version 1.4**.

## P895V14 compatibility confidence

Confidence that `P895V14.ROM` is appropriate for V4P895P3/SMT V5.0 is high because:

- QDI's archived description explicitly targets `V4P895/SMT`;
- an independent VOGONS user with `V4P895P3/SMT V5.0` reported that P895V14 resolved an outdated-BIOS problem and allowed an AMD 5x86-133 to run;
- the extracted POST module of the VOGONS V5 image and P895V14 is **99.47% similar** by sequence-aware matching;
- CPU-identification data in P895V14 explicitly includes `5x86` and `5x86-P75`.

A failed firmware experiment is still possible. The safest test procedure is to leave the original TMS27C512 untouched and program P895V14 into a separate compatible ROM.
