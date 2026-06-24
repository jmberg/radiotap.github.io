---
title: UHR
categories: [suggested]
type: 38
---
Type
: 38 (not assigned yet)

Structure
: - u32 known
  - u32 data[9]
  - {u32 user_known, u32 user_info}[] (Note this is variable length)

Required Alignment
: 4

Unit(s)
: none

## known

Note that for trigger-based (TB) PPDUs, the UHR-SIG isn't present over the
air as all the information for each station is provided in the trigger
frame. Sniffer should - where possible - provide a subset of the OFDMA
related fields to simplify understanding of the resulting capture (i.e. to
be able to read it without having to go back to the prior trigger frame.)

| **bits**         | **OFDMA (including TB)** | **non-OFDMA** |
| **`0x00000001`** | Spatial Reuse Known | (same) |
| **`0x00000002`** | GI+LTF Size Known | (same) |
| **`0x00000004`** | Number of UHR-LTF Symbols Known | (same) |
| **`0x00000008`** | LDPC Extra Symbol Segment Known | (same) |
| **`0x00000010`** | Pre-FEC Padding Factor Known | (same) |
| **`0x00000020`** | PE Disambiguity Known | (same) |
| **`0x00000040`** | Disregard Known | (reserved) |
| **`0x00000080`** | CRC1 Known | (same) |
| **`0x00000100`** | Tail1 Known | (same) |
| **`0x00000200`** | CRC2 Known | (reserved) |
| **`0x00000400`** | Tail2 Known | (reserved) |
| **`0x00000800`** | (reserved) | Interference Mitigation Known |
| **`0x00001000`** | (reserved) | Disregard Known |
| **`0x00002000`** | (reserved) | Number Of Non-OFDMA Users Known |
| **`0x00004000`** | (reserved) | Common Encoding Block CRC Known |
| **`0x00008000`** | (reserved) | Common Encoding Block Tail Known |
| **`0x00010000`** | RU/MRU/DRU Size Known | (same) |
| **`0x00020000`** | RU/MRU Index Known | (same) |
| **`0x00040000`** | DRU/RRU Allocation (TB format) Known | (reserved) |
| **`0x00080000`** | Primary 80 MHz Channel Position known | (same) |
| **`0xfff00000`** | (reserved) | (reserved) |

Note: The "RU/MRU/DRU Size" and "RU/MRU Index" fields are provided for
sniffers that cannot provide the entirety of the RU allocations and user
information fields for downlink OFDMA PPDUs (which would allow knowing
exactly which AID got which RU allocation), but can only provide a
subset of the information. They apply only to the captured AID. It's
advantageous to provide this even when all the RU Allocaction and User
Information fields are known, so the display tool need not parse all of
them.

Note: For uplink OFDMA, the DRU size could also be given here for
similar reasons, though the actual DRU allocation should then be
given for the captured AID from the trigger frame in the DRU/RRU
Allocation fields.

Some values here could also appear for non-OFDMA PPDUs.

## data[0]

| **bits**         | **meaning** |
| **`0x0000000F`** | Spatial Reuse |
| **`0x00000030`** | GI+LTF Size (0=2xLTF+0.8us, 1=2xLTF+1.6us, 2=4xLTF+0.8us, 3=4xLTF+3.2us) |
| **`0x000000c0`** | (reserved) |
| **`0x00000700`** | Number of LTF symbols (0=1x, 1=2x, 2=4x, 3=6x, 4=8x, 5-7=reserved) |
| **`0x00000800`** | LDPC Extra Symbol Segment |
| **`0x00003000`** | Pre-FEC padding factor |
| **`0x00004000`** | PE Disambiguity |
| **`0x00078000`** | Disregard (OFDMA only, B13-B16) |
| **`0x00780000`** | CRC1 (OFDMA: B17+9N-B20+9N, non-OFDMA: B42-B45) |
| **`0x1f800000`** | Tail1 (OFMDA: B21+9N-B26+9N, non-OFDMA: B46-B51) |
| **`0xe0000000`** | (reserved) |

## data[1]

| **bits** | **meaning** |
| **`0x0000001f`** | RU/MRU/DRU Size (0: 26, 1: 52, 2: 106, 3: 242, 4: 484, 5: 996, 6: 2x996, 7: 4x996, 8: 52+26, 9: 106+26, 10: 484+242, 11: 996+484, 12: 996+484+242, 13: 2x996+484, 14: 3x996, 15: 3x996+484) |
| **`0x00001fe0`** | RU/MRU Index (see IEEE 802.11 36.3.2 "Subcarrier and resource allocation") |
| **`0x003fe000`** | RU Allocation 1 |
| **`0x00400000`** | RU Allocation 1 Known |
| **`0x3f800000`** | (reserved) |
| **`0xc0000000`** | Primary 80 MHz Channel Position (0: lowest, 3: highest in frequency)|

Note: The RU Allocation 1 here and other RU Allocation fields are in the
downlink OFDMA format and do not represent DRUs, which are used only in
uplink OFDMA. All RU allocations in uplink OFDMA are are visible only in
the trigger frame, and, for the PPDU that was captured, in the other RU
related fields.

Note: The RU/MRU/DRU Size and RU/MRU/DRU Index are calculated fields, ideally
the sniffer should provide them for all OFDMA and punctured non-OFDMA PPDUs to
simplify filtering and other data uses.

Note: The Primary 80 MHz channel position is numbered from low frequency to
high frequency. Also note that it is required in order to decode the "DRU/RRU
Allocation (TB format)" field, since the decoding thereof requires doing the
table lookup from 802.11 Table 9-65 (Lookup table for X1 and N) to have
the correct values for interpretation of 802.11 Table 9-64 (Encoding of
PS160 and RU Allocation subfields in an EHT variant User Info field).

## data[2]-data[6]

| **bits** | **meaning** |
| **`0x000001ff`** | RU Allocation X |
| **`0x00000200`** | RU Allocation X known |
| **`0x0007fc00`** | RU Allocation (X + 1) |
| **`0x00080000`** | RU Allocation (X + 1) known |
| **`0x1ff00000`** | RU Allocation (X + 2) |
| **`0x20000000`** | RU Allocation (X + 2) known |
| **`0xc0000000`** | (reserved) |

### RU Allocation field order

Together with the "RU Allocation 1" field from data[1], the RU Allocation
fields shall be in the following order:

| RU Allocation                        | 20MHz | 40MHz | 80MHz | 160MHz | 320MHz |
| :---                                 | :---: | :---: | :---: |  :---: |  :---: |
| Content Channel 1 RU Allocation 1::1 |     X |     X |     X |      X |      X |
| Content Channel 2 RU Allocation 1::1 |     - |     X |     X |      X |      X |
| Content Channel 1 RU Allocation 1::2 |     - |     - |     X |      X |      X |
| Content Channel 2 RU Allocation 1::2 |     - |     - |     X |      X |      X |
| Content Channel 1 RU Allocation 2::1 |     - |     - |     - |      X |      X |
| Content Channel 2 RU Allocation 2::1 |     - |     - |     - |      X |      X |
| Content Channel 1 RU Allocation 2::2 |     - |     - |     - |      X |      X |
| Content Channel 2 RU Allocation 2::2 |     - |     - |     - |      X |      X |
| Content Channel 1 RU Allocation 2::3 |     - |     - |     - |      - |      X |
| Content Channel 2 RU Allocation 2::3 |     - |     - |     - |      - |      X |
| Content Channel 1 RU Allocation 2::4 |     - |     - |     - |      - |      X |
| Content Channel 2 RU Allocation 2::4 |     - |     - |     - |      - |      X |
| Content Channel 1 RU Allocation 2::5 |     - |     - |     - |      - |      X |
| Content Channel 2 RU Allocation 2::5 |     - |     - |     - |      - |      X |
| Content Channel 1 RU Allocation 2::6 |     - |     - |     - |      - |      X |
| Content Channel 2 RU Allocation 2::6 |     - |     - |     - |      - |      X |

## data[7]

| **bits** | **meaning** |
| **`0x0000000f`** | CRC2 (OFDMA only: for RU Allocation-B) |
| **`0x000003f0`** | Tail2 (OFDMA only: after RU Allocation-B) |
| **`0x00000400`** | Interference Mitigation (non-OFDMA only, 0=enabled, 1=disabled as in preamble) |
| **`0x00001800`** | Disregard (non-OFDMA only) |
| **`0x0000e000`** | Number Of Non-OFDMA Users (non-OFDMA only) |
| **`0x000f0000`** | Common Encoding Block CRC (non-OFDMA only, B42-B45) |
| **`0x03f00000`** | Common Encoding Block Tail (non-OFDMA only, B46-B51) |
| **`0xfc000000`** | (reserved) |

## data[8]

| **bits** | **meaning** |
| **`0x00000001`** | DRU/RRU Allocation (TB format): PS 160 |
| **`0x00000002`** | DRU/RRU Allocation (TB format): B0 |
| **`0x000001fc`** | DRU/RRU Allocation (TB format): B7--B1 |
| **`0x00000200`** | DRU/RRU Indication (0: DRU, 1: RRU) |
| **`0xfffffc00`** | (reserved) |

Note: The `0x1ff` bits here indicate the DRU/RRU Allocation for the captured
station (AID) in the trigger-based (TB) format, per Table 9-64 (Encoding of
PS160 and RU Allocation subfields in an EHT variant User Info field). This
may be given by the sniffer even for downlink OFDMA PPDUs if calculated from
the RU Allocation from EHT-SIG for the given station. Note the bit split to
ease cross-referencing Table 9-64.

If the indicated RU is a DRU, then the UHR variant tables are used instead:
Tables 9-65a (Encoding of the PS160 and RU Allocation subfields in a UHR
variant User Info field for DBW 20 MHz), 9-65b (40 MHz), 9-65c (80 MHz).

Note: The DRU/RRU Indication bit can only be 0 (indicating DRU)
for uplink OFDMA. There's no separate "known" bit for it since
it must be given to interpret the DRU/RRU Allocation.

## user entry

Each `user_known` and `user_info` entry carries information for a given
user entry in the UHR preamble. If multiple user fields are captured,
ideally all of them are, and they should be given in the same order as
they were ordered in the frame.

Note that to simplify parsing we indicate non-MU-MIMO (e.g. OFDMA) and
MU-MIMO information via separate known bits, so that parsers can show
these fields without consulting other information to know the PPDU type.

The "Data captured for this user" bit indicates which user the data was
captured for that this radiotap header is attached to. It should be set
for exactly one user entry in the entire radiotap header, even if
potentially the entire UHR field is given multiple times.

### `user_known`

| **bits**         | **non-MU-MIMO / Co-SR** | **MU-MIMO / Co-BF** |
| **`0x00000001`** | STA-ID known | (same) |
| **`0x00000002`** | MCS known | (same) |
| **`0x00000004`** | NSS known | (reserved) |
| **`0x00000008`** | UEQM known | (reserved) |
| **`0x00000010`** | Beamforming known | (reserved) |
| **`0x00000020`** | Coding known | (same) |
| **`0x00000040`** | UEQM Pattern known | (reserved) |
| **`0x00000080`** | 2xLDPC known | (same) |
| **`0x00000100`** | (reserved) | Spatial Configuration known |
| **`0x00000200`** | (reserved) | Disregard known |
| **`0x00000400`** | (reserved) | BSS Color Indication known |
| **`0x00000800`** | User Encoding Block CRC known | (same) |
| **`0x00001000`** | User Encoding Block Tail known | (same) |
| **`0x0000e000`** | (reserved) | (reserved) |
| **`0x000f0000`** | User Encoding Block CRC (B23N-B23N+3) | (same) |
| **`0x03f00000`** | User Encoding Block Tail (B23N+4-B23N+9)| (same) |
| **`0x7c000000`** | (reserved) | (reserved) |
| **`0x80000000`** | Data captured for this user | (same) |

### `user_info`

| **bits**         | **non-MU-MIMO / Co-SR** | **MU-MIMO / Co-BF** |
| **`0x000007ff`** | STA-ID | (same) |
| **`0x0000f800`** | MCS | (same) |
| **`0x00070000`** | NSS (0=1 spatial stream, 1=2 spatial streams, ...) | (overlap) |
| **`0x00080000`** | (reserved) | (overlap) |
| **`0x000f0000`** | (overlap) | Spatial Configuration |
| **`0x00100000`** | UEQM | Disregard |
| **`0x00200000`** | Beamforming | BSS Color Indication |
| **`0x00c00000`** | UEQM Pattern | (reserved) |
| **`0x01000000`** | Coding (0 for BCC, 1 for LDPC) | (same) |
| **`0x02000000`** | 2xLDPC | (same) |
| **`0xfc000000`** | (reserved) | (reserved) |
