---
parent: DISK 1
nav_order: 11
has_children: true
---

# SUBS

**SUBS** is on Disk 1. It is loaded into $0800–$2000 by **INIT** very early on.

It contains subroutines shared by multiple files.


**$0800** / **$0AB5** — get a keypress

**$081B** / **$0FD2** — print the following null-terminated string via the OS

**$081E** / **$0F81** — set the current cursor location to the X, Y index registers then display the following null-terminated string (as in **$0F85**)

**$0821** / **$0F85** — display the following null-terminated string at the current cursor location

**$0824** / **$0FA9** — display the single character in the accumulator at the current cursor location

**$082D** / **$1002** — set up **$FE**/**$FF** to point to the right character data vector based on **$D4**

**$0833** / **$107C** — display a decimal-encoded byte (one decimal digit per nibble) in the accumulator at the current cursor location

[**$0842**](01_SUBS_0842) / **$1868** — request disk switch

[**$0845**](01_SUBS_0845) / **$10C3** — display player stats

**$0866** / **$108B** — display the single digit number in the accumulator at the current cursor location

[**$0872**](01_SUBS_0872) / **$11E2** — display direction faced and level in dungeon

**$1263** — persist the current cursor location

**$126E** — restore the saved cursor location and return

**$104A** — display name of character in slot **$D4**

**$150C** — process and buffer any keypress
