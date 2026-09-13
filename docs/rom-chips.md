# BIOS ROM-chip compatibility

## Original device

The physical motherboard used for this archive contains a **Texas Instruments TMS27C512** in DIP-28.

Observed/programmer facts:

- capacity: **512 kbit = 64 KiB = 64K × 8**;
- package: **DIP-28**;
- programmer ID: **`97 85`**;
- XGecu profile used: **`TMS27C512 @ DIP28`**;
- two independent reads were bit-identical;
- original ROM SHA-256: `7874a75e23389c329917e1b50bfa531a4853829b821e4a6fec46430a06966097`.

The TMS27C512 is a UV-erasable EPROM. Windowed ceramic versions are erased with UV light; the opaque BIOS label also protects the window from light during normal use.

## What the motherboard cares about

During a normal boot the motherboard only reads the ROM. It does not care whether the stored bits come from UV EPROM, electrically erasable EPROM, EEPROM, or flash **provided the read-side electrical interface is compatible**.

The important properties are:

- correct capacity/addressing;
- correct package/socket alignment;
- compatible pinout;
- 5 V read operation;
- compatible logic levels;
- sufficiently fast access time;
- correct `/CE` and `/OE` behavior.

Programming and erase voltages matter to the programmer, not to the motherboard during normal reading.

## Recommended reusable replacement: Winbond W27C512

For repeated experiments, a **Winbond W27C512 in DIP-28** is the most convenient replacement discussed here.

Why:

- 64K × 8 / 64 KiB, matching the original ROM capacity;
- compatible 28-pin read interface;
- 5 V read operation;
- electrically erasable and reprogrammable;
- directly supported by XGecu T48/Xgpro.

Suitable speed grades include:

```text
W27C512-45Z
W27C512-70
W27C512-90
W27C512-12
```

A `W27C512-45Z` is **not “too fast”**. The access-time suffix specifies the maximum time by which valid data are guaranteed. The motherboard continues to generate its original bus timing; a faster ROM merely presents valid data earlier.

Always select the exact `W27C512` device profile in Xgpro for erase/program operations.

Recommended workflow:

```text
Erase
→ Blank Check
→ Program
→ Verify
→ Read back
→ compare SHA-256 with source image
```

Keep the original TMS27C512 unchanged as the known-good recovery ROM.

## Standard 27C512 family

Other 27C512-compatible 64K × 8 DIP-28 parts can generally provide the same read interface. They may be:

- UV-erasable windowed EPROMs;
- one-time-programmable plastic parts;
- manufacturer-specific variants with different programming algorithms/voltages.

For programming, always select the exact device in the programmer software.

## 28C512 EEPROM

A 28C512 is also 64K × 8 but is commonly a **32-pin** parallel EEPROM with a different programming/write interface and package arrangement.

A VOGONS user reported successfully using a 28C512 on this exact motherboard family because the board provides a larger ROM socket capable of accommodating it. This is useful evidence, but a 28C512 should **not** be treated as a universal pin-for-pin DIP-28 replacement.

For a simple reusable replacement, W27C512 DIP-28 is the cleaner option.

## Larger flash devices

Devices such as 39SF010/39SF040 are larger 32-pin flash memories. The motherboard manual mentions Flash ROM BIOS support, but this does **not** mean every parallel flash part is automatically compatible.

Differences can include:

- capacity;
- address-pin mapping;
- package/pin count;
- write-control pins;
- board strapping/address decoding.

Do not substitute a larger flash device without verifying the board socket pinout and the exact part datasheet.

## Socket alignment

The board uses a socket larger than the original 28-pin TMS27C512. When installing a 28-pin replacement, preserve the exact original position and pin-1 orientation. A correctly oriented chip can still be wrong if shifted by one or two socket positions.
