---
parent: DISK 1
title: Overview
nav_order: 1
---

## File Loading

| INIT |                         | `$4000`-`$50E5`   |
| SEL  | BLOADed by INIT `$4095` | `$0320`-`$03B5`   |
| SUBS | BLOADed by INIT `$40A7` | `$0800`-`$2000`   |
| SHP0 | BLOADed by INIT `$410E` | `$D000`-`$E000` bank 1  |
| SHP1 | BLOADed by INIT `$4122` | `$D000`-`$E000` bank 2  |
| TBLS | BLOADed by INIT `$412F` | `$E000`-`$E400`   |
| HTXT | BLOADed by INIT `$4143` | `$E400`-`$E800`   |
| BOOT | BRUN    by INIT `$4157` | `$6000`-`$7394`   |

| MUSA | BLOADed by SEL  `$0384` | `$F000`-          |

| ANIM | BLOADed by BOOT `$6003` | `$4000`-          |
| BGND | BLOADed by BOOT `$6017` | `$7A00`-          |

also:

| MBSM | BLOADed by BOOT `$69F0` | `$F000`-          |
| MBSI | BLOADed by BOOT `$6A04` | `$8000`-          |

finally

| ULT4 | BRUN    by BOOT `$6B24` | `$4000`-          |

DISK is loaded by:
* BOOT at `$70E6`
* SUBS at `$18EC`

ULT4 loads:

* PRTY
* LIST
* ROST
* TLST
* CON_
* SAVE

Also, NEWGAME is BRUN from BOOT `$6BEE` on character creation.

Other files still to investigate.

## memory map

* `$E800` disk buffer?
* `$E900` disk buffer?
* `$EA00` disk buffer?
* `$EB00` disk buffer?

* `$EC00` party roster
* `$ED00` overall attributes
* `$EE00` LIST or disk buffer

### player data vector

* `$00` thru `$07` - name?
* `$12` - player status (e.g. `G` or `P` or `D`)
* `$18` - most-significant digit of player health
* `$19` - lower two digits of player health
* `$1A` - most-significant digit of max player health
* `$1B` - lower two digits of max player health

### ED00

* `$00` thru `$07` - virtue scores ???
* `$08` - torch count?
* `$10` - first two digits of food
* `$11` - next two digits of food
* `$13` - first two digits of gold
* `$14` - next two digita of gold
