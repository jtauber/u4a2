---
grand_parent: DISK 1
parent: SEL
nav_order: 1
---

# SEL full listing

```
ORG $0320
LEN $0095

0320  4C 38 03      JMP $0338
0323  4C 9C 03      JMP $039C

;; DATA

0326  00
0327  00
0328  00

0329  AE 28 03      LDX $0328
032C  9A            TXS
032D  AD 27 03      LDA $0327
0330  48            PHA
0331  AD 26 03      LDA $0326
0334  48            PHA
0335  4C 1B 08      JMP $081B     ; PRINT routine in SUBS

0338  AA            TAX
0339  30 22         BMI ---

033B  CD 00 F0      CMP $F000     ; this is where MUSA is loaded
033E  B0 0C         BCS $034C
0340  85 80         STA $80
0342  85 81         STA $81
0344  C9 00         CMP #$00
0346  D0 05         BNE $034D
0348  A5 82         LDA $82
034A  D0 FC         BNE ---
034C  60            RTS

034D  A5 82         LDA $82
034F  D0 0B         BNE $035C
0351  A9 00         LDA #$00
0353  85 84         STA $84
0355  A9 F0         LDA #$F0
0357  85 85         STA $85
0359  20 00 04      JSR $0400     ; where is this?
035C  60            RTS

;; load MUS_ where _ is in accumulator

035D  8D 8E 03      STA $038E     ; the A in MUSA
0360  A5 D0         LDA $D0       ; DISK NUMBER?
0362  C9 01         CMP #$01      ; if it's zero, we'll need the ^A hack
0364  D0 0C         BNE $0372     ; otherwise branch

; assume the ^A in the filename

0366  A9 CD         LDA #$CD      ; put 'M' in
0368  8D 8A 03      STA $038A     ; the space between BLOAD and MUSA
036B  A9 81         LDA #$81      ; put '^A' in
036D  8D 8B 03      STA $038B     ; the M in MUSA ???
0370  D0 0A         BNE $037C

; assume the ^A NOT in the filename

0372  A9 A0         LDA #$A0      ; put ' ' in
0374  8D 8A 03      STA $038A     ; the space between BLOAD and MUSA
0377  A9 CD         LDA #$CD      ; put 'M' in
0379  8D 8B 03      STA $038B     ; the M in MUSA

037C  A9 00         LDA #$00
037E  20 38 03      JSR $0338
0381  20 1B 08      JSR $081B     ; PRINT routine in SUBS

0384  84 C2 CC CF C1 C4 A0 CD D5 D3 C1 AC C1 A4 C6 B0 B0 B0 8D            ^DBLOAD MUSA,A$F000^M

0397  00
0398  A9 01         LDA #$01
039A  D0 9C         BNE ---

;; turns disk drive motor on for a period of time
; preserves A

039C  48            PHA           ; preserve A
039A  AE E9 B7      LDX $B7E9     ; disk SLOT * 16
039D  BD 89 C0      LDA $C089,X   ; motor on!
03A0  A2 05         LDX $#05      ; X = 0x05

03A2  A0 00         LDY $#00      ; Y = 0x00

03A4  C8            INY           ; Y++
03A5  D0 FD         BNE $03A4     ; repeat until Y == 0x00
03A7  CA            DEX           ; X--
03A8  D0 F8         BNE $03A2     ; repeat until X = 0x00
03AA  AE E9 B7      LDX $B7E9     ; something to do with SLOT * 16
03AD  BD 88 C0      LDA $C088,X   ; motor off
03B0  68            PLA           ; restore A
03B1  60            RTS

03B2  28 00 00
```