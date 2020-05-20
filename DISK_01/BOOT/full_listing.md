---
grand_parent: DISK 1
parent: BOOT
nav_order: 1
---

# BOOT full listing

```
;; home screen set up

6000    20 1B 08    JSR $081B       ; PRINT routine in SUBS

6003    84 C2 CC CF C1 C4 A0 C1 81 CE C9 CD AC C1 A4 B4 B0 B0 B0 8D   ^DBLOAD A^ANIM,A$4000^M
6017    84 C2 CC CF C1 C4 A0 C2 81 C7 CE C4 AC C1 A4 B7 C1 B0 B0 8D   ^DBLOAD B^AGND,A$7A00^M
602B    00

602C    20 89 60    JSR $6089       ; ZERO OUT HIRES PAGE ONE
602F    2C 54 C0    BIT PAGE1       ; select text/graphics page1
6032    2C 57 C0    BIT SETHIRES    ; select Hi-res
6035    2C 52 C0    BIT CLRMIXED    ; clear mixed mode- enable full graphics
6038    2C 50 C0    BIT CLRTEXT     ; display graphics

603B    A9 70       LDA #$70        ; put #$70 in $10
603D    85 10       STA $10         ; .
603F    A9 71       LDA #$71        ; put #$71 in $11
6041    85 11       STA $11         ; .
6043    A9 00       LDA #$00        ; put #$00 in $12
6045    85 12       STA $12         ; .
6047    A2 00       LDX #$00        ; put #$00 in A and X
6049    8A          TXA             ; ? not sure why needed

604A    9D 00 EE    STA $EE00,X     ; zero out $EE00-$EEFF
604D    CA          DEX             ; .
604E    D0 FA       BNE $604A       ; .
```

```
;; home screen main sequence

6050    20 9D 60    JSR $609D       ; animate "Lord British" signature
6053    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
6056    20 C8 60    JSR $60C8       ; animate "and"
6059    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
605C    20 E5 60    JSR $60E5       ; draw a blue line from (0x40, 0x1F) to (0xD8, 0x1F)
605F    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
6062    20 6C 64    JSR $646C       ; "Origin Systems, Inc." scrolling up from blue line
6065    20 43 65    JSR $6543       ; "presents" scrolling down from blue line
6068    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
606B    20 05 61    JSR $6105       ; animate "Ultima IV" @@@
606E    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
6071    20 9F 65    JSR $659F       ; animate "Quest of the Avatar"
6074    20 3B 62    JSR $623B       ; wait-for-key-press with long timeout
6077    20 19 66    JSR $6619       ; animate unfolding of map
607A    20 73 66    JSR $6673       ; lower corner monsters, while animating map
607D    4C 4D 67    JMP $674D       ; set up drive variables then show main menu
```

```
;; DATA

6080    30
6081    0D
6082    DA
6083    08
6084    DB
6085    20
6086    24
6087    38
6088    30
```

```
;; ZERO OUT HIRES PAGE ONE

6089    A2 20       LDX #$20        ; put #$2000
608B    86 81       STX $81         ; in $80-$81
608D    A0 00       LDY #$00        ; #$20 in X
608F    84 80       STY $80         ; #$00 in Y
6091    98          TYA             ; and #$00 in A

6092    91 80       STA ($80),Y     ; zero out
6094    C8          INY             ; 256-bytes of
6095    D0 FB       BNE $6092       ; hires graphics

6097    E6 81       INC $81         ; next 256-bytes
6099    CA          DEX             ; X goes from #$20 to zero
609A    D0 F6       BNE $6092       ;

609C    60          RTS             ;
```

```
;; animate LB signature

; data is pairs of coordinates at $6257

609D    A9 57       LDA #$57        ; put $6257 in $80/$81
609F    85 80       STA $80         ; .
60A1    A9 62       LDA #$62        ; .
60A3    85 81       STA $81         ; .

60A5    A2 00       LDX #$00        ; put #$00 in X
60A7    A1 80       LDA ($80,X)     ; load the x-coordinate

60A9    F0 1C       BEQ $60C7       ; return if zero

60AB    8D 80 60    STA $6080       ; store the x-coordinate in $6080
60AE    20 34 62    JSR $6234       ; increment $80/$81
60B1    A9 BF       LDA #$BF        ; load 0xBF
60B3    38          SEC             ;
60B4    E1 80       SBC ($80,X)     ; subtract y-coordinate from #$BF
60B6    8D 81 60    STA $6081       ; store new y-coordinate in $6081
60B9    20 34 62    JSR $6234       ; increment $80/$81
60BC    20 04 62    JSR $6204       ; draw pixel at x=$6080, y=$6081
60BF    A9 40       LDA #$40        ; A = 0x40
60C1    20 46 62    JSR $6246       ; some kind of wait-for-key press with timeout given by A
60C4    4C A5 60    JMP $60A5       ; loop back to $60A5

60C7    60          RTS             ;
```

```
;; "and"?

; because _61C0 is zero we skip some graphics and sound effects

60C8    A9 00       LDA #$00        ;
60CA    8D C0 61    STA $61C0       ; _61C0 = 0x00
60CD    A9 13       LDA #$13        ;
60CF    8D 82 60    STA $6082       ; _6082 = 0x13    x-coordinate 1
60D2    8D 84 60    STA $6084       ; _6084 = 0x13    x-coordinate 2
60D5    A9 11       LDA #$11        ;
60D7    8D 83 60    STA $6083       ; _6083 = 0x11    y-coordinate 1
60DA    8D 85 60    STA $6085       ; _6085 = 0x11    y-coordinate 2
60DD    A2 04       LDX #$04        ; X = 0x04    number of X byte columns to copy
60DF    A0 04       LDY #$04        ; Y = 0x04    number of Y rows to copy
60E1    20 3E 61    JSR $613E       ; copy rectangle from BGND to video RAM (6082-6085 and X, Y)

60E4    60          RTS             ;
```

```
;; draw a blue line from (0x40, 0x1F) to (0xD8, 0x1F)

60E5    A9 40       LDA #$40        ; set x-coordinate = 0x40
60E7    8D 80 60    STA $6080       ; .
60EA    A9 1F       LDA #$1F        ; set y-coordinate = 0x1F
60EC    8D 81 60    STA $6081       ;

60EF    20 0D 62    JSR $620D       ; draw pixel at $6080, $6081
60F2    A9 30       LDA #$30        ; A = 0x30
60F4    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A
60F7    EE 80 60    INC $6080       ; increment x-coordinate by 2 (so only blue)
60FA    EE 80 60    INC $6080       ; .
60FD    AD 80 60    LDA $6080       ; if x-coordinate < 0xD9
6100    C9 D9       CMP #$D9        ;
6102    90 EB       BCC $60EF       ; loop back

6104    60          RTS             ; otherwise return
```

```
;; animate "Ultima IV"

; effects parameters

6105    A9 FF       LDA #$FF        ; .
6107    8D C0 61    STA $61C0       ; _61C0 = 0xFF
610A    A9 00       LDA #$00        ; .
610C    8D C1 61    STA $61C1       ; _61C1 = 0x00

; loop while _61C1 < 0x39

610F    A9 05       LDA #$05        ;
6111    8D 82 60    STA $6082       ; _6082 = 0x05 (source x)
6114    8D 84 60    STA $6084       ; _6084 = 0x05 (dest x)
6117    A9 22       LDA #$22        ;
6119    8D 83 60    STA $6083       ; _6083 = 0x22 (source y)
611C    8D 85 60    STA $6085       ; _6084 = 0x22 (dest y)

611F    2C 00 C0    BIT $C000       ; if no keypress,
6122    10 05       BPL $6129       ;   skip

6124    A9 38       LDA #$38        ; but if keypress,
6126    8D C1 61    STA $61C1       ;   _61C1 = 0x38 (short cut the loop and do one last copy)

6129    A2 1E       LDX #$1E        ; copy 0x1E byte colums
612B    A0 2D       LDY #$2D        ; copy 0x2D rows

612D    20 3E 61    JSR $613E       ; copy rectangle from BGND to video RAM (6082-6085 and X, Y)

6130    AD C1 61    LDA $61C1       ; .
6133    18          CLC             ; .
6134    69 01       ADC #$01        ; .
6136    8D C1 61    STA $61C1       ; _61C1++
6139    C9 39       CMP #$39        ;
613B    90 D2       BCC $610F       ; if _61C1 < 0x39, loop back

613D    60          RTS             ;
```

```
;; copy rectangle from BGND to video RAM
;
; _6082, _6083 are starting source x, y ($7A00-)
; _6084, _6085 are starting destination x, y ($2000-)
; X is how many byte column to copy
; Y is how many rows to copy
; _61C0 is something to do with the graphics and sound effects to do during copy

613E    8E BF 61    STX $61BF       ; _61BF = X (initially 0x04)
6141    8C 86 60    STY $6086       ; _6086 = Y (initially 0x04)

; outer loops back here with incremented y-coordinates and decremented _6086

6144    AD BF 61    LDA $61BF       ; A = _61BF (initially X, 0x04)
6147    8D 87 60    STA $6087       ; _6087 = A = _61BF (initially X, 0x04)

614A    AC 83 60    LDY $6083       ; Y = _6083 (set by caller, initially 0x11)
614D    B9 00 E0    LDA $E000,Y     ; get LSB of base address for y-coordinate 1
6150    85 80       STA $80         ; store in $80
6152    B9 C0 E0    LDA $E0C0,Y     ; get MSB of base address for y-coordinate 1
6155    18          CLC             ; add 0x5A to it
6156    69 5A       ADC #$5A        ; .
6158    85 81       STA $81         ; and store it in $81

615A    AC 85 60    LDY $6085       ; Y = _6083 (set by caller, initially 0x11)
615D    B9 00 E0    LDA $E000,Y     ; get LSB of base address for y-coordinate 2
6160    85 82       STA $82         ; store in $82
6162    B9 C0 E0    LDA $E0C0,Y     ; get MSB of base address for y-coordinate 2
6165    85 83       STA $83         ; store in $83

6167    AC 84 60    LDY $6084       ; Y = _6084 (set by caller, initially 0x13)

; inner loops back here with incremented Y and decremented _6087

616A    B1 80       LDA ($80),Y     ; load value at the base address (from the y-coordinate 1) plus _6084
616C    29 7F       AND #$7F        ; add it with 0111111
616E    F0 3B       BEQ $61AB       ; if zero, i.e. no pixels set, skip to $61AB

6170    2C C0 61    BIT $61C0       ; Z = $61C0 ^ A
6173    F0 34       BEQ $61A9       ; if zero, we can skip the whole next bit and just copy A

6175    48          PHA             ; preserve A (what was loaded from _6083 / _6084)

6176    AD BD 61    LDA $61BD       ; A = _61BD
6179    69 1D       ADC #$1D        ; A += 0x1D
617B    AA          TAX             ; X = A
617C    6D BE 61    ADC $61BE       ; A += _61BE
617F    8D BD 61    STA $61BD       ; _61BD = original _61DB + 0x1D + _61BE
6182    8E BE 61    STX $61BE       ; _61BE = original _61BD + 0x1D

6185    48          PHA             ; preserve A

6186    29 07       AND #$07        ; _61C2 = A ^ 0000111
6188    8D C2 61    STA $61C2       ;

618B    68          PLA             ; restore A (what was put in _61BD)

618C    18          CLC             ;
618D    6D C1 61    ADC $61C1       ; add_61BD and _61C1
6190    90 03       BCC $6195       ; skip speaker toggle if carry clear

6192    2C 30 C0    BIT $C030       ; SPEAKER TOGGLE

6195    AD C1 61    LDA $61C1       ; A = _61C1
6198    18          CLC             ;
6199    6D C2 61    ADC $61C2       ; A += _61C2
619C    AA          TAX             ; X = A
619D    BD C4 61    LDA $61C4,X     ; A = _61C4[X]
61A0    8D C3 61    STA $61C3       ; _61C3 = A

61A3    68          PLA             ; restore A (what was loaded from _6083 / _6084)

61A4    2D C3 61    AND $61C3       ; and with _C163
61A7    09 80       ORA #$80        ; and OR with 10000000

61A9    91 82       STA ($82),Y     ; write new byte

61AB    C8          INY             ; increment Y (x-coordinate?)
61AC    CE 87 60    DEC $6087       ; decrement the caller's X (initial 0x04)
61AF    D0 B9       BNE $616A       ; loop back if not zero

61B1    EE 83 60    INC $6083       ; increment y-coordinate 1
61B4    EE 85 60    INC $6085       ; increment y-coordinate 2
61B7    CE 86 60    DEC $6086       ; decrement caller's Y (initially 0x04)
61BA    D0 88       BNE $6144       ;
61BC    60          RTS             ;
```

```
;; DATA

61BD    35
61BE    9B
61BF    DC
61C0    20
61C1    24
61C2    31
61C3    45

61C4    00 00 00 00 00 00 00 00
61CC    00 01 02 04 08 12 20 21 10 18 40 24 42 25 14 1A ...... !..@$B%..
61DC    48 29 14 52 54 55 4A 59 45 5A 2A 6C 36 37 6A 67 H).RTUJYEZ*l67jg
61EC    4D 5E 39 6D 3B 6F 57 7D 5D 7E 6B 7B 77 FF 5F 3F M^9m;oW}]~k{w._?
61FC    7F 7F 7F 7F 7F 7F 7F 7F
```

```
;; draw double pixel at x=$6080, y=$6081

6204    EE 80 60    INC $6080       ; temporarily increment x-coordinate by 1
6207    20 0D 62    JSR $620D       ; draw pixel
620A    CE 80 60    DEC $6080       ; decrement x-coordinate again

; draw single pixel

620D    AD 81 60    LDA $6081       ;
6210    A8          TAY             ; Y = y-coordinate
6211    4A          LSR A           ;
6212    A9 00       LDA #$00        ;
6214    6A          ROR A           ;
6215    8D 33 62    STA $6233       ;
6218    B9 00 E0    LDA $E000,Y     ; look up base address for y-coordinate
621B    85 84       STA $84         ; and put in $84/$85
621D    B9 C0 E0    LDA $E0C0,Y     ; .
6220    85 85       STA $85         ; .
6222    AE 80 60    LDX $6080       ; X = x-coordinate
6225    BD 9F E2    LDA $E29F,X     ;
6228    BC 80 E1    LDY $E180,X     ;
622B    11 84       ORA ($84),Y     ;
622D    0D 33 62    ORA $6233       ;
6230    91 84       STA ($84),Y     ; write to video memory
6232    60          RTS             ;
```

```
6233    55                          ; DATA USED ABOVE
```

```
;; increment $80, carrying over to $81 on overflow

6234    E6 80       INC $80         ;
6236    D0 02       BNE $623A       ;

6238    E6 81       INC $81         ;

623A    60          RTS             ;
```

```
;; wait-for-key-press with long timeout

623B    A0 04       LDY #$04        ; Y = 0x04

623D    A9 FF       LDA #$FF        ; A = 0xFF
623F    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A
6242    88          DEY             ; decrease Y
6243    D0 F8       BNE $623D       ; loop until zero

6245    60          RTS             ;
```

```
;; wait-for-key-press with timeout based on A

6246    2C 00 C0    BIT $C000       ; if key press
6249    30 0B       BMI $6256       ; return
624B    38          SEC             ;

624C    48          PHA             ; preserve A

624D    E9 01       SBC #$01        ; reduce A by 1 (with borrow)

624F    D0 FC       BNE $624D       ; if not zero, loop again

6251    68          PLA             ; restore A
6252    E9 01       SBC #$01        ; reduce A by 1 (with borrow)
6254    D0 F6       BNE $624C       ; if not zero, loop again

6256    60          RTS             ;
```

```
;; LORD BRITISH SIGNATURE

; pairs of coordinates

6257                                     54 BD 55 BC 57
625C    BC 59 BC 5B BC 5C BD 5C BE 5B BF 59 BF 58 BE 58
626C    BD 57 BB 56 BA 56 B9 55 B8 55 B7 54 B6 54 B5 53
627C    B4 53 B3 52 B2 52 B1 51 B0 4F B0 4E B0 4D B1 4D
628C    B2 4E B3 50 B3 51 B2 54 B2 55 B1 56 B1 57 B0 59
629C    B0 5B B0 5D B0 5F B0 61 B0 63 B0 65 B0 67 B0 69
62AC    B0 6B B0 6D B0 6F B0 71 B0 73 B0 75 B0 77 B0 79
62BC    B0 7B B0 7D B0 7F B0 81 B0 83 B0 85 B0 87 B0 89
62CC    B0 8B B0 8D B0 8F B0 91 B0 93 B0 95 B0 97 B0 99
62DC    B0 9B B0 9D B0 9F B0 A1 B0 A3 B0 A5 B0 A7 B0 A9
62EC    B0 AB B0 AD B0 AF B0 B1 B0 B3 B0 B5 B0 B7 B0 B9
62FC    B0 BB B0 BD B0 BF B0 C1 B0 C3 B0 C5 B0 C7 B0 C9
630C    B0 CA B0 CB B1 CC B1 CD B2 5E B8 5E B7 5D B6 5D
631C    B5 5C B4 5C B3 5D B2 5F B2 60 B2 61 B3 61 B4 62
632C    B5 62 B6 63 B7 63 B8 62 B9 60 B9 5F B9 69 B9 6A
633C    B8 6A B7 69 B6 69 B5 68 B4 68 B3 67 B2 6B B8 6C
634C    B9 6E B9 77 B9 75 B9 74 B9 73 B8 73 B7 72 B6 72
635C    B5 71 B4 71 B3 72 B2 74 B2 75 B3 76 B4 77 B5 77
636C    B6 78 B7 78 B8 79 B9 79 BA 7A BB 7A BC 7B BD 7B
637C    BE 76 B3 77 B2 8B BE 8B BD 8A BC 8A BB 89 BA 89
638C    B9 88 B8 88 B7 87 B6 87 B5 86 B4 86 B3 85 B2 8C
639C    BF 8E BF 8F BF 90 BE 90 BD 8F BC 8F BB 8E BA 8E
63AC    B9 8C B9 8F B8 8F B7 8E B6 8E B5 8D B4 8D B3 8C
63BC    B2 8A B2 88 B2 87 B3 96 B9 97 B8 97 B7 96 B6 96
63CC    B5 95 B4 95 B3 94 B2 98 B8 99 B9 9B B9 A1 B9 A0
63DC    B8 A0 B7 9F B6 9F B5 9E B4 9E B3 9D B2 A2 BC A2
63EC    BB A9 BC A9 BB A8 BA A8 B9 A7 B8 A7 B7 A6 B6 A6
63FC    B5 A5 B4 A5 B3 A6 B2 A8 B2 A9 B2 AA B3 A5 B9 A6
640C    B9 AA B9 B2 B9 B1 B8 B1 B7 B0 B6 B0 B5 AF B4 AF
641C    B3 AE B2 B3 BC B3 BB BB B8 BA B9 B8 B9 B7 B9 B6
642C    B8 B6 B7 B7 B6 B8 B5 B9 B4 B9 B3 B8 B2 B6 B2 B5
643C    B2 B4 B3 C5 BE C5 BD C4 BC C4 BB C3 BA C3 B9 C2
644C    B8 C2 B7 C1 B6 C1 B5 C0 B4 C0 B3 BF B2 C5 B9 C6
645C    B9 C7 B8 C7 B7 C6 B6 C6 B5 C5 B4 C5 B3 C6 B2 00
```

```
;; "Origin Systems, Inc." scrolling up from blue line

; we want to copy from $7A00 (redundant?)

646C    A9 5A       LDA #$5A        ;
646E    8D 88 60    STA $6088       ; _6088 = 0x5A

6471    A9 00       LDA #$00        ;
6473    8D CD 64    STA $64CD       ; _64CD = 0x00

; loop 1: _64CD runs from 0x00 to 0x08 (inclusive)

6476    A9 00       LDA #$00        ;
6478    8D CE 64    STA $64CE       ; _64CE = 0x00

; loop 2: _64CE runs from 0x00 to _64CD (inclusive)

647B    A9 09       LDA #$09        ;
647D    8D CF 64    STA $64CF       ; _64CF = 0x09

; loop 3: _64CF runs from 0x09 to 0x1E (inclusive)

; the source and dest x byte is _64CF
; the source y is _64CE + 0x15
; the dest y is _64CE + 0x1D - _64CD

6480    AD CF 64    LDA $64CF       ; .
6483    8D 82 60    STA $6082       ; _6082 = _64CF (source x)
6486    8D 84 60    STA $6084       ; _6084 = _64CF (dest x)
6489    A9 15       LDA #$15        ; .
648B    18          CLC             ; .
648C    6D CE 64    ADC $64CE       ; .
648F    8D 83 60    STA $6083       ; _6083 = _64CE + 0x15 (source y)
6492    A9 1D       LDA #$1D        ; .
6494    18          CLC             ; .
6495    6D CE 64    ADC $64CE       ; .
6498    38          SEC             ; .
6499    ED CD 64    SBC $64CD       ; .
649C    8D 85 60    STA $6085       ; _6085 = _64CE + 0x1D - 0x64CD (dest y)

649F    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6085)

64A2    EE CF 64    INC $64CF       ; _64CF++
64A5    AD CF 64    LDA $64CF       ;
64A8    C9 1E       CMP #$1E        ;
64AA    90 D4       BCC $6480       ; if _64CF <= 0x1E,
64AC    F0 D2       BEQ $6480       ;   go to $6480, else...
64AE    EE CE 64    INC $64CE       ; _64CE++
64B1    AD CE 64    LDA $64CE       ;
64B4    CD CD 64    CMP $64CD       ;
64B7    90 C2       BCC $647B       ; if _64CE <= _64CD,
64B9    F0 C0       BEQ $647B       ;   go to $647B, else...

64BB    A9 C0       LDA #$C0        ; .
64BD    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A = 0xC0

64C0    EE CD 64    INC $64CD       ; _64CD++
64C3    AD CD 64    LDA $64CD       ;
64C6    C9 08       CMP #$08        ;
64C8    90 AC       BCC $6476       ; if _64CD <= 0x08,
64CA    F0 AA       BEQ $6476       ;   go to $6476, else...

64CC    60          RTS             ; return
```

```
;; DATA

64CD    44
64CE    54
64CF    20
```

```
;;

; enter at $64D0 to copy from $7A00- or $64D5 to copy from $4000-
; copy x1, y1 to x2, y2
; x1 = $6082
; y1 = $6083
; x2 = $6084
; y2 = $6085

64D0    A9 5A       LDA #$5A        ;
64D2    4C D7 64    JMP $64D7       ;
```

```
64D5    A9 20       LDA #$20        ;

64D7    8D 88 60    STA $6088       ; _6088 = 0x5A or 0x20 (depending on entry point)

; source in $80/$81

64DA    AC 83 60    LDY $6083       ; .
64DD    B9 00 E0    LDA $E000,Y     ; .
64E0    85 80       STA $80         ; $80 = LSB for y-coordinate 1 in $6083
64E2    B9 C0 E0    LDA $E0C0,Y     ; .
64E5    18          CLC             ; .
64E6    6D 88 60    ADC $6088       ; .
64E9    85 81       STA $81         ; $81 = MSB for y-coordinate 1 in $6083 with either 0x5a or 0x20 added (extra page or 2nd page)

; destination in $82/83

64EB    AC 85 60    LDY $6085       ; .
64EE    B9 00 E0    LDA $E000,Y     ; .
64F1    85 82       STA $82         ; $82 = LSB for y-coordinate 2 in $6085
64F3    B9 C0 E0    LDA $E0C0,Y     ; .
64F6    85 83       STA $83         ; $83 = MSB for y-coordinate 2 in $6085


64F8    AC 82 60    LDY $6082       ; .
64FB    B1 80       LDA ($80),Y     ; copy from x, y 1 (x1 in $6082)
64FD    AC 84 60    LDY $6084       ; .
6500    91 82       STA ($82),Y     ; to x, y 2 (x2 in $6084)

6502    60          RTS             ;
```

```
;;

6503    AC 83 60    LDY $6083       ;
6506    B9 00 E0    LDA $E000,Y     ;
6509    85 80       STA $80         ;
650B    B9 C0 E0    LDA $E0C0,Y     ;
650E    18          CLC             ;
650F    6D 88 60    ADC $6088       ;
6512    85 81       STA $81         ;
6514    AC 82 60    LDY $6082       ;
6517    B1 80       LDA ($80),Y     ;
6519    60          RTS             ;
```

```
;; DATA?

651A    8A 48
651C    18 A2 06 AD 42 65 7D 3B 65 9D 3B 65 CA 10 F7 A2
652C    07 FE 3B 65 D0 03 CA 10 F8 68 AA AD 3B 65 60 09
653C    0A 0B 0C 0D 0E 0F 10
```

```
;; "presents" scrolling down from blue line

6543    A9 00       LDA #$00        ;
6545    8D CD 64    STA $64CD       ;

; loop 1: _64CD runs from 0x00 to 0x04 (inclusive)

6548    A9 00       LDA #$00        ;
654A    8D CE 64    STA $64CE       ;

; loop 2: _64CE runs from 0x00 to _64CD (inclusive)

654D    A9 10       LDA #$10        ;
654F    8D CF 64    STA $64CF       ;

; loop 3: _64CF runs from 0x10 to 0x18 (inclusive)

6552    AD CF 64    LDA $64CF       ; .
6555    8D 82 60    STA $6082       ; _6082 = _64CF (source x)
6558    8D 84 60    STA $6084       ; _6084 = _64CF (dest x)
655B    A9 04       LDA #$04        ; .
655D    18          CLC             ; .
655E    6D CE 64    ADC $64CE       ; .
6561    38          SEC             ; .
6562    ED CD 64    SBC $64CD       ; .
6565    8D 83 60    STA $6083       ; _6083 = _64CE + 0x14 + 0x64CD (source x)
6568    A9 21       LDA #$21        ;
656A    18          CLC             ;
656B    6D CE 64    ADC $64CE       ;
656E    8D 85 60    STA $6085       ; _6085 = _64CE + 0x21 (dest y)

6571    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6065)

6574    EE CF 64    INC $64CF       ; _64CF++
6577    AD CF 64    LDA $64CF       ;
657A    C9 18       CMP #$18        ;
657C    90 D4       BCC ---         ; if _64CF <= 0x18
657E    F0 D2       BEQ ---         ;
6580    EE CE 64    INC $64CE       ; _64CE++
6583    AD CE 64    LDA $64CE       ;
6586    CD CD 64    CMP $64CD       ;
6589    90 C2       BCC ---         ; if _64CE <= _64CD,
658B    F0 C0       BEQ ---         ;

658D    A9 C0       LDA #$C0        ; .
658F    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A = 0xC0

6592    EE CD 64    INC $64CD       ; _64CD++
6595    AD CD 64    LDA $64CD       ; .
6598    C9 04       CMP #$04        ; .
659A    90 AC       BCC ---         ; if _64CD <= 0x04
659C    F0 AA       BEQ ---         ;

659E    60          RTS             ; return
```

```
;; animate "Quest of the Avatar"

; we want to copy from $7A00 (redundant?)

659F    A9 5A       LDA #$5A        ;
65A1    8D 88 60    STA $6088       ;

65A4    A9 00       LDA #$00        ;
65A6    8D CD 64    STA $64CD       ;

; loop 1: _64CD runs from 0x00 to 0x05 (inclusive)

65A9    A9 00       LDA #$00        ;
65AB    8D CE 64    STA $64CE       ;

; loop 2: _64CE runs from 0x00 to _64CD (inclusive)

65AE    A9 03       LDA #$03        ;
65B0    8D CF 64    STA $64CF       ;

; loop 3: _64CF runs from 0x03 to 0x24 (inclusive)

65B3    AD CF 64    LDA $64CF       ;
65B6    8D 82 60    STA $6082       ; _6082 = _64CF (source x)
65B9    8D 84 60    STA $6084       ; _6084 = _64CF (dest x)

; first pixel

65BC    A9 51       LDA #$51        ;
65BE    18          CLC             ;
65BF    6D CE 64    ADC $64CE       ;
65C2    8D 83 60    STA $6083       ; _6083 = _64CE + 0x51 (source y)
65C5    A9 56       LDA #$56        ;
65C7    18          CLC             ;
65C8    6D CE 64    ADC $64CE       ;
65CB    38          SEC             ;
65CC    ED CD 64    SBC $64CD       ;
65CF    8D 85 60    STA $6085       ; _6085 = _64CE + 0x56 - 0x64CD (dest y)

65D2    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6065)

; second pixel

65D5    A9 5C       LDA #$5C        ;
65D7    18          CLC             ;
65D8    6D CE 64    ADC $64CE       ;
65DB    38          SEC             ;
65DC    ED CD 64    SBC $64CD       ;
65DF    8D 83 60    STA $6083       ; _6083 = _64CE + 0x5C - 0x64DC (source y)
65E2    A9 57       LDA #$57        ;
65E4    18          CLC             ;
65E5    6D CE 64    ADC $64CE       ;
65E8    8D 85 60    STA $6085       ; _6085 = _64CE + 0x57

65EB    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6065)

65EE    EE CF 64    INC $64CF       ; _64CF++
65F1    AD CF 64    LDA $64CF       ;
65F4    C9 24       CMP #$24        ;
65F6    90 BB       BCC ---         ; if _64CF <= 0x24
65F8    F0 B9       BEQ ---         ;
65FA    EE CE 64    INC $64CE       ;
65FD    AD CE 64    LDA $64CE       ;
6600    CD CD 64    CMP $64CD       ;
6603    90 A9       BCC ---         ;
6605    F0 A7       BEQ ---         ;

6607    A9 C0       LDA #$C0        ;
6609    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A = 0xC0

660C    EE CD 64    INC $64CD       ;
660F    AD CD 64    LDA $64CD       ;
6612    C9 05       CMP #$05        ; if _64CD <= 0x05
6614    90 93       BCC ---         ;
6616    F0 91       BEQ ---         ;
6618    60          RTS             ;
```

```
;; animate unrolling of map

; we want to copy from $7A00 (redundant?)

6619    A9 5A       LDA #$5A        ;
661B    8D 88 60    STA $6088       ;

661E    A9 00       LDA #$00        ;
6620    8D CE 64    STA $64CE       ;

; loop 1: _64CE runs from 0x00 to 0x13 (inclusive)

6623    A9 60       LDA #$60        ;
6625    8D CF 64    STA $64CF       ;

; loop 2: _64CF runs from 0x60 to 0xBF (inclusive)

6628    AD CF 64    LDA $64CF       ;
662B    8D 83 60    STA $6083       ; _6083 = _64CF (source y)
662E    8D 85 60    STA $6085       ; _6085 = _64CF (dest y)

; first pixel

6631    A9 13       LDA #$13        ;
6633    38          SEC             ;
6634    ED CE 64    SBC $64CE       ;
6637    8D 82 60    STA $6082       ; _6082 = _64CE - 0x13 (source x)

663A    A9 13       LDA #$13        ;
663C    38          SEC             ;
663D    ED CE 64    SBC $64CE       ;
6640    8D 84 60    STA $6084       ; _6083 = _64CE - 0x13 (dest x)

6643    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6065)

; second pixel

6646    A9 14       LDA #$14        ;
6648    18          CLC             ;
6649    6D CE 64    ADC $64CE       ;
664C    8D 82 60    STA $6082       ; _6082 = _64CE + 0x14 (source x)
664F    8D 84 60    STA $6084       ; _6082 = _64CE + 0x14 (dest x)

6652    20 D0 64    JSR $64D0       ; copy pixels byte from ($6082,$6083) on $7A00 to ($6084,$6065)

6655    EE CF 64    INC $64CF       ; _64CF++
6658    AD CF 64    LDA $64CF       ;
665B    C9 BF       CMP #$BF        ;
665D    90 C9       BCC ---         ; if _64CF <= 0xBF
665F    F0 C7       BEQ ---         ;

6661    A9 C0       LDA #$C0        ; .
6663    20 46 62    JSR $6246       ; wait-for-key-press with timeout based on A = 0xC0

6666    EE CE 64    INC $64CE       ; _64CE++
6669    AD CE 64    LDA $64CE       ;
666C    C9 13       CMP #$13        ;
666E    90 B3       BCC ---         ; if _64CE <= 0x13
6670    F0 B1       BEQ ---         ;

6672    60          RTS             ;
```

```
;; lower corner monsters, while animating map ??

6673    A9 E1       LDA #$E1        ;
6675    8D 81 60    STA $6081       ; _6081 = 0xE1

6678    A9 1E       LDA #$1E        ;
667A    8D DB 6C    STA $6CDB       ; _6CDB = 0x1E
667D    8D DC 6C    STA $6CDC       ; _6CDB = 0x1E

; loop from _6081 is 0xE1 forward to 0x00 inclusive

6680    20 9D 66    JSR $669D       ; lower the corner monsters?
6683    20 35 67    JSR $6735       ; do some bit-fiddling with _6CDB and _6CDC
6686    20 06 6E    JSR $6E06       ; draw minimap on homescreen
6689    20 06 6E    JSR $6E06       ; draw minimap on homescreen (not sure why done twice)

668C    2C 00 C0    BIT $C000       ; if no keypress,
668F    10 01       BPL $6692       ;   continue

6691    60          RTS             ; if keypress, return
```

```
6692    EE 81 60    INC $6081       ; _6081++
6695    AD 81 60    LDA $6081       ; .
6698    C9 01       CMP #$01        ; .
669A    D0 E4       BNE $6680       ; if _6081 != 0x01, loop back

669C    60          RTS             ;
```

```
;; lower the corner monsters?

669D    AE DB 6C    LDX $6CDB       ; value of _6CDB
66A0    BC DD 6C    LDY $6CDD,X     ;   is used as offset into _6CDD
66A3    B9 A7 6D    LDA $6DA7,Y     ;     and value of item in _6CDD[] is used as offset into _6DA7
66A6    8D D0 66    STA $66D0       ; that is stored as ADC operand below
66A9    B9 95 6D    LDA $6D95,Y     ; and value in _6D95
66AC    8D DF 66    STA $66DF       ; is stored as other ADC operand below

66AF    AE DC 6C    LDX $6CDC       ; then, the value of _6CDC
66B2    BC 55 6D    LDY $6D55,X     ;   is used as offset into the _6D55 array
66B5    B9 CB 6D    LDA $6DCB,Y     ;     whose value is used as offset into _6DCB array
66B8    8D F7 66    STA $66F7       ; and that is stored as yet another ADC operand below
66BB    B9 B9 6D    LDA $6DB9,Y     ; and value in _6DB9 array
66BE    8D 0D 67    STA $670D       ; is stored as one more ADC operand below

66C1    A9 00       LDA #$00        ; .
66C3    8D CD 64    STA $64CD       ; _64CD = 0x00

; loop 1: _64CD from 0x00 to 0x1F exclusive

66C6    A9 00       LDA #$00        ; .
66C8    8D CE 64    STA $64CE       ; _64CE = 0x00

; loop 2: _64CE from 0x00 to 0x07 exclusive

66CB    AD CE 64    LDA $64CE       ; .
66CE    18          CLC             ; .
66CF    69 FF       ADC #$__        ; .
66D1    8D 82 60    STA $6082       ; start x = _64CE + _6DA7[_6CDD[_6CDB]]

66D4    AD CE 64    LDA $64CE       ; .
66D7    8D 84 60    STA $6084       ; end x = _64CE

66DA    AD CD 64    LDA $64CD       ; .
66DD    18          CLC             ; .
66DE    69 FF       ADC #$__        ; .
66E0    8D 83 60    STA $6083       ; start y = _64CD + _6D95[_6CDD[_6CDB]]

66E3    AD CD 64    LDA $64CD       ; .
66E6    18          CLC             ; .
66E7    6D 81 60    ADC $6081       ; .

66EA    30 06       BMI $66F2       ; skip if high bit set

66EC    8D 85 60    STA $6085       ; end y = _6DCD + _6081
66EF    20 D5 64    JSR $64D5       ; copies ($6082,$6083) to ($6084,$6085) from $4000

66F2    AD CE 64    LDA $64CE       ;
66F5    18          CLC             ;
66F6    69 FF       ADC #$__        ;
66F8    8D 82 60    STA $6082       ; start x = _64CE + _6DCB[_6D55[_6CDC]]

66FB    AD CE 64    LDA $64CE       ; .
66FE    18          CLC             ; .
66FF    69 22       ADC #$22        ; .
6701    C9 28       CMP #$28        ;
6703    B0 1B       BCS $6720       ; skip @@@

6705    8D 84 60    STA $6084       ; end x = _64CE + 0x22

6708    AD CD 64    LDA $64CD       ; .
670B    18          CLC             ; .
670C    69 FF       ADC #$__        ; .
670E    8D 83 60    STA $6083       ; start y = _64CD + _6DB9[_6D55[_6CDC]]

6711    AD CD 64    LDA $64CD       ; .
6714    18          CLC             ; .
6715    6D 81 60    ADC $6081       ; .
6718    30 06       BMI $6720       ; skip if high bit set

671A    8D 85 60    STA $6085       ; end y = _64CD + _6081
671D    20 D5 64    JSR $64D5       ; copies ($6082,$6083) to ($6084,$6085) from $4000

6720    EE CE 64    INC $64CE       ; loop 2
6723    AD CE 64    LDA $64CE       ; .
6726    C9 07       CMP #$07        ; .
6728    90 A1       BCC $66CB       ; .

672A    EE CD 64    INC $64CD       ; loop 1
672D    AD CD 64    LDA $64CD       ; .
6730    C9 1F       CMP #$1F        ; .
6732    90 92       BCC $66C6       ; .

6734    60          RTS             ;
```

```
;; do some bit-fiddling with _6CDB and _6CDC

6735    EE DB 6C    INC $6CDB       ; _6CDB++
6738    AD DB 6C    LDA $6CDB       ;
673B    29 7F       AND #$7F        ;
673D    8D DB 6C    STA $6CDB       ; _6CDB ^= 01111111

6740    EE DC 6C    INC $6CDC       ; _6CDC++
6743    AD DC 6C    LDA $6CDC       ;
6746    29 3F       AND #$3F        ;
6748    8D DC 6C    STA $6CDC       ; _6CDC ^= 00111111

674B    60          RTS             ;
```

```
;; something to do with Mockingboard card

674C    00
```

```
;; set up drive variables then show menu

674D    A9 00       LDA #$00        ;
674F    85 0B       STA $0B         ; _0B = 0x00 ???
6751    85 D3       STA $D3         ; _D3 = 0x00 (disk number in drive 2)
6753    A9 01       LDA #$01        ;
6755    85 D1       STA $D1         ; _D1 = 0x01 (??? number of drives ???)
6757    85 D2       STA $D2         ; _D2 = 0x01 (disk number in drive 1)
6759    20 08 6C    JSR $6C08       ; why?

;; 'R' pressed (Return to the view)

675C    20 09 6C    JSR $6C09       ;
675F    08          PHP             ;
6760    58          CLI             ;
6761    A5 82       LDA $82         ; _81 = _82
6763    85 81       STA $81         ; .
6765    28          PLP             ;
6766    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page

;; MAIN MENU

6769    A9 10       LDA #$10        ; store 0x10
676B    8D 15 6B    STA $6B15       ;  in (secondary) text row
676E    A9 17       LDA #$17        ; store 0x17
6770    8D 14 6B    STA $6B14       ;  in (secondary) text column

6773    A2 02       LDX #$02        ; X = 0x02 (text column)
6775    A0 0E       LDY #$0E        ; Y = 0x0E (text row)
6777    20 1E 08    JSR $081E       ; PRINT

677A    C9 CE A0 C1 CE CF D4 C8 C5 D2 A0 D7 CF D2 CC C4 AC A0                   IN ANOTHER WORLD,
678C    C9 CE A0 C1 A0 D4 C9 CD C5 A0 D4 CF A0 C3 CF CD C5 AE 00                IN A TIME TO COME.^@

679F    A2 0F       LDX #$0F        ; X = 0x0F (text column)
67A1    A0 10       LDY #$10        ; Y = 0x10 (text row)
67A3    20 1E 08    JSR $081E       ; PRINT

67A6    CF D0 D4 C9 CF CE D3 BA 00                                              OPTIONS:^@

67AF    A2 0B       LDX #$0B        ; X = 0x0B (text column)
67B1    A0 11       LDY #$11        ; Y = 0x11 (text row)
67B3    20 1E 08    JSR $081E       ; PRINT

67B6    D2 E5 F4 F5 F2 EE A0 F4 EF A0 F4 E8 E5 A0 F6 E9 E5 F7 00                Return to the view^@

67B9    A2 0B       LDX #$0B        ; X = 0x0B (text column)
67BB    A0 12       LDY #$12        ; Y = 0x12 (text row)
67BD    20 1E 08    JSR $081E       ; PRINT

; $67E1 is the number 1

67CC    CE F5 ED E2 E5 F2 A0 EF E6 A0 E4 F2 E9 F6 E5 F3 AD B1 00                Number of drives-1^@

67E3    A2 0D       LDX #$0D        ; X = 0x0D (text column)
67E5    A0 13       LDY #$13        ; Y = 0x13 (text row)
67E7    20 1E 08    JSR $081E       ; PRINT

67EA    CA EF F5 F2 EE E5 F9 A0 EF EE F7 E1 F2 E4 00                            Journey onward^@

67F9    A2 0B       LDX #$0B        ; X = 0x0B (text column)
67FB    A0 14       LDY #$14        ; Y = 0x14 (text row)
67FD    20 1E 08    JSR $081E       ; PRINT

6800    C9 EE E9 F4 E9 E1 F4 E5 A0 EE E5 F7 A0 E7 E1 ED E5 00                   Initiate new game^@

6812    2C 4C 67    BIT $674C       ; if high bit of _674C is set (something to do with Mockingboard card)
6815    30 1D       BMI $6834       ; skip

6817    A2 09       LDX #$09        ; X = 0x09 (text column)
6819    A0 15       LDY #$15        ; Y = 0x15 (text row)
681B    20 1E 08    JSR $081E       ; PRINT

681E    C1 E3 F4 E9 F6 E1 F4 E5 A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 00       Activate Mockingboard^@

6834    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
6837    C9 B0       CMP #$B0        ; if key < 0xB0
6839    90 F9       BCC $6834       ;   prompt again
683B    C9 BA       CMP #$BA        ; if key >= 0xBA
683D    B0 09       BCS $6848       ;   skip

; key a number between 0 and 9

683F    38          SEC             ;
6840    E9 B0       SBC #$B0        ; convert key code to actual number
6842    20 74 6A    JSR $6A74       ; ???
6845    4C 34 68    JMP $6834       ; jump back to prompt for a character
```

```
6848    C9 D4       CMP #$D4        ; if key == 'T'
684A    F0 10       BEQ $685C       ;   go to $685C

684C    C9 CF       CMP #$CF        ; if key == 'O'
684E    F0 0C       BEQ $685C       ;   go to $685C

6850    C9 C4       CMP #$C4        ; if key == 'D'
6852    F0 08       BEQ $685C       ;   go to $685C

6854    C9 C2       CMP #$C2        ; if key == 'B'
6856    F0 04       BEQ $685C       ;   go to $685C

6858    C9 C3       CMP #$C3        ; if key != 'C'
685A    D0 06       BNE $6862       ;   skip to $6862

685C    20 74 6A    JSR $6A74       ; ???
685F    4C 34 68    JMP $6834       ; jump back to prompt for a character
```

```
; not T, O, D, B, or C

6862    C9 D2       CMP #$D2        ; if key != 'R'
6864    D0 03       BNE $6869       ;   skip

6866    4C 5C 67    JMP $675C       ; 'R' pressed (Return to the view)
```

```
6869    C9 CE       CMP #$CE        ; if key != 'N'
686B    D0 03       BNE $6870       ;   skip

686D    4C DD 6F    JMP $6FDD       ; 'N' pressed (Number of drives-1)
```

```
6870    C9 CA       CMP #$CA        ; if key != 'J'
6872    D0 03       BNE $6877       ;   skip

6874    4C 16 6B    JMP $6B16       ; 'J' pressed (Journey onward)
```

```
6877    C9 C9       CMP #$C9        ; if key != 'I'
6879    D0 03       BNE $687E       ;   skip

687B    4C 38 6B    JMP $6B38       ; 'I' pressed (Initiate new game)
```

```
687E    C9 C1       CMP #$C1        ; if key != 'A' (Activate Mockingboard)
6880    D0 B2       BNE ---         ;
6882    2C 4C 67    BIT $674C       ; (something to do with Mockingboard card)
6885    30 AD       BMI ---         ;
6887    A9 00       LDA #$00        ;
6889    20 74 6A    JSR $6A74       ; ???
688C    AD 81 C0    LDA $C081       ;
688F    AD B3 FB    LDA $FBB3       ;
6892    AE C0 FB    LDX $FBC0       ;
6895    2C 8B C0    BIT $C08B       ;
6898    2C 8B C0    BIT $C08B       ;
689B    C9 06       CMP #$06        ;
689D    D0 04       BNE $68A3       ;

689F    E0 00       CPX #$00        ;
68A1    F0 91       BEQ ---         ;

68A3    A9 00       LDA #$00        ; .
68A5    85 80       STA $80         ; _80 = 0x00
68A7    85 81       STA $81         ; _81 = 0x00
68A9    85 82       STA $82         ; _82 = 0x00
68AB    85 83       STA $83         ; _83 = 0x00
68AD    EE 14 6B    INC $6B14       ; increment (secondary) text column
68B0    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page

68B3    A2 0F       LDX #$0F        ; X = 0x0F
68B5    A0 10       LDY #$10        ; Y = 0x10
68B7    20 1E 08    JSR $081E       ; PRINT

68BA    C8 EF F7 A0 ED E1 EE F9 BF 00                                           How many?^@

68C4    A2 0C       LDX #$0C        ; X = 0x0C
68C6    A0 12       LDY #$12        ; Y = 0x12
68C8    20 1E 08    JSR $081E       ; PRINT

68CB    CF EE E5 A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 00                      One Mockingboard^@

68DC    A2 0C       LDX #$0C        ;
68DE    A0 13       LDY #$13        ;
68E0    20 1E 08    JSR $081E       ;

68E3    D4 F7 EF A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 F3 00                   Two Mockingboards^@

68F5    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
68F8    C9 CF       CMP #$CF        ;
68FA    F0 0B       BEQ $6907       ;
68FC    C9 D4       CMP #$D4        ;
68FE    F0 0B       BEQ $690B       ;
6900    C9 9B       CMP #$9B        ;
6902    D0 F1       BNE ---         ;
6904    4C 66 67    JMP $6766       ;
```

```
6907    A9 00       LDA #$00        ;
6909    F0 02       BEQ $690D       ;

690B    A9 80       LDA #$80        ;

690D    85 B0       STA $B0         ;

690F    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page
6912    20 37 6A    JSR $6A37       ;

6915    A0 10       LDY #$10        ;
6917    A2 0D       LDX #$0D        ;
6919    20 1E 08    JSR $081E       ;

691C    D7 E8 E9 E3 E8 A0 EB E9 EE E4 BF 00                                     Which kind?^@

6928    A0 12       LDY #$12        ;
692A    A2 0A       LDX #$0A        ;
692C    20 1E 08    JSR $081E       ;

692F    B1 AD A0 D3 EF F5 EE E4 A0 B1 00                                        1- Sound 1^@

693A    A0 13       LDY #$13        ;
693C    A2 0A       LDX #$0A        ;
693E    20 1E 08    JSR $081E       ;

6941    B2 AD A0 D3 EF F5 EE E4 AD D3 F0 E5 E5 E3 E8 00                         2- Sound-Speech^@

6951    A0 14       LDY #$14        ;
6953    A2 0A       LDX #$0A        ;
6955    20 1E 08    JSR $081E       ;

6958    B3 AD A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 A0 C1 A0 EF F2 A0 C3 00    3- Mockingboard A or C^@

696F    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
6972    C9 9B       CMP #$9B        ;
6974    D0 03       BNE $6979       ;

6976    4C 66 67    JMP $6766       ;
```

```
6979    C9 B1       CMP #$B1        ;
697B    90 F2       BCC ---         ;
697D    C9 B4       CMP #$B4        ;
697F    B0 EE       BCS ---         ;
6981    38          SEC             ;
6982    E9 B0       SBC #$B0        ;
6984    A8          TAY             ;
6985    A5 B0       LDA $B0         ;
6987    29 02       AND #$02        ;
6989    AA          TAX             ;
698A    94 81       STY $81,X       ;
698C    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page
698F    20 37 6A    JSR $6A37       ;

6992    A0 10       LDY #$10        ;
6994    A2 0D       LDX #$0D        ;
6996    20 1E 08    JSR $081E       ;

6999    D7 E8 E9 E3 E8 A0 F3 EC EF F4 BF 00                                     Which slot?^@

69A5    A0 13       LDY #$13        ;
69A7    A2 0B       LDX #$0B        ;
69A9    20 1E 08    JSR $081E       ;

69AC    C5 EE F4 E5 F2 A0 E1 A0 EE F5 ED E2 E5 F2 A0 B1 AD B7 00                Enter a number 1-7^@

69BF    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
69C2    C9 9B       CMP #$9B        ;
69C4    D0 03       BNE $69C9       ;

69C6    4C 66 67    JMP $6766       ;
```

```
69C9    C9 B1       CMP #$B1        ;
69CB    90 F2       BCC ---         ;
69CD    C9 B8       CMP #$B8        ;
69CF    B0 EE       BCS ---         ;
69D1    38          SEC             ;
69D2    E9 B0       SBC #$B0        ;
69D4    A8          TAY             ;
69D5    A5 B0       LDA $B0         ;
69D7    29 02       AND #$02        ;
69D9    AA          TAX             ;
69DA    94 80       STY $80,X       ;
69DC    24 B0       BIT $B0         ;
69DE    10 0D       BPL $69ED       ;
69E0    E6 B0       INC $B0         ;
69E2    E6 B0       INC $B0         ;
69E4    A5 B0       LDA $B0         ;
69E6    C9 84       CMP #$84        ;
69E8    B0 03       BCS $69ED       ;

69EA    4C 0F 69    JMP $690F       ;
```

```
69ED    20 1B 08    JSR $081B       ;

69F0    84 C2 CC CF C1 C4 A0 CD 81 C2 D3 CD AC C1 A4 C6 B0 B0 B0 8D             ^DBLOAD M^ABSM,A$F000^M
6A04    84 C2 CC CF C1 C4 A0 CD 81 C2 D3 C9 AC C1 A4 B8 B0 B0 B0 8D 00          ^DBLOAD M^ABSI,A$8000^M^@

6A19    20 00 80    JSR $8000       ;
6A1C    90 16       BCC #6A34       ;
6A1E    A9 4C       LDA #$4C        ;
6A20    8D 20 03    STA $0320       ;
6A23    A9 00       LDA #$00        ;
6A25    85 82       STA $82         ;
6A27    A9 D4       LDA #$D4        ;
6A29    20 74 6A    JSR $6A74       ; ???
6A2C    A9 01       LDA #$01        ;
6A2E    20 74 6A    JSR $6A74       ; ???
6A31    CE 4C 67    DEC $674C       ; (something to do with Mockingboard card)

6A34    4C 66 67    JMP $6766       ;
```

```
6A37    A0 0E       LDY #$0E        ;
6A39    A2 08       LDX #$08        ;
6A3B    A5 B0       LDA $B0         ;
6A3D    10 1B       BPL $6A5A       ;
6A3F    29 02       AND #$02        ;
6A41    D0 18       BNE $6A5B       ;
6A43    20 1E 08    JSR $081E       ;

6A46    C6 E9 F2 F3 F4 A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 BA 00             First Mockingboard:^@

6A5A    60          RTS             ;
```

```
6A5B    20 1E 08    JSR $081E       ;

6A5E    D3 E5 E3 EF EE E4 A0 CD EF E3 EB E9 EE E7 E2 EF E1 F2 E4 BA 00          Second Mockingboard:^@

6A73    60          RTS             ;
```

```
;;

6A74    AA          TAX             ; X = A
6A75    10 0B       BPL $6A82       ; if >= 0, skip to $6A82
6A77    8D 89 6A    STA $6A89       ; _6A89 = A
6A7A    A2 01       LDX #$01        ; X = 0x01
6A7C    8E 88 6A    STX $6A88       ; _6A88 = 0x01
6A7F    4C 20 03    JMP $0320       ; some to do with SEL?
```

```
6A82    8D 88 6A    STA $6A88       ; _6A88 = A
6A85    4C 20 03    JMP $0320       ; some to do with SEL?
```

```
;; DATA

6A88    FF
6A89    FF

6A8A    CF D4 C4 C3 C2 00
6A90    01 02 03 00 01 00 00 04 01 00 00 00 01 00 00 00 01 00 00 00 FF

;;

6AA5    A5 82       LDA $82         ; if _82 == _81
6AA7    C5 81       CMP $81         ; .
6AA9    F0 01       BEQ $6AAC       ;   continue

6AAB    60          RTS             ; otherwise return
```

```
6AAC    C9 00       CMP #$00        ; if _82 == 0x00
6AAE    F0 03       BEQ $6AB3       ; skip

6AB0    8D 88 6A    STA $6A88       ; otherwise _6A88 == _82

6AB3    A2 00       LDX #$00        ; X = 0x00
6AB5    AD 89 6A    LDA $6A89       ; A = _6A89
6AB8    DD 8A 6A    CMP $6A8A,X     ; if _6A89 == _6A8A[X]
6ABB    F0 03       BEQ $6AC0       ; skip

6ABD    E8          INX             ; otherwise X++
6ABE    D0 F8       BNE $6AB8       ; loop back until _6A89 == _6A8A[X]

6AC0    8A          TXA             ; A = X
6AC1    0A          ASL A           ; A = 4 * A
6AC2    0A          ASL A           ; .
6AC3    18          CLC             ; .
6AC4    6D 88 6A    ADC $6A88       ; A += _6A88
6AC7    AA          TAX             ; X = A
6AC8    BD 90 6A    LDA $6A90,X     ; A = _6A90 + X
6ACB    D0 03       BNE $6AD0       ; if not zero, skip

6ACD    E8          INX             ; X++
6ACE    D0 F8       BNE $6AC8       ; loop back to test _6A90 + X until it's zero

6AD0    10 04       BPL $6AD6       ; if non-negative, skip

6AD2    A2 00       LDX #$00        ; otherwise X = 0x00
6AD4    F0 FA       BEQ $6AD0       ; effectively a NOP (BRAs twice)

6AD6    8A          TXA             ; A = X
6AD7    4A          LSR A           ; A = A / 4
6AD8    4A          LSR A           ; .
6AD9    A8          TAY             ; Y = A
6ADA    B9 8A 6A    LDA $6A8A,Y     ; .
6ADD    CD 89 6A    CMP $6A89       ; if _6A89 == _6A8A[Y]
6AE0    F0 0C       BEQ $6AEE       ;   skip to $6AE
6AE2    A0 00       LDY #$00        ; otherwise
6AE4    84 81       STY $81         ;   _81 = 0
6AE6    A4 82       LDY $82         ;   _82 = 0
6AE8    D0 03       BNE $6AED       ; NOP?

6AEA    20 74 6A    JSR $6A74       ; ???

6AED    60          RTS             ; RETURN
```

```
6AEE    BD 90 6A    LDA $6A90,X     ; _81 = _6A90[X]
6AF1    85 81       STA $81         ; .

6AF3    60          RTS             ; RETURN
```

```
;; prompt for a character and return it in A

6AF4    AE 14 6B    LDX $6B14       ; X = (secondary) text column
6AF7    AC 15 6B    LDY $6B15       ; Y = (secondary) text row
6AFA    20 39 08    JSR $0839       ; set $CE/$CF, rotate cursor, print it, dec column
6AFD    20 25 6C    JSR $6C25       ; ??? animation ???
6B00    20 4E 08    JSR $084E       ; generate random number?
6B03    20 EE 6F    JSR $6FEE       ; fixed length timer
6B06    AD 00 C0    LDA $C000       ; load keypress
6B09    10 E9       BPL $6AF4       ; if no key press, go back to start of subroutine
6B0B    2C 10 C0    BIT $C010       ; clear keyboard
6B0E    48          PHA             ; preserve keypress on stack
6B0F    20 24 08    JSR $0824       ; display character (in-game graphics)
6B12    68          PLA             ; restore keypress from stack into A
6B13    60          RTS             ; return
```

```
;; used for temporarily preserving text column and row outside of zeropage
6B14    00
6B15    00
```

```
;; 'J' pressed (Journey onward)

6B16    A9 00       LDA #$00        ;
6B18    85 0F       STA $0F         ; _0F = 0x00
6B1A    85 B8       STA $B8         ; _B8 = 0x00
6B1C    A9 00       LDA #$00        ;
6B1E    20 74 6A    JSR $6A74       ; ???
6B21    20 1B 08    JSR $081B       ; PRINT (to run UALT4 file)

6B24    84 C2 D2 D5 CE A0 D5 81 CC D4 B4 AC C1 A4 B4 B0 B0 B0 8D 00             ^DBRUN U^ALT4,A$4000^M^@

;;

6B28    A9 00       LDA #$00        ;
6B2A    20 74 6A    JSR $6A74       ; ???
6B2D    A5 D1       LDA $D1         ; number of drives?
6B2F    C9 01       CMP #$01        ; if one drive
6B31    F0 09       BEQ $6B3C       ;   skip disk request

6B33    A9 02       LDA #$02        ; otherwise, request disk 2 (in drive 2)
6B35    20 1C 70    JSR $701C       ; .

;; 'I' pressed (Initiate new game)

6B38    A9 01       LDA #$01        ; set disk number loaded
6B3A    85 D0       STA $D0         ; to 1

6B3C    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page

6B3F    A2 04       LDX #$04        ; X = 0x04 (text column)
6B41    A0 10       LDY #$10        ; Y = 0x10 (text row)
6B43    20 1E 08    JSR $081E       ; PRINT

6B56    C2 F9 A0 F7 E8 E1 F4                                                    By what
6B5C    A0 EE E1 ED E5 A0 F3 E8 E1 EC F4 A0 F4 E8 EF F5                          name shalt thou
6B6C    A0 E2 E5 A0 EB EE EF F7 EE 00                                            be known^@

6B77    A2 04       LDX #$04        ; X = 0x04 (text column)
6B79    A0 11       LDY #$11        ; Y = 0x11 (text row)
6B7B    20 1E 08    JSR $081E       ; PRINT

6B7E    E9 EE A0 F4 E8 E9 F3 A0 F7 EF F2 EC E4 A0                               in this world
6B8C    E1 EE E4 A0 F4 E9 ED E5 BF A0 00                                        and time? ^@

6B97    A9 13       LDA #$13        ; text row = 0x13
6B99    85 CF       STA $CF         ; .
6B9B    A9 0C       LDA #$0C        ; text column = 0x0C
6B9D    85 CE       STA $CE         ; .
6B9F    20 51 6F    JSR $6F51       ; take input (in $300 buffer)
6BA2    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page

6BA5    A2 04       LDX #$04        ; X = 0x04 (text column)
6BA7    A0 11       LDY #$11        ; Y = 0x11 (text row)
6BA9    20 1E 08    JSR $081E       ; PRINT

6BAC    C1 F2 F4 A0 F4 E8 EF F5 A0 CD E1 EC E5 A0 EF F2                         Art thou Male or
6BBC    A0 C6 E5 ED E1 EC E5 BF A0 00                                           Female? ^@

6BC6    A5 CE       LDA $CE         ; store text column
6BC8    8D 14 6B    STA $6B14       ;   in _6B14
6BCB    A5 CF       LDA $CF         ; store text row
6BCD    8D 15 6B    STA $6B15       ;   in _6B15
6BD0    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
6BD3    C9 CD       CMP #$CD        ; if 'M'
6BD5    F0 08       BEQ $6BDF       ;   go to $6BDF

6BD7    C9 C6       CMP #$C6        ; if not 'F'
6BD9    D0 F5       BNE $6BD0       ;   go to back to prompt
6BDB    A9 7B       LDA #$7B        ; A = 0x7B
6BDD    D0 02       BNE $6BE1       ; BRAnch to $6BE1

6BDF    A9 5C       LDA #$5C        ; A = 0x5C

6BE1    85 BC       STA $BC         ; _BC = 0x5C for Male and 0x7B for Female
6BE3    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page
6BE6    A9 00       LDA #$00        ; A = 0x00
6BE8    20 74 6A    JSR $6A74       ; ??? something to do with SEL
6BEB    20 1B 08    JSR $081B       ; PRINT

6BEE    84 C2 D2 D5 CE A0 CE 81 C5 D7 C7 C1 CD C5 AC C1 A4 B6 B4 B0 B0 AC C4 B1 8D 00  ^DBRUN N^AEWGAME,A$6400,D1^M^@

6C08    60          RTS             ;
```

```
;;

6C09    20 18 6C    JSR $6C18       ;
6C0C    20 A5 6A    JSR $6AA5       ;
6C0F    AD 00 C0    LDA $C000       ; load a key
6C12    10 F5       BPL $6C09       ; if not key pressed, look back
6C14    2C 10 C0    BIT $C010       ; otherwise clear keyboard
6C17    60          RTS             ; and return with key in A
```

```
;;

6C18    20 06 6E    JSR $6E06       ; draw minimap on homescreen
6C1B    AD D5 6C    LDA $6CD5       ; .
6C1E    49 FF       EOR #$FF        ; .
6C20    8D D5 6C    STA $6CD5       ; . invert _6CD5 bit-wise
6C23    10 F3       BPL $6C18       ; on no high-bit, loop back

;; ??? animation ???

6C25    A9 00       LDA #$00        ; .
6C27    8D D6 6C    STA $6CD6       ; _6CD6 = 0x00
6C2A    EE DB 6C    INC $6CDB       ; _6CDB++
6C2D    AD DB 6C    LDA $6CDB       ; .
6C30    29 7F       AND #$7F        ; .
6C32    AA          TAX             ; .
6C33    BC DD 6C    LDY $6CDD,X     ; .
6C36    B9 95 6D    LDA $6D95,Y     ; .
6C39    8D D9 6C    STA $6CD9       ; _6CD9 = _6D95[_6CDD[_6CDB ^ 0111111]]
6C3C    B9 A7 6D    LDA $6DA7,Y     ; .
6C3F    8D DA 6C    STA $6CDA       ; _6CDA = _6DA7[_6CDD[_6CDB ^ 0111111]]

; loop 1: 6CD6 from 0x00 to 0x20

6C42    AE D6 6C    LDX $6CD6       ; .
6C45    BD 00 E0    LDA $E000,X     ; .
6C48    85 FE       STA $FE         ; _FE = _E000[_6CD6]

6C4A    BD C0 E0    LDA $E0C0,X     ; .
6C4D    85 FF       STA $FF         ; _FF = _E0C0[_6CD6]

6C4F    18          CLC             ; .
6C50    AD D9 6C    LDA $6CD9       ; .
6C53    6D D6 6C    ADC $6CD6       ; .
6C56    AA          TAX             ; .
6C57    BD C0 E0    LDA $E0C0,X     ; .
6C5A    18          CLC             ; .
6C5B    69 20       ADC #$20        ; .
6C5D    85 FD       STA $FD         ; _FD = _E0C0[_6CD9 + _6CD6] + 0x20

6C5F    BD 00 E0    LDA $E000,X     ; .
6C62    18          CLC             ; .
6C63    6D DA 6C    ADC $6CDA       ; .
6C66    85 FC       STA $FC         ; _FC = _E000[_6CD9 + _6CD6] + _6CDA

6C68    A0 06       LDY #$06        ; Y = 0x06

; loop 2: Y from 0x06 to 0x00 inclusive

6C6A    B1 FC       LDA ($FC),Y     ; copy byte from address at $FC/$FD + Y
6C6C    91 FE       STA ($FE),Y     ;   to address at $FE/$FF + Y
6C6E    88          DEY             ;
6C6F    10 F9       BPL $6C6A       ;

; end loop 2

6C71    EE D6 6C    INC $6CD6       ; _6CD6++
6C74    AD D6 6C    LDA $6CD6       ; A = _6CD6
6C77    C9 20       CMP #$20        ;
6C79    90 C7       BCC $6C42       ;

; end loop 1

6C7B    A9 00       LDA #$00        ; .
6C7D    8D D6 6C    STA $6CD6       ; _6CD6 = 0x00
6C80    EE DC 6C    INC $6CDC       ; _6CDC++
6C83    AD DC 6C    LDA $6CDC       ; .
6C86    29 3F       AND #$3F        ; .
6C88    AA          TAX             ; .
6C89    BC 55 6D    LDY $6D55,X     ; Y = _6D55[_6CDC + 0x3F]
6C8C    B9 B9 6D    LDA $6DB9,Y     ;
6C8F    8D D9 6C    STA $6CD9       ;
6C92    B9 CB 6D    LDA $6DCB,Y     ;
6C95    8D DA 6C    STA $6CDA       ;

; loop

6C98    AE D6 6C    LDX $6CD6       ;
6C9B    BD 00 E0    LDA $E000,X     ;
6C9E    18          CLC             ;
6C9F    69 22       ADC #$22        ;
6CA1    85 FE       STA $FE         ;
6CA3    BD C0 E0    LDA $E0C0,X     ;
6CA6    85 FF       STA $FF         ;
6CA8    18          CLC             ;
6CA9    AD D9 6C    LDA $6CD9       ;
6CAC    6D D6 6C    ADC $6CD6       ;
6CAF    AA          TAX             ;
6CB0    BD C0 E0    LDA $E0C0,X     ;
6CB3    18          CLC             ;
6CB4    69 20       ADC #$20        ;
6CB6    85 FD       STA $FD         ;
6CB8    BD 00 E0    LDA $E000,X     ;
6CBB    18          CLC             ;
6CBC    6D DA 6C    ADC $6CDA       ;
6CBF    85 FC       STA $FC         ;
6CC1    A0 05       LDY #$05        ;

6CC3    B1 FC       LDA ($FC),Y     ;
6CC5    91 FE       STA ($FE),Y     ;
6CC7    88          DEY             ;
6CC8    10 F9       BPL $6CC3       ;

6CCA    EE D6 6C    INC $6CD6       ;
6CCD    AD D6 6C    LDA $6CD6       ;
6CD0    C9 20       CMP #$20        ;
6CD2    90 C4       BCC $6C98       ;

6CD4    60          RTS             ;
```

```
;; DATA

6CD5    00
6CD6    00
6CD7    FF FF

;; the value in _6D95[] is stored in _6CD9
;; the value in _6DA7[] is stored in _6CDA
6CD9    00 00

6CDB    FF
6CDC    FF

;; some sort of 128-byte array
; min = 00; max = 11
; then used as offset into _6D95 or _6DA7

6CDD    01 01 01 00 00 01 01 01 00 00 01 01 02 02 03 03
6CED    04 04 01 02 03 04 01 02 05 06 07 08 05 06 07 08
6CFD    05 06 07 08 05 06 07 08 05 06 07 08 05 06 07 08
6D0D    09 0A 09 0A 09 0A 0B 0B 0B 0B 0C 0C 0D 0D 0C 0D
6D1D    0C 0D 0C 0B 0B 0B 00 00 01 02 03 04 01 02 05 06
6D2D    07 08 05 06 07 08 09 0A 0B 0B 0B 00 00 0E 0E 0E
6D3D    0F 10 10 10 11 11 11 10 10 10 11 11 11 10 10 10
6D4D    0F 0E 0E 00 00 0B 0B 0B 01 00 01 02 03 04 03 02

6D5D    01 00 01 02 03 04 05 06 05 06 05 06 04 07 08
6D6C    09 0A 09 08 07 08 09 0A 0B 0C 0B 0C 0D 0B 0C 0D
6D7C    01 0D 01 0E 01 0F 01 0E 01 0F 0A 09 08 10 11 10
6D8C    11 10 11 09 08 07 04 03 02

;; another array; the values in _6CDD are offsets into this

6D95    00 20 40 60 80 A0 00 20 40 60 80 A0 00 20 40 60 80 A0

;; another array; the values in _6CDD are also offsets into this

6DA7    00 00 00 00 00 00 07 07 07 07 07 07 0E 0E 0E 0E 0E 0E

; so effectively the table is:
; 00 = 0000
; 01 = 0020
; 02 = 0040
; 03 = 0060
; 04 = 0080
; 05 = 00A0
; 06 = 0700
; 07 = 0720
; 08 = 0740
; 09 = 0760
; 0A = 0780
; 0B = 07A0
; 0C = 0E00
; 0D = 0E20
; 0E = 0E40
; 0F = 0E60
; 10 = 0E80
; 11 = 0EA0

;;

6DB9    00 20 40
6DBC    60 80 A0 00 20 40 60 80 A0 00 20 40 60 80 A0 22
6DCC    22 22 22 22 22 1C 1C 1C 1C 1C 1C 16 16 16 16 16
6DDC    16
```

```
;; set up initial 19 x 5 default map in $0200 then override some cells based on $EEXX

; set up initial $0200 - $025E based on $6E73

6DDD    A2 5E       LDX #$5E        ; 19 x 5 - 1

6DDF    BD 73 6E    LDA $6E73,X     ;
6DE2    9D 00 02    STA $0200,X     ;
6DE5    CA          DEX             ;
6DE6    10 F7       BPL $6DDF       ;

; loop from X = 0x00 to 0x1F

6DE8    A2 00       LDX #$00        ;

6DEA    BD 00 EE    LDA $EE00,X     ; .
6DED    F0 11       BEQ $6E00       ; if _EE00[X] is zero, skip, otherwise...
6DEF    BC 40 EE    LDY $EE40,X     ; .
6DF2    B9 6E 6E    LDA $6E6E,Y     ; .
6DF5    18          CLC             ; .
6DF6    7D 20 EE    ADC $EE20,X     ; .
6DF9    A8          TAY             ; .
6DFA    BD 00 EE    LDA $EE00,X     ; .
6DFD    99 00 02    STA $0200,Y     ; _0200[_6E6E[_EE40[X]] + _EE20[X]] = _EE00[X]

6E00    E8          INX             ; X++
6E01    E0 20       CPX #$20        ; .
6E03    D0 E5       BNE $6DEA       ; if X != 0x20 loop back

6E05    60          RTS             ;
```

```
;; draw minimap on homescreen

6E06    20 D2 6E    JSR $6ED2       ; set up some stuff in $EEXX based on $7170 ($10,$11,$12 set up already)
6E09    20 6C 08    JSR $086C       ;
6E0C    20 DD 6D    JSR $6DDD       ; set up initial 19 x 5 default map in $0200 then override some cells based on $EEXX

6E0F    A9 00       LDA #$00        ;
6E11    85 F2       STA $F2         ; _F2 = 0x00
6E13    8D 38 6E    STA $6E38       ; _6E38 = 0x00; _6E38 is offset into $02XX (which tile to use in which cell)

; loop over _F2

6E16    A4 F2       LDY $F2         ; $F2 is row number?
6E18    B9 68 E0    LDA $E068,Y     ; look up base address (LSB) for row Y/_F2
6E1B    8D 41 6E    STA $6E41       ; low byte base address of video ram to write to (left-half of shape)
6E1E    8D 4B 6E    STA $6E4B       ; low byte base address of video ram to write to (right-half of shape)
6E21    B9 28 E1    LDA $E128,Y     ; look up base address (MSB) for row Y/_F2
6E24    8D 42 6E    STA $6E42       ; high byte base address of video ram to write to (left-half of shape)
6E27    8D 4C 6E    STA $6E4C       ; high byte base address of video ram to write to (right-half of shape)
6E2A    98          TYA             ; A = Y
6E2B    29 0F       AND #$0F        ; A ^= 00001111
6E2D    09 D0       ORA #$D0        ; A |= 11010000

; so shape base is D_00 where _ is the low nibble of the row number _F2

6E2F    8D 3F 6E    STA $6E3F       ; half-shape page
6E32    8D 49 6E    STA $6E49       ; half-shape page

6E35    A2 01       LDX #$01        ; X = 0x01

; loop from X = 0x01 to 0x27 exclusive (X = byte column on screen)
; this writes one pixel row of map

6E37    AC 00 02    LDY $02__       ; Y = tile to use for next cell
6E3A    2C 8B C0    BIT $C08B       ; bank switch
6E3D    B9 00 FF    LDA __00,Y      ; get half-shape
6E40    9D FF FF    STA $____,X     ; and put it on screen

6E43    E8          INX             ; X++
6E44    2C 83 C0    BIT $C083       ; bank switch
6E47    B9 00 FF    LDA $__00,Y     ; get other half-shape
6E4A    9D FF FF    STA $____,X     ; and put it on screen

6E4D    EE 38 6E    INC $6E38       ; increment the cell offset into $02XX

6E50    E8          INX             ; X++
6E51    E0 27       CPX #$27        ; .
6E53    90 E2       BCC $6E37       ; if X < 0x27, loop back to $6E37

; _F2 is the pixel row number so the high nibble is the cell row and the low nibble is the pixel row within that cell

6E55    E6 F2       INC $F2         ; _F2++
6E57    A5 F2       LDA $F2         ; .
6E59    C9 50       CMP #$50        ; .
6E5B    F0 10       BEQ $6E6D       ; if _F2 == 0x50, return
6E5D    29 0F       AND #$0F        ; otherwise, A ^= 00001111
6E5F    F0 B5       BEQ $6E16       ; if _F2 had zero low nibble, loop back to $6E16

; 0x13 is the "width" of the map so subtracting 0x13 goes to the "previous" cell row

6E61    AD 38 6E    LDA $6E38       ; the cell offset into $02XX
6E64    38          SEC             ;
6E65    E9 13       SBC #$13        ; subtract 0x13 from it
6E67    8D 38 6E    STA $6E38       ; and store it

6E6A    4C 16 6E    JMP $6E16       ; loop back to loop over _F2
```

```
6E6D    60          RTS             ;
```

```
;;

6E6E    00 13 26 39 4C

;; DATA (default map on home screen)

6E73    06 06 06 04 04 04 01 01 00 00 00 00 01 04 04 0D 0E 0F 04
6E86    06 06 04 04 04 01 01 01 01 00 00 01 01 04 04 04 04 04 04
6E99    06 04 04 01 01 01 02 02 01 01 01 01 01 01 0A 04 04 04 06
6EAC    06 04 04 01 01 02 02 01 01 09 08 01 01 01 01 04 06 06 06
6EBF    04 04 04 04 01 01 01 01 04 04 08 08 01 01 01 01 01 06 06
```

```
;;
; reading and manipulating $12
; reading and manipulating $10,$11 (used as start of array, starting at $7170)
; arrays in $EE00, $EE20, $EE40, $EE60
; data array in $6F42

6ED2    C6 12       DEC $12         ; _12--
6ED4    10 19       BPL $6EEF       ; if non-negative, return

6ED6    A0 00       LDY #$00        ; Y = 0x00

;

6ED8    B1 10       LDA ($10),Y     ;
6EDA    10 1E       BPL $6EFA       ; if _10[Y] non-negative, branch to $6EFA
6EDC    AA          TAX             ; preserve A in X
6EDD    29 F0       AND #$F0        ; A ^= 11110000 (high nibble)
6EDF    C9 80       CMP #$80        ;
6EE1    D0 0D       BNE $6EF0       ; if A != 0x80, reset _10/_11 to 0x70, 0x71

6EE3    8A          TXA             ; restore A from X
6EE4    29 0F       AND #$0F        ; A ^= 00001111 (low nibble)
6EE6    0A          ASL A           ; double it
6EE7    85 12       STA $12         ; _12 = A

; increment _10/_11 with wrap
6EE9    E6 10       INC $10         ; _10++
6EEB    D0 02       BNE $6EEF       ; if not zero, return
6EED    E6 11       INC $11         ; if zero (wrapped) _11++

6EEF    60          RTS             ; return
```

```
;;

6EF0    A9 70       LDA #$70        ; .
6EF2    85 10       STA $10         ; _10 = 0x70
6EF4    A9 71       LDA #$71        ; .
6EF6    85 11       STA $11         ; _11 = 0x71
6EF8    D0 DE       BNE $6ED8       ; go back

6EFA    48          PHA             ; .
6EFB    29 0F       AND #$0F        ; .
6EFD    AA          TAX             ; X = low nibble of A
6EFE    68          PLA             ; .
6EFF    29 F0       AND #$F0        ; A = high nibble of A
6F01    4A          LSR A           ; shifted down to low nibble
6F02    4A          LSR A           ; .
6F03    4A          LSR A           ; .
6F04    4A          LSR A           ; .
6F05    C9 07       CMP #$07        ; .
6F07    F0 2B       BEQ $6F31       ; if A == 0x7, go to $6F31
6F09    9D 40 EE    STA $EE40,X     ;
6F0C    20 E9 6E    JSR $6EE9       ; increment _10/_11 with wrap

6F0F    B1 10       LDA ($10),Y     ; A = _10[Y]
6F11    48          PHA             ; .
6F12    29 1F       AND #$1F        ; .
6F14    9D 20 EE    STA $EE20,X     ; _EE20[X] = A ^ 0x1F
6F17    68          PLA             ; .
6F18    4A          LSR A           ; shift A right 5 times (/32)
6F19    4A          LSR A           ; .
6F1A    4A          LSR A           ; .
6F1B    4A          LSR A           ; .
6F1C    4A          LSR A           ; .
6F1D    18          CLC             ; .
6F1E    7D 42 6F    ADC $6F42,X     ; .
6F21    9D 00 EE    STA $EE00,X     ; _EE00[X] = A + _6F42[X]
6F24    E0 01       CPX #$01        ; .
6F26    D0 03       BNE $6F2B       ; .

6F28    BD 42 6F    LDA $6F42,X     ; if X == 1, A = _6F42[X]

6F2B    9D 60 EE    STA $EE60,X     ; _EE60[X] = A
6F2E    20 E9 6E    JSR $6EE9       ; increment _10/_11 with wrap
6F31    4C D8 6E    JMP $6ED8       ;
```

```
6F34    A9 00       LDA #$00        ; .
6F36    9D 00 EE    STA $EE00,X     ; _EE00[X] = 0x00
6F39    9D 60 EE    STA $EE60,X     ; _EE60[X] = 0x00
6F3C    20 E9 6E    JSR $6EE9       ; increment _10/_11 with wrap
6F3F    4C D8 6E    JMP $6ED8       ;
```

```
;; DATA

6F42    40 80 10 38 38 C8 C8 24 20 88 F0 F8 4D 4F 4E

;; take input

6F51    A9 00       LDA #$00        ; .
6F53    85 EA       STA $EA         ; _EA = 0x00

6F55    A6 CE       LDX $CE         ; X = $CE (text column)
6F57    A4 CF       LDY $CF         ; Y = $CF (text row)
6F59    20 39 08    JSR $0839       ; set $CE/$CF, rotate cursor, print it, dec column
6F5C    20 25 6C    JSR $6C25       ; ??? animation ???
6F5F    A9 00       LDA #$00        ;
6F61    85 13       STA $13         ;

6F63    E6 13       INC $13         ;
6F65    30 EE       BMI $6F55       ; if enough time has passed, go back and rotate cursor and animate

6F67    A0 00       LDY #$00        ; wait a short time
6F69    88          DEY             ; .
6F6A    D0 FD       BNE $6F69       ; .
6F6C    AD 00 C0    LDA $C000       ; load keypress
6F6F    10 F2       BPL $6F63       ; if no key, go back to loop and increment $13

6F71    2C 10 C0    BIT $C010       ; clear keyboard
6F74    C9 FF       CMP #$FF        ; compare to 0xFF (del?)
6F76    D0 02       BNE $6F7A       ; if not, skip

6F78    A9 88       LDA #$88        ; but if del, treat as ^H

6F7A    C9 8D       CMP #$8D        ; if ^M
6F7C    F0 32       BEQ $6FB0       ;   go to $6FB0
6F7E    C9 88       CMP #$88        ; if ^H
6F80    F0 15       BEQ $6F97       ;   go to $6F97
6F82    C9 A0       CMP #$A0        ; if < 0xA0
6F84    90 DD       BCC $6F63       ;   ignore and go back to $6F63
6F86    A6 EA       LDX $EA         ; X = _EA (initially 0x00)
6F88    E0 0F       CPX #$0F        ; if X == 0xF
6F8A    F0 D7       BEQ $6F63       ;   ignore and go back to $6F63
6F8C    9D 00 03    STA $0300,X     ; store in the input buffer
6F8F    20 24 08    JSR $0824       ; display character (in-game graphics)
6F92    E6 EA       INC $EA         ; increment character buffer pointer
6F94    4C 63 6F    JMP $6F63       ; back for more characters
```

```
; ^H
6F97    A5 EA       LDA $EA         ; load the character buffer pointer
6F99    F0 C8       BEQ $6F63       ; if zero, ignore and go back to $6F63
6F9B    C6 EA       DEC $EA         ; otherwise decrement
6F9D    A9 A0       LDA #$A0        ; display space
6F9F    20 24 08    JSR $0824       ; .
6FA2    C6 CE       DEC $CE         ; move cursor two left
6FA4    C6 CE       DEC $CE         ; .
6FA6    A9 A0       LDA #$A0        ; display space
6FA8    20 24 08    JSR $0824       ; .
6FAB    C6 CE       DEC $CE         ; move cursor one left
6FAD    4C 63 6F    JMP $6F63       ; back for more characters
```

```
; ^M
6FB0    A6 EA       LDX $EA         ; X = character buffer pointer
6FB2    F0 AF       BEQ $6F63       ; if zero, back for more characters
6FB4    A9 00       LDA #$00        ; otherwise, store null
6FB6    9D 00 03    STA $0300,X     ; at end of character buffer
6FB9    E8          INX             ; X++
6FBA    E0 10       CPX #$10        ; if X < 0x10
6FBC    90 F8       BCC $6FB6       ; go back and null out more
6FBE    A9 8D       LDA #$8D        ; print a new line
6FC0    20 24 08    JSR $0824       ; .

6FC3    60          RTS             ; RETURN
```

```
;; clear lower-half window on home page

6FC4    A2 4F       LDX #$4F        ; X = 0x4F

; loop 1: loop from X = 0x4F to 0x00 inclusive

6FC6    BD 68 E0    LDA $E068,X     ; _FE = E068[0x4F]   initially 0x50
6FC9    85 FE       STA $FE         ; .
6FCB    BD 28 E1    LDA $E128,X     ; _FF = E128[0x4F]   initially 0x3F
6FCE    85 FF       STA $FF         ; .

6FD0    A0 26       LDY #$26        ; Y = 0x26
6FD2    A9 00       LDA #$00        ; A = 0x00

; loop 2: loop from Y = 0x26 to 0x00 (exclusive)

6FD4    91 FE       STA ($FE),Y     ; (_FE)[Y] = 0x00    initially $3F76 (on hires screen)
6FD6    88          DEY             ; Y--
6FD7    D0 FB       BNE $6FD4       ; repeat

6FD9    CA          DEX             ; X--
6FDA    10 EA       BPL $6FC6       ;

6FDC    60          RTS             ;
```

```
;; 'N' pressed (Number of drives-1)

;; 01 EOR 03 == 10
;; 10 EOR 03 == 01

6FDD    A5 D1       LDA $D1         ; toggle _D1
6FDF    49 03       EOR #$03        ; between 1 and 2
6FE1    85 D1       STA $D1         ; .
6FE3    AD E1 67    LDA $67E1       ; toggle _67E1
6FE6    49 03       EOR #$03        ; between 1 and 2
6FE8    8D E1 67    STA $67E1       ; .
6FEB    4C 69 67    JMP $6769       ; jump to MAIN MENU
```

```
;; timer lasting a certain fixed amount

6FEE    A2 96       LDX #$96        ;
6FF0    A0 00       LDY #$00        ;
6FF2    88          DEY             ;
6FF3    D0 FD       BNE $6FF2       ;
6FF5    CA          DEX             ;
6FF6    D0 FA       BNE $6FF2       ;
6FF8    60          RTS             ;
```

```
6FF9    A2 BF       LDX #$BF        ;
6FFB    BD 00 E0    LDA $E000,X     ;
6FFE    85 FE       STA $FE         ;
7000    BD C0 E0    LDA $E0C0,X     ;
7003    85 FF       STA $FF         ;
7005    A9 00       LDA #$00        ;
7007    A0 27       LDY #$27        ;
7009    91 FE       STA ($FE),Y     ;
700B    88          DEY             ;
700C    10 FB       BPL ---         ;
700E    CA          DEX             ;
700F    E0 90       CPX #$90        ;
7011    B0 E8       BCS ---         ;
7013    A9 00       LDA #$00        ;
7015    85 CE       STA $CE         ;
7017    A9 12       LDA #$12        ;
7019    85 CF       STA $CF         ;
701B    60          RTS             ;
```

```
;; request disk switch
; A = desired disk number
; 1 = ULTIMA
; 2 = BRITANNIA
; 3 = TOWN
; 4 = DUNGEON

701C    85 DE       STA $DE         ; store A in $DE (desired disk number)

701E    20 C4 6F    JSR $6FC4       ; clear lower-half window on home page
7021    A9 10       LDA #$10        ;
7023    85 CF       STA $CF         ; _CF = 0x10 (text row)
7025    A5 DE       LDA $DE         ; A = $DE (desired disk number)
7027    C9 02       CMP #$02        ; if 2 (BRITANNIA)
7029    F0 6C       BEQ $7097       ;   go to $7097 (request insert in drive 2)
702B    C9 04       CMP #$04        ; if 4 (DUNGEON)
702D    F0 68       BEQ $7097       ;   go to $7097 (request insert in drive 2)
702F    A9 01       LDA #$01        ;
7031    85 DF       STA $DF         ; _DF = 0x01
7033    A5 DE       LDA $DE         ;
7035    C5 D2       CMP $D2         ; if _D2 == _DE (if disk in drive 1 is the desired one)
7037    F0 5B       BEQ $7094       ;   go to $7094
7039    A4 CF       LDY $CF         ; Y = _CF (text row)
703B    A2 0B       LDX #$0B        ; X = 0x0B
703D    20 1E 08    JSR $081E       ; PRINT

7040    D0 CC C5 C1 D3 C5 A0 D0 CC C1 C3 C5 A0 D4 C8 C5 00                      PLEASE PLACE THE^@

7051    E6 CF       INC $CF         ; increment text row
7053    20 08 71    JSR $7108       ; display name of desired disk (in $DE)
7056    A4 CF       LDY $CF         ; Y = $CF
7058    A2 0D       LDX #$0D        ; X = 0xD
705A    20 1E 08    JSR $081E       ; PRINT

705D    C9 CE D4 CF A0 C4 D2 C9 D6 C5 A0 B1 00                                  INTO DRIVE 1^@

706A    E6 CF       INC $CF         ; increment text row
706C    A4 CF       LDY $CF         ; Y = $CF
706E    A2 0B       LDX #$0B        ; X = 0xB
7070    20 1E 08    JSR $081E       ; PRINT

7073    C1 CE C4 A0 D0 D2 C5 D3 D3 A0 DB C5 D3 C3 DD 00                         AND PRESS [ESC]^@

7083    A5 CE       LDA $CE         ; store text column
7085    8D 14 6B    STA $6B14       ;   in _6B14
7088    A5 CF       LDA $CF         ; store text row
708A    8D 15 6B    STA $6B15       ;   in _6B15
708D    20 F4 6A    JSR $6AF4       ; prompt for a character and return it in A
7090    C9 9B       CMP #$9B        ; if not ESC
7092    D0 F9       BNE $708D       ;   go back and prompt again
7094    4C DB 70    JMP $70DB       ; but if ESC go to $70DB
```

```
;;

7097    A5 D1       LDA $D1         ;
7099    C9 02       CMP #$02        ;
709B    90 92       BCC ---         ;
709D    A9 02       LDA #$02        ;
709F    85 DF       STA $DF         ;
70A1    A5 DE       LDA $DE         ;
70A3    C5 D3       CMP $D3         ;
70A5    F0 34       BEQ ---         ;
70A7    A4 CF       LDY $CF         ;
70A9    A2 0B       LDX #$0B        ;
70AB    20 1E 08    JSR $081E       ;

70AE    D0 CC C5 C1 D3 C5 A0 D0 CC C1 C3 C5 A0 D4 C8 C5 00                      PLEASE PLACE THE^@

70BF    E6 CF       INC $CF         ; increment text row
70C1    20 08 71    JSR $7108       ; display name of desired disk (in $DE)
70C4    A4 CF       LDY $CF         ; Y = $CF (text row)
70C6    A2 0D       LDX #$0D        ; X = 0x0D
70C8    20 1E 08    JSR $081E       ; PRINT

70CB    C9 CE D4 CF A0 C4 D2 C9 D6 C5 A0 B2 00                                  INTO DRIVE 2^@

70D8    4C 6A 70    JMP $706A       ; back to drive 1 case
```

```
;;

70DB    A5 DF       LDA $DF         ; A = _DF (drive number)
70DD    18          CLC             ;
70DE    69 B0       ADC #$B0        ; added B0 to it to convert to character code
70E0    8D F3 70    STA $70F3       ; change drive number in DOS command
70E3    20 1B 08    JSR $081B       ; PRINT

; $70F3 is the 1
70E6    84 C2 CC CF C1 C4 A0 C4 C9 D3 CB AC C4 B1 8D 00                         ^DBLOAD DISK,D1^M^@

; the disk number gets loaded into _D0

70F6    A6 DF       LDX $DF         ; X = _DF (drive number)
70F8    A5 D0       LDA $D0         ; A = _D0 (disk number as declared in DISK file)
70FA    95 D1       STA $D1,X       ; put the disk number in _D2 (if drive 1) or _D3 (if drive 2)
70FC    C5 DE       CMP $DE         ; if loaded disk == _DE (desired disk)
70FE    F0 07       BEQ $7107       ; return

7100    C6 CF       DEC $CF         ; decrement row by two
7102    C6 CF       DEC $CF         ; .

7104    4C 1E 70    JMP $701E       ; go back and request disk switch again
```

```
7107    60          RTS             ;
```

```
;;
; display the name of the desired disk (stored in $DE)
; 1 = ULTIMA
; 2 = BRITANNIA
; 3 = TOWN
; 4 = DUNGEON

7108    A6 DE       LDX $DE         ; load desired disk number
710A    CA          DEX             ; X--
710B    D0 16       BNE $7123       ; if not zero, skip to next section, otherwise

710D    A4 CF       LDY $CF         ; Y = $CF (text row)
710F    A2 0D       LDX #$0D        ; X = 0x0D (text column)
7111    20 1E 08    JSR $081E       ; PRINT

7114    D5 CC D4 C9 CD C1 A0 C4 C9 D3 CB 00                                     ULTIMA DISK^@

7120    E6 CF       INC $CF         ; increment text row
7122    60          RTS             ; return
```

```
7123    CA          DEX             ; X--
7124    D0 19       BNE $713F       ; if not zero, skip to next section, otherwise

7126    A4 CF       LDY $CF         ; Y = $CF (text row)
7128    A2 0C       LDX #$0C        ; X = 0x0C (text column)
712A    20 1E 08    JSR $081E       ; PRINT

712D    C2 D2 C9 D4 C1 CE CE C9 C1 A0 C4 C9 D3 CB 00                            BRITANNIA DISK^@

713C    E6 CF       INC $CF         ; increment text row
713E    60          RTS             ; return
```

```
713F    CA          DEX             ; X--
7140    D0 14       BNE $7156       ; if not zero, skip to next section, otherwise

7142    A4 CF       LDY $CF         ; Y = $CF (text row)
7144    A2 0E       LDX #$0E        ; X = 0x0E (text column)
7146    20 1E 08    JSR $081E       ; PRINT

7149    D4 CF D7 CE A0 C4 C9 D3 CB 00                                           TOWN DISK^@

7153    E6 CF       INC $CF         ; increment text row
7155    60          RTS             ; return
```

```
7156    CA          DEX             ; X--
7157    D0 16       BNE $716F       ; if not zero, skip and return, otherwise

7159    A4 CF       LDY $CF         ; Y = $CF (text row)
715B    A2 0D       LDX #$0D        ; X = 0x0D (text column)
715D    20 1E 08    JSR $081E       ; PRINT

7160    C4 D5 CE C7 C5 CF CE A0 C4 C9 D3 CB 00                                  DUNGEON DISK^@

716D    E6 CF       INC $CF         ; increment text row
716F    60          RTS             ;
```

```
;; DATA

7170    88 20 11 80 20 31 80 20 51 80 20 71
717C    88 27 11 84 27 10 84 17 10 84 07 10 84 77 88 20
718C    51 80 20 31 80 20 11 80 70 88 01 66 84 11 66 84
719C    07 10 84 17 10 82 11 46 82 17 0F 82 11 47 82 17
71AC    0E 82 11 48 27 0E 84 11 49 77 84 11 4A 88 27 0E
71BC    84 17 0E 84 17 0D 84 28 0E 84 18 0E 84 11 2A 80
71CC    1C 0B 80 1C 0C 80 7C 1D 0D 81 7D 84 1E 0D 80 1E
71DC    0C 80 1E 0B 80 1E 0A 81 7E 84 11 4A 84 11 4B 07
71EC    0D 84 11 4C 07 0E 84 15 0C 84 15 0D 84 05 0D 16
71FC    0C 84 16 0D 84 0D 0E 81 7D 84 1D 0E 81 7D 84 0D
720C    0D 81 7D 84 1C 0D 81 7C 84 0D 0E 81 7D 84 1D 0E
721C    81 7D 84 0D 0D 81 7D 75 03 0D 84 1D 0D 81 7D 84
722C    1C 0E 81 7C 84 1D 0D 81 7D 76 14 0D 88 18 0D 84
723C    18 0C 17 0E 84 78 17 0D 84 17 0C 84 77 88 71 12
724C    4C 84 12 6C 84 12 0C 84 4B 00 73 82 3B 01 82 2B
725C    02 12 0B 82 2B 03 82 12 0A 1B 04 74 82 1B 05 82
726C    1B 06 12 6A 82 1B 07 82 1C 09 80 1C 08 80 7C 1D
727C    07 81 7D 88 1B 08 82 1C 09 80 7C 1D 08 81 7D 7B
728C    84 3A 09 84 4A 09 84 4A 08 12 0A 84 12 09 84 12
729C    08 84 2C 08 80 3C 08 80 7C 4D 08 81 7D 7A 88 12
72AC    68 84 22 68 84 32 68 84 32 08 84 32 07 84 32 67
72BC    84 42 67 88 47 07 84 47 08 84 47 09 48 07 84 37
72CC    09 48 08 84 77 48 09 84 38 09 84 78 88 37 09 84
72DC    47 09 84 47 08 38 09 84 47 07 48 09 84 77 48 08
72EC    84 48 07 84 78 88 42 07 84 42 06 84 42 05 84 42
72FC    04 84 42 24 84 32 24 84 32 04 84 32 03 88 37 03
730C    84 37 02 84 37 01 38 03 84 47 01 38 02 82 09 09
731C    82 47 00 48 02 19 08 82 29 07 82 47 01 39 06 82
732C    3E 05 80 3E 04 80 3E 03 81 7E 72 82 39 05 82 39
733C    04 48 03 37 01 82 39 03 82 3E 03 37 02 81 7E 83
734C    4D 03 81 7D 78 44 03 84 3D 03 81 7D 82 3D 02 81
735C    7D 82 3D 03 81 7D 79 88 47 02 84 47 03 74 88 20
736C    01 80 20 21 80 20 41 80 20 61 85 47 02 84 37 02
737C    84 27 02 84 27 01 84 77 88 20 61 80 20 41 80 20
738C    21 80 20 01 80 70 88 FF 00 00 00 00 00 00 00 00
```