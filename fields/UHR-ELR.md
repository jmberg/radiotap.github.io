---
title: UHR-ELR
categories: [suggested]
type: 37
---
Type
: 37 (not assigned yet)

Structure
: - u32 known
  - u32 sig1, sig2, mark

Required Alignment
: 4

Unit(s)
: none

This field represents (data taken from) the ELR preamble for a UHR
ELR (enhanced long range) PPDU. Such PPDUs are always transmitted
with a single spatial stream and on a 20 MHz channel.

## known

| **bits**         | **meaning** |
| **`0x00000001`** | ELR Version Identifier Known |
| **`0x00000002`** | UL/DL Known |
| **`0x00000004`** | MCS Known |
| **`0x00000008`** | Coding Known |
| **`0x00000010`** | Length Known |
| **`0x00000020`** | LDPC Extra OFDM Symbol Known |
| **`0x00000040`** | ELR-SIG-1 CRC Known |
| **`0x00000080`** | ELR-SIG-1 Tail Known |
| **`0x00000100`** | STA-ID Known |
| **`0x00000200`** | Disregard Known |
| **`0x00000400`** | ELR-SIG-2 CRC Known |
| **`0x00000800`** | ELR-SIG-2 Tail Known |
| **`0x00001000`** | ELR-SIG-1 CRC Checked |
| **`0x00002000`** | ELR-SIG-2 CRC Checked |
| **`0x0000C000`** | (reserved) |
| **`0x00010000`** | ELR-MARK BSS Color Known |
| **`0xFFFE0000`** | (reserved) |

## sig1

Represents data from ELR-SIG-1, subject to availability (`known` flags)

| **bits**         | **meaning** |
| **`0x00000001`** | ELR Version Identifier |
| **`0x00000002`** | UL/DL |
| **`0x00000004`** | MCS |
| **`0x00000008`** | Coding |
| **`0x00001FF0`** | Length |
| **`0x00002000`** | LDPC Extra OFDM Symbol |
| **`0x0003C000`** | CRC |
| **`0x00FC0000`** | Tail |
| **`0x7F000000`** | (reserved) |
| **`0x80000000`** | CRC valid (if checked) |

## sig2

Represents data from ELR-SIG-2, subject to availability (`known` flags)

| **bits**         | **meaning** |
| **`0x000007FF`** | STA-ID |
| **`0x00003800`** | Disregard |
| **`0x0003C000`** | CRC |
| **`0x00FC0000`** | Tail |
| **`0x7F000000`** | (reserved) |
| **`0x80000000`** | CRC valid (if checked) |

## mark

Represents data from ELR-MARK, subject to availability (`known` flags)

| **bits**         | **meaning** |
| **`0x0000003F`** | BSS Color |
| **`0xFFFFFFC0`** | (reserved) |
