---
parent: DISK 2
nav_order: 2
---

# CAMP

Contains the camp map and other data. File is of length 0xC0, similar to the CON_ files, with the map proper starting 0x40 in.

It is loaded into $0240 by HOLE $8800 and $889B.

From the HOLE code at $8830 to $8839 it seems $0260 and $0268 are zero-indexed arrays over the characters in the party. They get copied in to $EF8X and $EF9X respectively.

Purely speculating, but these could be the starting (x, y) for each character.

```
0240    04 06 03 07 01 09 00 0A
0248    00 0A 01 09 03 07 04 06
0250    00 00 01 01 03 03 04 04
0258    06 06 07 07 09 09 0A 0A

0260    05 03 05 07 04 04 06 06
0268    03 05 07 05 04 06 06 04

0270    08 AD 83 C0 AD 83 C0 AD
0278    83 C0 A0 00 B9 A6 08 F0
```

```
;; map

0280    05 05 05 04 04 04 04 04 05 05 05
028B    05 05 04 04 04 04 04 04 04 05 05
0296    05 04 04 04 39 39 39 04 04 04 05
02A1    04 04 04 04 04 04 04 04 39 04 04
02AC    04 04 39 04 04 04 04 39 39 04 04
02B7    04 04 39 04 04 4B 04 04 39 04 04
02C2    04 04 39 39 04 04 04 04 39 04 04
02CD    04 04 39 04 04 04 39 04 04 04 04
02D8    05 04 04 04 39 39 39 04 04 04 05
02E3    05 05 04 04 04 04 04 04 04 05 05
02EE    05 05 05 04 04 04 04 04 05 05 05
```

![CAMP map](/assets/game/02_CAMP.png)

```
;;

02F9    8D 00 00 00 00 B7 09
```
