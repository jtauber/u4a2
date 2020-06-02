---
parent: DISK 1
nav_order: 8
has_children: true
---

# NEWGAME

Run when starting a new game. This code takes the user through the introductory images and text, presents the virtue dilemmas, and sets up the initial character stats and starting location based on them.

## Additional BLOAD'd

* `TREE.SPK` — loaded at `$6439` and `$64AC`
* `PRTL.SPK` — loaded at `$6486`
* `LOOK.SPK` — loaded at `$64D2`
* `FAIR.SPK` — loaded at `$6504`
* `WAGN.SPK` — loaded at `$652A`
* `GYPS.SPK` — loaded at `$6550`
* `TABL.SPK` — loaded at `$6576`

The above `SPK` files are loaded into `$4000` and then decoded into the hires page 1. The decoding routine starts at `$6B96`.

* `CRDS` — loaded at `$659C`

The above likely contains the card images. It is loaded into `$4000` and `$6B3E` is the routine for drawing a particular card on the screen.

* `NPRT` — loaded at `$6734` into `$00`
* `NLST` — loaded at `$6734` into `$EE00`
* `NRST` — loaded at `$6734` into `$EC00`

* `DISK` — loaded at `$69EA` just to check what disk is in the drive

And when done, ULT4 is BRUN.

## Key Data Locations

* `$642F` — stores the text number we're up to in the introductory story
* `$67E6[]` — the virtue selections during the dilemmas (`00` = not seen; `01` = chosen; `FF` = rejected)
* `$67EE[]` — base text numbers for the dilemmas each virtue
* `$67F5` — top virtue / character class
* `$67F6` — first card/virtue
* `$67F7` — second card/virtue
* `$67F8` — number of dilemmas / card pairs seen
* `$67F9[]` — possible X starting location based on virtue/class
* `$6801[]` — possibly Y starting location based on virtue/class
* `$6809` — STR
* `$680A` — DEX
* `$680B` — INT
* `$680C[]` — increase to STR based on top virtue / class
* `$6814[]` — increase to DEX based on top virtue / class
* `$681C[]` — increase to INT based on top virtue / class
* `$6824[]` — initial virtue scores
* `$6B86[]` — 16-bit values; address of card image by virtue
* `$6CB3-` — string pool

## String Pool

The data is at `$6CB3` and consists of null-delimited strings. The strings are numbers by how many nulls you have to pass to get to the string you want. The text display routine at `$6A77`.

* `0x01` thru `0x1C` are the virtue dilemmas
* `0x1D` thru `0x34` are the parts of the introductory story leading to the card placement
* `0x35–0x38` are texts when gypsy is placing cards
* `0x39–0x40` are virtue names
* `0x42–0x43` are final messages before main game starts