---
nav_order: 6
---

# Zero Page Notes

```
$0A - related to player location
$0B - if #$03, we're in a dungeon

$0C - in dungeon, what level we're on (one less than what's displayed, e.g. #$00 = level 1)

$0E - what player's icon/tile is
      0x10 - 0x13 ship
      0x14 - 0x15 horse
      0x18 balloon
      0x1F on foot
$0F - party size

$10 - in dungeon, what direction we're facing

$14 - is inverted on Yell and high bit is used to determine whether 'woah!' or 'giddyup!'
      is 0x00 after resurrection

$15 - 0x00 if never met LB before, 0x01 once met
$1B - ship strength

$80
$81
$82
$83

$BC - gender: 0x5C for Male and 0x7B for Female

$C6 - avatar status character?

$C8 - tile player currently on?

$CE - text column
$CF - text row

$D0 - the disk number just loaded (from DISK file)
$D1 - ??? number of drives ???
$D2 - the disk number in drive 1
$D3 - the disk number in drive 2

$D4 - character number

$D8 - ???
$D9 - temp var
$DA - temp var

$DE - desired disk number
$DF - drive number

; RWTS
$E0 - track number
$E1 - sector number
$E2 - command (0=SEEK, 1=READ, 2=WRITE, 4=FORMAT)
$E3 - data buffer high byte ?

$EA - character buffer pointer

$F5 - what direction wind is blowing from
      #$00  WEST
      #$01  NORTH
      #$02  EAST
      #$03  SOUTH
or something else?
```