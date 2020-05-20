---
grand_parent: DISK 1
parent: SUBS
nav_order: 1
---

# SUBS full listing

```
ORG $0800
LEN $1800

0800  4C B5 0A      JMP $0AB5
0803  4C 08 0A      JMP $0A08
0806  4C 99 08      JMP $0899
0809  4C BD 08      JMP $08BD
080C  4C E0 08      JMP $08E0
080F  4C 04 09      JMP $0904
0812  4C 79 16      JMP $1679     ; set the hires page one to all 0x80
0815  4C 74 0E      JMP $0E74     ; draw map
0818  4C 7A 0C      JMP $0C7A
081B  4C D2 0F      JMP $0FD2     ; some sort of PRINT
081E  4C 81 0F      JMP $0F81     ; set text col, row to X, Y then ...
0821  4C 85 0F      JMP $0F85     ; display null-terminated string in game font
0824  4C A9 0F      JMP $0FA9     ; display character in game font
0827  4C 19 13      JMP $1319     ; set the hires page one to all 0x80
082A  4C 69 13      JMP $1369
082D  4C 02 10      JMP $1002     ; set $FE/$FF vector based on $D4 (character number)
0830  4C 31 10      JMP $1031
0833  4C 7C 10      JMP $107C     ; output a byte as two digits
0836  4C 67 10      JMP $1067     ; rotate cursor, print it, and decrement text column
0839  4C 63 10      JMP $1063     ; set text col, row to X, Y then do cursor as above
083C  4C 18 16      JMP $1618
083F  4C 20 16      JMP $1620     ; store $CE/$CF in $1677/$1678 and $00 in $1676
0842  4C 68 18      JMP $1868     ; request disk switch
0845  4C C3 10      JMP $10C3     ; DISPLAY STATS
0848  4C E3 12      JMP $12E3
084B  4C DC 0A      JMP $0ADC
084E  4C 4A 15      JMP $154A     ; random number generator?
0851  4C 7F 0A      JMP $0A7F
0854  4C 39 19      JMP $1939     ; play sound A for duration X ???
0857  4C 52 0B      JMP $0B52
085A  4C 7B 12      JMP $127B
085D  4C CA 12      JMP $12CA
0860  4C 8F 11      JMP $118F
0863  4C 49 0B      JMP $0B49
0866  4C 8B 10      JMP $108B     ; display single digit
0869  4C 91 10      JMP $1091
086C  4C 0F 0F      JMP $0F0F
086F  4C 11 10      JMP $1011     ; print player name?
0872  4C E2 11      JMP $11E2     ; in dungeon, display direction and level
0875  4C 31 13      JMP $1331     ; clear screen?
0878  4C 4C 13      JMP $134C     ; invert the hires screen?
087B  4C 99 1A      JMP $1A99
087E  4C D3 1A      JMP $1AD3
0881  4C 08 1F      JMP $1F08
0884  4C 2E 1F      JMP $1F2E
0887  4C 5C 1F      JMP $1F5C
088A  4C 80 1F      JMP $1F80
088D  4C 8D 1F      JMP $1F8D
0890  4C 94 1F      JMP $1F94
0893  4C B7 1F      JMP $1FB7
0896  4C D6 1F      JMP $1FD6

;; ... if west wind

0899    E6 00       INC $00         ;
089B    E6 02       INC $02         ;
089D    A5 02       LDA $02         ;
089F    C9 18       CMP #$18        ; if _02 < #$18
08A1    90 03       BCC $08A6       ; skip

08A3    20 23 03    JSR $0323       ; turns disk drive motor on for a period of time?

08A6    C9 1B       CMP #$1B        ; if _02 < #$1B
08A8    90 12       BCC $08BC       ; return

08AA    29 0F       AND #$0F        ;
08AC    85 02       STA $02         ; _02 = _02 % 16
08AE    20 28 09    JSR $0928       ; copy $E9XX to $E8XX and $EBXX to $EAXX
08B1    E6 04       INC $04         ;
08B3    A5 04       LDA $04         ;
08B5    29 0F       AND #$0F        ;
08B7    85 04       STA $04         ; _04 = (_04 + 1) % 16
08B9    20 70 09    JSR $0970       ; load a track/sector based on $04/$05 into $E9XX and $EBXX

08BC    60          RTS             ;

;; ... if east wind

08BD    C6 00       DEC $00         ;
08BF    C6 02       DEC $02         ;
08C1    A5 02       LDA $02         ;
08C3    C9 08       CMP #$08        ; if _02 >= #$08
08C5    B0 03       BCS $08CA       ; skip

08C7    20 23 03    JSR $0323       ; turns disk drive motor on for a period of time?

08CA    C9 05       CMP #$05        ; if _02 >= #$05
08CC    B0 EE       BCS $08BC       ; return
08CE    09 10       ORA #$10        ;
08D0    85 02       STA $02         ; _02 = 16 + _02
08D2    20 3A 09    JSR $093A       ; copy $E8XX to $E9XX and $EAXX to $EBXX
08D5    C6 04       DEC $04         ;
08D7    A5 04       LDA $04         ;
08D9    29 0F       AND #$0F        ;
08DB    85 04       STA $04         ; _04 = (_04 + 1) % 16
08DD    4C 98 09    JMP $0998       ;

;; ... if north wind

08E0    E6 01       INC $01         ;
08E2    E6 03       INC $03         ;
08E4    A5 03       LDA $03         ;
08E6    C9 18       CMP #$18        ;
08E8    90 03       BCC ---         ;
08EA    20 23 03    JSR $0323       ; turns disk drive motor on for a period of time?
08ED    C9 1B       CMP #$1B        ;
08EF    90 12       BCC ---         ;
08F1    29 0F       AND #$0F        ;
08F3    85 03       STA $03         ;
08F5    20 4C 09    JSR $094C       ;
08F8    E6 05       INC $05         ;
08FA    A5 05       LDA $05         ;
08FC    29 0F       AND #$0F        ;
08FE    85 05       STA $05         ;
0900    20 BC 09    JSR $09BC       ;
0903    60          RTS             ;

;; ... if south wind

0904    C6 01       DEC $01         ;
0906    C6 03       DEC $03         ;
0908    A5 03       LDA $03         ;
090A    C9 08       CMP #$08        ;
090C    B0 03       BCS $0911       ;
090E    20 23 03    JSR $0323       ; turns disk drive motor on for a period of time?
0911    C9 05       CMP #$05        ;
0913    B0 12       BCS $0927       ;
0915    09 10       ORA #$10        ;
0917    85 03       STA $03         ;
0919    20 5E 09    JSR $095E       ; copy $E8XX to $EAXX and $E9XX to $EBXX
091C    C6 05       DEC $05         ;
091E    A5 05       LDA $05         ;
0920    29 0F       AND #$0F        ;
0922    85 05       STA $05         ;
0924    20 E4 09    JSR $09E4       ; load a track/sector based on $04/$05 into $E8XX and $E9XX
0927    60          RTS

;; copy $E9XX to $E8XX and $EBXX to $EAXX

0928    A2 00       LDX #$00        ;

092A    BD 00 E9    LDA $E900,X     ;
092D    9D 00 E8    STA $E800,X     ;
0930    BD 00 EB    LDA $EB00,X     ;
0933    9D 00 EA    STA $EA00,X     ;
0936    E8          INX             ;
0937    D0 F1       BNE $092A       ;

0939    60          RTS             ;

;; copy $E8XX to $E9XX and $EAXX to $EBXX

093A    A2 00       LDX #$00        ;

093C    BD 00 E8    LDA $E800,X     ;
093F    9D 00 E9    STA $E900,X     ;
0942    BD 00 EA    LDA $EA00,X     ;
0945    9D 00 EB    STA $EB00,X     ;
0948    E8          INX             ;
0949    D0 F1       BNE $093C       ;

094B    60          RTS             ;

;; copy $EAXX to $E8XX and $EBXX to $E9XX

094C    A2 00       LDX #$00        ;

094E    BD 00 EA    LDA $EA00,X     ;
0951    9D 00 E8    STA $E800,X     ;
0954    BD 00 EB    LDA $EB00,X     ;
0957    9D 00 E9    STA $E900,X     ;
095A    E8          INX             ;
095B    D0 F1       BNE $094E       ;

095D    60          RTS             ;

;; copy $E8XX to $EAXX and $E9XX to $EBXX

095E    A2 00       LDX #$00        ;

0960    BD 00 E8    LDA $E800,X     ;
0963    9D 00 EA    STA $EA00,X     ;
0966    BD 00 E9    LDA $E900,X     ;
0969    9D 00 EB    STA $EB00,X     ;
096C    E8          INX             ;
096D    D0 F1       BNE $0960       ;

096F    60          RTS             ;

;; load a track/sector based on $04/$05 into $E9XX and $EBXX

0970    A9 01       LDA #$01        ; _E2 = 0x01 (COMMAND = READ)
0972    85 E2       STA $E2         ;
0974    18          CLC             ;
0975    A5 04       LDA $04         ;
0977    69 01       ADC #$01        ;
0979    29 0F       AND #$0F        ;
097B    85 E1       STA $E1         ; _E1 = (_04 + 1) % 16 (SECTOR NUMBER)
097D    A5 05       LDA $05         ;
097F    85 E0       STA $E0         ; _E0 = _05 (TRACK NUMBER)
0981    A9 E9       LDA #$E9        ;
0983    85 E3       STA $E3         ; _E3 = 0xE9
0985    20 7F 0A    JSR $0A7F       ; RWTS call (load into $E9XX)
0988    18          CLC             ;
0989    A5 05       LDA $05         ;
098B    69 01       ADC #$01        ;
098D    29 0F       AND #$0F        ;
098F    85 E0       STA $E0         ; _E0 = (_05 + 1) % 16 (TRACK NUMBER)
0991    A9 EB       LDA #$EB        ;
0993    85 E3       STA $E3         ; _E3 = 0xEB
0995    4C 7F 0A    JMP $0A7F       ; RWTS (load into $EBXX)

;; load a track/sector based on $04/$05 into $E8XX and $EAXX

0998    A9 01       LDA #$01        ;
099A    85 E2       STA $E2         ; _E2 = 0x01 (COMMAND = READ)
099C    18          CLC             ;
099D    A5 04       LDA $04         ;
099F    85 E1       STA $E1         ; _E1 = _04 (SECTOR NUMBER)
09A1    A5 05       LDA $05         ;
09A3    85 E0       STA $E0         ; _E0 = _05 (TRACK NUMBER)
09A5    A9 E8       LDA #$E8        ;
09A7    85 E3       STA $E3         ; _E3 = 0xE8
09A9    20 7F 0A    JSR $0A7F       ; RWTS call (load into $E8XX)
09AC    18          CLC             ;
09AD    A5 05       LDA $05         ;
09AF    69 01       ADC #$01        ;
09B1    29 0F       AND #$0F        ;
09B3    85 E0       STA $E0         ; _E0 = (_05 + 1) % 16 (TRACK NUMBER)
09B5    A9 EA       LDA #$EA        ;
09B7    85 E3       STA $E3         ; _E3 = 0xEA
09B9    4C 7F 0A    JMP $0A7F       ; RWTS (load into $EAXX)

;; load a track/sector based on $04/$05 into $EAXX and $EBXX

09BC    A9 01       LDA #$01        ;
09BE    85 E2       STA $E2         ; _E2 = 0x01 (COMMAND = READ)
09C0    18          CLC             ;
09C1    A5 05       LDA $05         ;
09C3    69 01       ADC #$01        ;
09C5    29 0F       AND #$0F        ;
09C7    85 E0       STA $E0         ; _E0 = (_05 + 1) % 16 (TRACK NUMBER)
09C9    A5 04       LDA $04         ;
09CB    85 E1       STA $E1         ; _E1 = _04 (SECTOR NUMBER)
09CD    A9 EA       LDA #$EA        ;
09CF    85 E3       STA $E3         ; _E3 = 0xEA
09D1    20 7F 0A    JSR $0A7F       ; RWTS (load into $EAXX)
09D4    18          CLC             ;
09D5    A5 04       LDA $04         ;
09D7    69 01       ADC #$01        ;
09D9    29 0F       AND #$0F        ;
09DB    85 E1       STA $E1         ; _E1 = (_04 + 1) % 16 (SECTOR NUMBER)
09DD    A9 EB       LDA #$EB        ;
09DF    85 E3       STA $E3         ; _E3 = 0xEB
09E1    4C 7F 0A    JMP $0A7F       ; RWTS (load into $EBXX)

;; load a track/sector based on $04/$05 into $E8XX and $E9XX

09E4    A9 01       LDA #$01        ;
09E6    85 E2       STA $E2         ; _E2 = 0x01 (COMMAND = READ)
09E8    18          CLC             ;
09E9    A5 05       LDA $05         ;
09EB    85 E0       STA $E0         ; _E0 = _05 (TRACK NUMBER)
09ED    A5 04       LDA $04         ;
09EF    85 E1       STA $E1         ; _E1 = _04 (SECTOR NUMBER)
09F1    A9 E8       LDA #$E8        ;
09F3    85 E3       STA $E3         ; _E3 = 0xE8
09F5    20 7F 0A    JSR $0A7F       ; RWTS (load into $E8XX)
09F8    18          CLC             ;
09F9    A5 04       LDA $04         ;
09FB    69 01       ADC #$01        ;
09FD    29 0F       AND #$0F        ;
09FF    85 E1       STA $E1         ; _E1 = (_04 + 1) % 16 (SECTOR NUMBER)
0A01    A9 E9       LDA #$E9        ;
0A03    85 E3       STA $E3         ; _E3 = 0xE9
0A05    4C 7F 0A    JMP $0A7F       ; RWTS (load into $E9XX)

;;

0A08    A5 00       LDA $00         ;
0A0A    20 01 15    JSR $1501       ; shift acc 4 bits to right (i.e. // 16)
0A0D    85 04       STA $04         ;
0A0F    A5 00       LDA $00         ;
0A11    29 0F       AND #$0F        ;
0A13    85 02       STA $02         ;
0A15    C9 08       CMP #$08        ;
0A17    B0 0D       BCS ---         ;
0A19    18          CLC             ;
0A1A    69 10       ADC #$10        ;
0A1C    85 02       STA $02         ;
0A1E    C6 04       DEC $04         ;
0A20    A5 04       LDA $04         ;
0A22    29 0F       AND #$0F        ;
0A24    85 04       STA $04         ;
0A26    A5 01       LDA $01         ;
0A28    20 01 15    JSR $1501       ; shift acc 4 bits to right (i.e. // 16)
0A2B    85 05       STA $05         ;
0A2D    A5 01       LDA $01         ;
0A2F    29 0F       AND #$0F        ;
0A31    85 03       STA $03         ;
0A33    C9 08       CMP #$08        ;
0A35    B0 0D       BCS ---         ;
0A37    18          CLC             ;
0A38    69 10       ADC #$10        ;
0A3A    85 03       STA $03         ;
0A3C    C6 05       DEC $05         ;
0A3E    A5 05       LDA $05         ;
0A40    29 0F       AND #$0F        ;
0A42    85 05       STA $05         ;
0A44    A9 01       LDA #$01        ;
0A46    85 E2       STA $E2         ;
0A48    A5 04       LDA $04         ;
0A4A    85 E1       STA $E1         ;
0A4C    A5 05       LDA $05         ;
0A4E    85 E0       STA $E0         ;
0A50    A9 E8       LDA #$E8        ;
0A52    85 E3       STA $E3         ;
0A54    20 7F 0A    JSR $0A7F       ;
0A57    18          CLC             ;
0A58    A5 04       LDA $04         ;
0A5A    69 01       ADC #$01        ;
0A5C    29 0F       AND #$0F        ;
0A5E    85 E1       STA $E1         ;
0A60    A9 E9       LDA #$E9        ;
0A62    85 E3       STA $E3         ;
0A64    20 7F 0A    JSR $0A7F       ;
0A67    18          CLC             ;
0A68    A5 05       LDA $05         ;
0A6A    69 01       ADC #$01        ;
0A6C    29 0F       AND #$0F        ;
0A6E    85 E0       STA $E0         ;
0A70    A9 EB       LDA #$EB        ;
0A72    85 E3       STA $E3         ;
0A74    20 7F 0A    JSR $0A7F       ;
0A77    A5 04       LDA $04         ;
0A79    85 E1       STA $E1         ;
0A7B    A9 EA       LDA #$EA        ;
0A7D    85 E3       STA $E3         ;

;; some wrapper around RWTS call
; $E0 - track number
; $E1 - sector number
; $E3 - data buffer high byte ?
; $E2 - command (0=SEEK, 1=READ, 2=WRITE, 4=FORMAT)

0A7F    A9 00       LDA #$00        ;
0A81    8D EB B7    STA $B7EB       ; RWTS: Volume number
0A84    8D F0 B7    STA $B7F0       ; RWTS: Pointer to user data buffer for READ/WRITE
0A87    A5 E0       LDA $E0         ;
0A89    8D EC B7    STA $B7EC       ; RWTS: Track number
0A8C    A5 E1       LDA $E1         ;
0A8E    8D ED B7    STA $B7ED       ; RWTS: Sector number
0A91    A5 E3       LDA $E3         ;
0A93    8D F1 B7    STA $B7F1       ; RWTS: Pointer to user data buffer for READ/WRITE
0A96    A5 E2       LDA $E2         ;
0A98    8D F4 B7    STA $B7F4       ; RWTS: Command code: 0=SEEK, 1=READ, 2=WRITE, 4=FORMAT
0A9B    A9 AD       LDA #$AD        ;
0A9D    85 4E       STA $4E         ;
0A9F    8D 5D BD    STA $BD5D       ; ???
0AA2    A9 9B       LDA #$9B        ;
0AA4    8D 2C BF    STA $BF2C       ; ??? in TRACK WRITE routine
0AA7    A9 E8       LDA #$E8        ;
0AA9    A0 B7       LDY #$B7        ;
0AAB    20 B5 B7    JSR $B7B5       ; Disable interrupts and call RWTS
0AAE    A9 00       LDA #$00        ;
0AB0    85 48       STA $48         ;
0AB2    B0 CB       BCS $0A7F       ;
0AB4    60          RTS             ;

;;

0AB5    A9 80       LDA #$80        ; _0ADB = 0x00
0AB7    8D DB 0A    STA $0ADB       ; .

0ABA    20 0C 15    JSR $150C       ; process any keypress onto $B0 buffer
0ABD    20 67 10    JSR $1067       ; rotate cursor, print it, and decrement text column
0AC0    20 2E 15    JSR $152E       ; basically doing a for Y in range [0, _B8]: _B0[Y] = _B1[Y]
0AC3    30 0E       BMI $0AD3       ;
0AC5    20 DC 0A    JSR $0ADC       ;
0AC8    CE DB 0A    DEC $0ADB       ; _0ABD--
0ACB    D0 ED       BNE $0ABA       ; if _0ADB != 0, loop back

0ACD    20 73 10    JSR $1073       ; print space and decrement text column
0AD0    A9 00       LDA #$00        ;
0AD2    60          RTS             ;

0AD3    48          PHA             ;
0AD4    20 73 10    JSR $1073       ; print space and decrement text column
0AD7    68          PLA             ;
0AD8    C9 00       CMP #$00        ;
0ADA    60          RTS             ;

;;;

0ADB  00

;;

0ADC    A5 0B       LDA $0B         ;
0ADE    F0 32       BEQ $0B12       ; if _0B == 0x00, go to $0B12
0AE0    30 26       BMI $0B08       ; if _0B >= 0x80, go to $0B08
0AE2    C9 01       CMP #$01        ; only do following if _0B == 0x01
0AE4    D0 0A       BNE $0AF0       ; otherwise go to $0AF0

; _0B == 0x01

0AE6    20 8F 11    JSR $118F       ;    randomly change wind
0AE9    20 1D 0B    JSR $0B1D       ;    ?
0AEC    20 7A 0C    JSR $0C7A       ;    ?
0AEF    60          RTS             ;

;;

0AF0    C9 02       CMP #$02        ; if A == 0x02, go to $0C13, else $0AF8
0AF2    D0 04       BNE $0AF8       ; .
0AF4    20 13 0C    JSR $0C13       ; .
0AF7    60          RTS             ;

;;

0AF8    C9 03       CMP #$03        ;
0AFA    D0 09       BNE $0B05       ; only do the following if A == 0x03:
0AFC    20 97 13    JSR $1397       ;   some sort of cycle of shapes in memory banks
0AFF    20 18 14    JSR $1418       ;
0B02    20 03 8C    JSR $8C03       ;
0B05    4C 12 0B    JMP $0B12       ; wait a period of time (and return)

;;

0B08    C9 FF       CMP #$FF        ;
0B0A    F0 03       BEQ $0B0F       ;

0B0C    4C 52 0B    JMP $0B52       ;

0B0F    4C 49 0B    JMP $0B49       ;

;; wait a period of time

0B12    A2 4B       LDX #$4B        ;
0B14    A0 00       LDY #$00        ;
0B16    88          DEY             ;
0B17    D0 FD       BNE $0B16       ;

0B19    CA          DEX             ;
0B1A    D0 FA       BNE $0B16       ;

0B1C    60          RTS             ;

;;

0B1D    A5 0E       LDA $0E         ; if _0E == 0x18
0B1F    C9 18       CMP #$18        ; .
0B21    D0 24       BNE $0B47       ;   return
0B23    A5 F4       LDA $F4         ; if _F4 == 0x00
0B25    F0 20       BEQ $0B47       ;   return
0B27    CE 48 0B    DEC $0B48       ; decrement _0B48
0B2A    AD 48 0B    LDA $0B48       ; check last
0B2D    29 03       AND #$03        ; two bits of _B48
0B2F    D0 16       BNE $0B47       ; if they're not both zero, return
0B31    A6 F5       LDX $F5         ; get wind direction (or something else???)
0B33    D0 03       BNE $0B38       ; if WEST,
0B35    4C 99 08    JMP $0899       ;    jump $0899

0B38    CA          DEX             ; if NORTH,
0B39    D0 03       BNE $0B3E       ; .

0B3B    4C E0 08    JMP $08E0       ;     jump $08E0

0B3E    CA          DEX             ; if EAST,
0B3F    D0 03       BNE $0B44       ; .

0B41    4C BD 08    JMP $08BD       ;     jump $08BD

0B44    4C 04 09    JMP $0904       ; else (if SOUTH) jump $0904

0B47    60          RTS             ;

;; DATA

0B48  00

;;

0B49  20 97 13      JSR $1397

0B4C  20 18 14      JSR $1418
0B4F  4C 74 0E      JMP $0E74       ; draw map

;;

0B52    20 97 13    JSR $1397       ;
0B55    20 18 14    JSR $1418       ;
0B58    A2 7F       LDX #$7F        ;
0B5A    BD 80 02    LDA $0280,X     ;
0B5D    9D 00 02    STA $0200,X     ;
0B60    CA          DEX             ;
0B61    10 F7       BPL ---         ;
0B63    A2 0F       LDX #$0F        ;
0B65    BC 10 EF    LDY $EF10,X     ;
0B68    B9 51 1F    LDA $1F51,Y     ;
0B6B    18          CLC             ;
0B6C    7D 00 EF    ADC $EF00,X     ;
0B6F    A8          TAY             ;
0B70    BD 50 EF    LDA $EF50,X     ;
0B73    F0 47       BEQ ---         ;
0B75    BD 70 EF    LDA $EF70,X     ;
0B78    D0 3C       BNE ---         ;
0B7A    BD 50 EF    LDA $EF50,X     ;
0B7D    C9 90       CMP #$90        ;
0B7F    B0 12       BCS ---         ;
0B81    20 4A 15    JSR $154A       ; random number generator?
0B84    29 01       AND #$01        ;
0B86    18          CLC             ;
0B87    7D 50 EF    ADC $EF50,X     ;
0B8A    9D 60 EF    STA $EF60,X     ;
0B8D    99 00 02    STA $0200,Y     ;
0B90    4C BC 0B    JMP $0BBC       ;
0B93    C9 AC       CMP #$AC        ;
0B95    D0 0D       BNE ---         ;
0B97    BD 60 EF    LDA $EF60,X     ;
0B9A    C9 3C       CMP #$3C        ;
0B9C    D0 06       BNE ---         ;
0B9E    99 00 02    STA $0200,Y     ;
0BA1    4C BC 0B    JMP $0BBC       ;

0BA4    20 4A 15    JSR $154A       ; random number generator?
0BA7    29 01       AND #$01        ;
0BA9    F0 0B       BEQ ---         ;
0BAB    FE 60 EF    INC $EF60,X     ;
0BAE    BD 60 EF    LDA $EF60,X     ;
0BB1    29 03       AND #$03        ;
0BB3    4C 86 0B    JMP $0B86       ;

0BB6    BD 60 EF    LDA $EF60,X     ;
0BB9    99 00 02    STA $0200,Y     ;
0BBC    CA          DEX             ;
0BBD    10 A6       BPL ---         ;
0BBF    A6 0F       LDX $0F         ;
0BC1    BD 9F EF    LDA $EF9F,X     ;
0BC4    F0 31       BEQ ---         ;
0BC6    BC 8F EF    LDY $EF8F,X     ;
0BC9    B9 51 1F    LDA $1F51,Y     ;
0BCC    18          CLC             ;
0BCD    7D 7F EF    ADC $EF7F,X     ;
0BD0    A8          TAY             ;
0BD1    BD 9F EF    LDA $EF9F,X     ;
0BD4    C9 38       CMP #$38        ;
0BD6    F0 1C       BEQ ---         ;
0BD8    E4 C5       CPX $C5         ;
0BDA    D0 0F       BNE ---         ;
0BDC    CE 48 0B    DEC $0B48       ;
0BDF    AD 48 0B    LDA $0B48       ;
0BE2    29 03       AND #$03        ;
0BE4    D0 05       BNE ---         ;
0BE6    A9 7D       LDA #$7D        ;
0BE8    4C F4 0B    JMP $0BF4       ;

0BEB    20 4A 15    JSR $154A       ; random number generator?
0BEE    29 01       AND #$01        ;
0BF0    18          CLC             ;
0BF1    7D 9F EF    ADC $EF9F,X     ;
0BF4    99 00 02    STA $0200,Y     ;
0BF7    CA          DEX             ;
0BF8    D0 C7       BNE ---         ;
0BFA    AD FD EF    LDA $EFFD       ;
0BFD    F0 11       BEQ ---         ;
0BFF    AE FF EF    LDX $EFFF       ;
0C02    BD 51 1F    LDA $1F51,X     ;
0C05    18          CLC             ;
0C06    6D FE EF    ADC $EFFE       ;
0C09    A8          TAY             ;
0C0A    AD FD EF    LDA $EFFD       ;
0C0D    99 00 02    STA $0200,Y     ;
0C10    4C 74 0E    JMP $0E74       ; draw map

;; this is called when $084B/$0ADC is called with $0B = 0x02

0C13    20 0F 0F    JSR $0F0F       ; (something to do with EE00)
0C16    38          SEC             ;
0C17    A5 00       LDA $00         ;
0C19    E9 05       SBC #$05        ;
0C1B    85 C0       STA $C0         ;
0C1D    38          SEC             ;
0C1E    A5 01       LDA $01         ;
0C20    E9 05       SBC #$05        ;
0C22    85 C1       STA $C1         ;
0C24    A9 00       LDA #$00        ;
0C26    85 C2       STA $C2         ;
0C28    85 C3       STA $C3         ;
0C2A    A8          TAY             ;
0C2B    AA          TAX             ;

0C2C    18          CLC             ;
0C2D    A5 C0       LDA $C0         ;
0C2F    65 C2       ADC $C2         ;
0C31    85 C4       STA $C4         ;
0C33    C9 20       CMP #$20        ;
0C35    B0 28       BCS $0C5F       ;

0C37    18          CLC             ;
0C38    A5 C1       LDA $C1         ;
0C3A    65 C3       ADC $C3         ;
0C3C    85 C5       STA $C5         ;
0C3E    C9 20       CMP #$20        ;
0C40    B0 1D       BCS $0C5F       ;

0C42    85 FD       STA $FD         ;
0C44    A9 00       LDA #$00        ;
0C46    46 FD       LSR $FD         ;
0C48    6A          ROR A           ;
0C49    46 FD       LSR $FD         ;
0C4B    6A          ROR A           ;
0C4C    46 FD       LSR $FD         ;
0C4E    6A          ROR A           ;
0C4F    65 C4       ADC $C4         ;
0C51    85 FC       STA $FC         ;
0C53    18          CLC             ;
0C54    A5 FD       LDA $FD         ;
0C56    69 E8       ADC #$E8        ;
0C58    85 FD       STA $FD         ;
0C5A    B1 FC       LDA ($FC),Y     ;
0C5C    4C 61 0C    JMP $0C61       ;

0C5F    A9 04       LDA #$04        ;

0C61    9D 80 02    STA $0280,X     ;
0C64    E8          INX             ;
0C65    E6 C2       INC $C2         ;
0C67    A5 C2       LDA $C2         ;
0C69    C9 0B       CMP #$0B        ;
0C6B    D0 BF       BNE $0C2C       ;
0C6D    84 C2       STY $C2         ;
0C6F    E6 C3       INC $C3         ;
0C71    A5 C3       LDA $C3         ;
0C73    C9 0B       CMP #$0B        ;
0C75    D0 B5       BNE $0C2C       ;
0C77    4C D8 0C    JMP $0CD8       ;

;;

0C7A    20 0F 0F    JSR $0F0F       ;
0C7D    20 7C 17    JSR $177C       ;
0C80    38          SEC             ;
0C81    A5 02       LDA $02         ;
0C83    E9 05       SBC #$05        ;
0C85    85 C0       STA $C0         ;
0C87    38          SEC             ;
0C88    A5 03       LDA $03         ;
0C8A    E9 05       SBC #$05        ;
0C8C    85 C1       STA $C1         ;
0C8E    A9 00       LDA #$00        ;
0C90    85 C2       STA $C2         ;
0C92    85 C3       STA $C3         ;
0C94    A8          TAY             ;
0C95    AA          TAX             ;

0C96    18          CLC             ;
0C97    A5 C0       LDA $C0         ;
0C99    65 C2       ADC $C2         ;
0C9B    85 C4       STA $C4         ;
0C9D    18          CLC             ;
0C9E    A5 C1       LDA $C1         ;
0CA0    65 C3       ADC $C3         ;
0CA2    85 C5       STA $C5         ;
0CA4    20 07 15    JSR $1507       ;
0CA7    85 FC       STA $FC         ;
0CA9    A5 C4       LDA $C4         ;
0CAB    29 0F       AND #$0F        ;
0CAD    05 FC       ORA $FC         ;
0CAF    85 FC       STA $FC         ;
0CB1    A5 C5       LDA $C5         ;
0CB3    29 10       AND #$10        ;
0CB5    0A          ASL A           ;
0CB6    05 C4       ORA $C4         ;
0CB8    20 01 15    JSR $1501       ; shift acc 4 bits to right (i.e. // 16)
0CBB    18          CLC             ;
0CBC    69 E8       ADC #$E8        ;
0CBE    85 FD       STA $FD         ;
0CC0    B1 FC       LDA ($FC),Y     ;
0CC2    9D 80 02    STA $0280,X     ;
0CC5    E8          INX             ;
0CC6    E6 C2       INC $C2         ;
0CC8    A5 C2       LDA $C2         ;
0CCA    C9 0B       CMP #$0B        ;
0CCC    D0 C8       BNE $0C96       ;
0CCE    84 C2       STY $C2         ;
0CD0    E6 C3       INC $C3         ;
0CD2    A5 C3       LDA $C3         ;
0CD4    C9 0B       CMP #$0B        ;
0CD6    D0 BE       BNE $0C96       ;

;;

0CD8    A2 1F       LDX #$1F        ;

0CDA    BD 00 EE    LDA $EE00,X     ;
0CDD    F0 2D       BEQ $0D0C       ;

0CDF    BD 20 EE    LDA $EE20,X     ;
0CE2    18          CLC             ;
0CE3    69 05       ADC #$05        ;
0CE5    38          SEC             ;
0CE6    E5 00       SBC $00         ;
0CE8    C9 0B       CMP #$0B        ;
0CEA    B0 20       BCS $0D0C       ;

0CEC    85 C4       STA $C4         ;
0CEE    BD 40 EE    LDA $EE40,X     ;
0CF1    18          CLC             ;
0CF2    69 05       ADC #$05        ;
0CF4    38          SEC             ;
0CF5    E5 01       SBC $01         ;
0CF7    C9 0B       CMP #$0B        ;
0CF9    B0 11       BCS $0D0C       ;

0CFB    85 C5       STA $C5         ;
0CFD    A4 C5       LDY $C5         ;
0CFF    B9 51 1F    LDA $1F51,Y     ;
0D02    18          CLC             ;
0D03    65 C4       ADC $C4         ;
0D05    A8          TAY             ;
0D06    BD 00 EE    LDA $EE00,X     ;
0D09    99 80 02    STA $0280,Y     ;

0D0C    CA          DEX             ;
0D0D    10 CB       BPL $0CDA       ;

0D0F    A5 ED       LDA $ED         ;
0D11    F0 2A       BEQ $0D3D       ;

0D13    A5 EE       LDA $EE         ;
0D15    18          CLC             ;
0D16    69 05       ADC #$05        ;
0D18    38          SEC             ;
0D19    E5 00       SBC $00         ;
0D1B    C9 0B       CMP #$0B        ;
0D1D    B0 1E       BCS $0D3D       ;
0D1F    85 C4       STA $C4         ;
0D21    A5 EF       LDA $EF         ;
0D23    18          CLC             ;
0D24    69 05       ADC #$05        ;
0D26    38          SEC             ;
0D27    E5 01       SBC $01         ;
0D29    C9 0B       CMP #$0B        ;
0D2B    B0 10       BCS $0D3D       ;
0D2D    85 C5       STA $C5         ;
0D2F    A4 C5       LDY $C5         ;
0D31    B9 51 1F    LDA $1F51,Y     ;
0D34    18          CLC             ;
0D35    65 C4       ADC $C4         ;
0D37    A8          TAY             ;
0D38    A5 ED       LDA $ED         ;
0D3A    99 80 02    STA $0280,Y     ;

0D3D    AD B1 02    LDA $02B1       ;
0D40    85 C9       STA $C9         ;
0D42    AD C7 02    LDA $02C7       ;
0D45    85 CA       STA $CA         ;
0D47    AD BD 02    LDA $02BD       ;
0D4A    85 CB       STA $CB         ;
0D4C    AD BB 02    LDA $02BB       ;
0D4F    85 CC       STA $CC         ;
0D51    AD BC 02    LDA $02BC       ;
0D54    85 C8       STA $C8         ;
0D56    A5 0E       LDA $0E         ;
0D58    8D BC 02    STA $02BC       ;
0D5B    A5 0D       LDA $0D         ;
0D5D    F0 0E       BEQ $0D6D       ;
0D5F    A2 78       LDX #$78        ;

0D61    BD 80 02    LDA $0280,X     ;
0D64    9D 00 02    STA $0200,X     ;
0D67    CA          DEX             ;
0D68    10 F7       BPL $0D61       ;
0D6A    4C 74 0E    JMP $0E74       ; draw map

;;

0D6D    A2 78       LDX #$78        ;
0D6F    A9 7E       LDA #$7E        ;

0D71    9D 00 02    STA $0200,X     ;
0D74    CA          DEX             ;
0D75    10 FA       BPL $0D71       ;
0D77    A9 78       LDA #$78        ;
0D79    85 F0       STA $F0         ;
0D7B    A9 0A       LDA #$0A        ;
0D7D    85 C4       STA $C4         ;
0D7F    85 C5       STA $C5         ;

0D81    A5 C4       LDA $C4         ;
0D83    85 C0       STA $C0         ;
0D85    A5 C5       LDA $C5         ;
0D87    85 C1       STA $C1         ;
0D89    A5 F0       LDA $F0         ;
0D8B    85 F1       STA $F1         ;

0D8D    A6 C0       LDX $C0         ;
0D8F    A4 C1       LDY $C1         ;
0D91    A5 F1       LDA $F1         ;
0D93    18          CLC             ;
0D94    7D D6 0E    ADC $0ED6,X     ;
0D97    18          CLC             ;
0D98    79 E1 0E    ADC $0EE1,Y     ;
0D9B    C9 3C       CMP #$3C        ;
0D9D    F0 2F       BEQ $0DCE       ;

0D9F    85 F1       STA $F1         ;
0DA1    AA          TAX             ;
0DA2    BD 80 02    LDA $0280,X     ;
0DA5    C9 06       CMP #$06        ;
0DA7    F0 2D       BEQ $0DD6       ;
0DA9    C9 08       CMP #$08        ;
0DAB    F0 29       BEQ $0DD6       ;
0DAD    C9 49       CMP #$49        ;
0DAF    F0 25       BEQ $0DD6       ;
0DB1    C9 7E       CMP #$7E        ;
0DB3    F0 21       BEQ $0DD6       ;
0DB5    C9 7F       CMP #$7F        ;
0DB7    F0 1D       BEQ $0DD6       ;
0DB9    A5 C0       LDA $C0         ;
0DBB    AA          TAX             ;
0DBC    18          CLC             ;
0DBD    7D D6 0E    ADC $0ED6,X     ;
0DC0    85 C0       STA $C0         ;
0DC2    A5 C1       LDA $C1         ;
0DC4    AA          TAX             ;
0DC5    18          CLC             ;
0DC6    7D D6 0E    ADC $0ED6,X     ;
0DC9    85 C1       STA $C1         ;
0DCB    4C 8D 0D    JMP $0D8D       ;

;;

0DCE    A6 F0       LDX $F0         ;
0DD0    BD 80 02    LDA $0280,X     ;
0DD3    9D 00 02    STA $0200,X     ;

0DD6    C6 F0       DEC $F0         ;
0DD8    C6 C4       DEC $C4         ;
0DDA    10 A5       BPL $0D81       ;
0DDC    A9 0A       LDA #$0A        ;
0DDE    85 C4       STA $C4         ;
0DE0    C6 C5       DEC $C5         ;
0DE2    10 9D       BPL $0D81       ;
0DE4    A9 78       LDA #$78        ;
0DE6    85 F0       STA $F0         ;
0DE8    A9 0A       LDA #$0A        ;
0DEA    85 C4       STA $C4         ;
0DEC    85 C5       STA $C5         ;

;;

0DEE    A5 C4       LDA $C4         ;
0DF0    85 C0       STA $C0         ;
0DF2    A5 C5       LDA $C5         ;
0DF4    85 C1       STA $C1         ;
0DF6    A5 F0       LDA $F0         ;
0DF8    85 F1       STA $F1         ;
0DFA    AA          TAX             ;
0DFB    BD 00 02    LDA $0200,X     ;
0DFE    C9 7E       CMP #$7E        ;
0E00    D0 61       BNE $0E63       ;
0E02    A6 C0       LDX $C0         ;
0E04    A4 C1       LDY $C1         ;
0E06    BD EC 0E    LDA $0EEC,X     ;
0E09    D9 EC 0E    CMP $0EEC,Y     ;
0E0C    F0 04       BEQ $0E12       ;
0E0E    90 21       BCC ---         ;
0E10    B0 13       BCS $0E25       ;
0E12    A5 F1       LDA $F1         ;
0E14    18          CLC             ;
0E15    7D D6 0E    ADC $0ED6,X     ;
0E18    18          CLC             ;
0E19    79 E1 0E    ADC $0EE1,Y     ;
0E1C    20 F7 0E    JSR $0EF7       ;
0E1F    20 03 0F    JSR $0F03       ;
0E22    4C 3A 0E    JMP $0E3A       ;

;;

0E25    A5 F1       LDA $F1         ;
0E27    18          CLC             ;
0E28    7D D6 0E    ADC $0ED6,X     ;
0E2B    20 F7 0E    JSR $0EF7       ;
0E2E    4C 3A 0E    JMP $0E3A       ;

0E31    A5 F1       LDA $F1         ;
0E33    18          CLC             ;
0E34    79 E1 0E    ADC $0EE1,Y     ;
0E37    20 03 0F    JSR $0F03       ;
0E3A    C9 3C       CMP #$3C        ;
0E3C    F0 1D       BEQ ---         ;
0E3E    85 F1       STA $F1         ;
0E40    AA          TAX             ;
0E41    BD 80 02    LDA $0280,X     ;
0E44    C9 06       CMP #$06        ;
0E46    F0 1B       BEQ ---         ;
0E48    C9 08       CMP #$08        ;
0E4A    F0 17       BEQ ---         ;
0E4C    C9 49       CMP #$49        ;
0E4E    F0 13       BEQ ---         ;
0E50    C9 7E       CMP #$7E        ;
0E52    F0 0F       BEQ ---         ;
0E54    C9 7F       CMP #$7F        ;
0E56    F0 0B       BEQ ---         ;
0E58    4C 02 0E    JMP $0E02       ;

;;

0E5B    A6 F0       LDX $F0         ;
0E5D    BD 80 02    LDA $0280,X     ;
0E60    9D 00 02    STA $0200,X     ;

0E63    C6 F0       DEC $F0         ;
0E65    C6 C4       DEC $C4         ;
0E67    10 85       BPL ---         ;
0E69    A9 0A       LDA #$0A        ;
0E6B    85 C4       STA $C4         ;
0E6D    C6 C5       DEC $C5         ;
0E6F    30 03       BMI ---         ;
0E71    4C EE 0D    JMP $0DEE       ;
```

```
;; draw map (described in $02XX?)

0E74    A9 00       LDA #$00        ;
0E76    85 F2       STA $F2         ; we set $F2 to 0x00
0E78    8D 9D 0E    STA $0E9D       ; we also set the low byte of address in $02XX to load in $0E9C

0E7B    A4 F2       LDY $F2         ; Y = 0x00
0E7D    B9 08 E0    LDA $E008,Y     ; A = 0x80
0E80    8D A6 0E    STA $0EA6       ; set low byte of video RAM to write to in $0EA5
0E83    8D B0 0E    STA $0EB0       ; set low byte of video RAM to write to in $0EAF
0E86    B9 C8 E0    LDA $E0C8,Y     ; A = 0x20
0E89    8D A7 0E    STA $0EA7       ; set high byte of video RAM to write to in $0EA5
0E8C    8D B1 0E    STA $0EB1       ; set high byte of video RAM to write to in $0EAF
0E8F    98          TYA             ; A = Y = 0x00
0E90    29 0F       AND #$0F        ; 0x00
0E92    09 D0       ORA #$D0        ; 0xD0
0E94    8D A4 0E    STA $0EA4       ; set high byte of D000 bank 1 to read (SHP0)
0E97    8D AE 0E    STA $0EAE       ; set high byte of D000 bank 2 to read (SHP1)

;; loop from X = 1 to 17 (exclusive)

; these are the rows of the shape

0E9A    A2 01       LDX #$01        ; X = 0x01

;; get the shape number from 02XX,
;; look up the Xth row in the D000 banks (SHP0 and SHP1 files),
;; and write to video RAM

0E9C    AC 00 02    LDY $02__       ; Y = ($0200)
0E9F    2C 8B C0    BIT $C08B       ; use D000 bank 1 R/W
0EA2    B9 00 FF    LDA $__00,Y     ; A = _D000[Y]
0EA5    9D FF FF    STA $____,X     ; 9D 80 20   STA $2080,X
0EA8    E8          INX             ; X++
0EA9    2C 83 C0    BIT $C083       ; use D000 bank 2 R/W
0EAC    B9 00 FF    LDA $__00,Y     ; $D000
0EAF    9D FF FF    STA $____,X     ; 9D 80 20   STA $2080,X

0EB2    EE 9D 0E    INC $0E9D       ; increment our position in $02XX
0EB5    E8          INX             ; and increment X
0EB6    E0 17       CPX #$17        ; if X < 0x17
0EB8    90 E2       BCC $0E9C       ;   loop back

0EBA    E6 F2       INC $F2         ; increment $F2
0EBC    A5 F2       LDA $F2         ;
0EBE    C9 B0       CMP #$B0        ; if $F2 == 0xB0
0EC0    F0 13       BEQ $0ED5       ; return

0EC2    29 0F       AND #$0F        ;
0EC4    F0 B5       BEQ $0E7B       ;

0EC6    AD 9D 0E    LDA $0E9D       ; go back 11 tiles
0EC9    38          SEC             ; .
0ECA    E9 0B       SBC #$0B        ; .
0ECC    8D 9D 0E    STA $0E9D       ; set the low byte of address in $02XX to load in $0E9C
0ECF    20 0C 15    JSR $150C       ; process any keypress onto $B0 buffer
0ED2    4C 7B 0E    JMP $0E7B       ; loop back

0ED5    60          RTS             ;
```

```
;; DATA (3 arrays)

0ED6  01 01 01 01 01 00 FF FF FF FF FF
0EE1  0B 0B 0B 0B 0B 00 F5 F5 F5 F5 F5
0EEC  05 04 03 02 01 00 01 02 03 04 05

;; _C0 := _C0 + _0ED6[_C0]  (keep A untouched)
;;
;;   if 0 <= C0 < 5,    C0++
;;   if C0 == 5,        C0 const.
;;   if 5 < C0 <= 10,   C0--

0EF7    48          PHA             ;
0EF8    A5 C0       LDA $C0         ;
0EFA    AA          TAX             ;
0EFB    18          CLC             ;
0EFC    7D D6 0E    ADC $0ED6,X     ;
0EFF    85 C0       STA $C0         ;
0F01    68          PLA             ;
0F02    60          RTS             ;

;; _C1 := _C1 + _0ED6[_C1]  (keep A untouched)
;;
;;   if 0 <= C1 < 5,    C1++
;;   if C1 == 5,        C1 const.
;;   if 5 < C1 <= 10,   C1--

0F03    48          PHA             ;
0F04    A5 C1       LDA $C1         ;
0F06    AA          TAX             ;
0F07    18          CLC             ;
0F08    7D D6 0E    ADC $0ED6,X     ;
0F0B    85 C1       STA $C1         ;
0F0D    68          PLA             ;
0F0E    60          RTS             ;

;;

0F0F    A2 00       LDX #$00        ;

0F11    BD 60 EE    LDA $EE60,X     ;
0F14    F0 58       BEQ $0F6E       ; if _EE60[X] == 0,     go to $0F6E
0F16    10 37       BPL $0F4F       ; if _EE60[X] > 0,      go to $0F4F
0F18    C9 90       CMP #$90        ;
0F1A    B0 1B       BCS $0F37       ; if _EE60[X] >= 0x90,  go to $0F37
0F1C    C9 80       CMP #$80        ;
0F1E    F0 53       BEQ $0F73       ; if _EE60[X] == 0x80,  go to $0F73

0F20    20 7B 15    JSR $157B       ; some seeding of the random number generator??
0F23    C9 C0       CMP #$C0        ;
0F25    B0 4C       BCS $0F73       ; if ??? >= 0xC0,       go to $0F73
0F27    BD 00 EE    LDA $EE00,X     ;
0F2A    49 01       EOR #$01        ;
0F2C    29 01       AND #$01        ;
0F2E    1D 60 EE    ORA $EE60,X     ;
0F31    9D 00 EE    STA $EE00,X     ; EE00[X] == EE60[X] but low bit also set if EE00[X] low bit was clear
0F34    4C 73 0F    JMP $0F73       ;

0F37    20 7B 15    JSR $157B       ; some seeding of the random number generator??
0F3A    C9 C0       CMP #$C0        ;
0F3C    B0 35       BCS $0F73       ; if ??? >= 0xC0,       go to $0F73
0F3E    BD 00 EE    LDA $EE00,X     ;
0F41    18          CLC             ;
0F42    69 01       ADC #$01        ;
0F44    29 03       AND #$03        ;
0F46    1D 60 EE    ORA $EE60,X     ;
0F49    9D 00 EE    STA $EE00,X     ; EE00[X] == EE60[X] || ((EE00[X] + 1) % 4)
0F4C    4C 73 0F    JMP $0F73       ;

0F4F    C9 50       CMP #$50        ;
0F51    90 07       BCC $0F5A       ; if _EE60[X] < 0x50,   go to $0F5A
0F53    C9 60       CMP #$60        ;
0F55    B0 0E       BCS $0F65       ; if _EE60[X] >= 0x60,  go to $0F65
0F57    4C 20 0F    JMP $0F20       ;

0F5A    C9 20       CMP #$20        ;
0F5C    90 07       BCC $0F65       ; if _EE60[X] < 0x20,   go to $0F65
0F5E    C9 30       CMP #$30        ;
0F60    B0 03       BCS $0F65       ; if _EE60[X] >= 0x30,  go to $0F65
0F62    4C 20 0F    JMP $0F20       ;

0F65    BD 60 EE    LDA $EE60,X     ;
0F68    9D 00 EE    STA $EE00,X     ; EE00[X] == EE60[X]
0F6B    4C 73 0F    JMP $0F73       ;

0F6E    A9 00       LDA #$00        ;
0F70    9D 00 EE    STA $EE00,X     ; EE00[X] == 0x00

0F73    E8          INX             ; X++
0F74    E0 20       CPX #$20        ;
0F76    90 99       BCC $0F11       ; if X < 0x20, go back to $0F11

0F78    20 97 13    JSR $1397       ;
0F7B    20 18 14    JSR $1418       ;
0F7E    4C 7D 14    JMP $147D       ;

;; set text col, row to X, Y then print (in-game graphical)

0F81    86 CE       STX $CE         ; text col
0F83    84 CF       STY $CF         ; text row

;; another sort of PRINT (maybe in-game graphical)

0F85    68          PLA             ; pull return address off stack
0F86    8D 96 0F    STA $0F96       ; and put them in $0F96-$0F97
0F89    68          PLA             ; .
0F8A    8D 97 0F    STA $0F97       ; .

0F8D    EE 96 0F    INC $0F96       ; increment by one, carrying if necessary
0F90    D0 03       BNE $0F95       ; .
0F92    EE 97 0F    INC $0F97       ; .

0F95    AD FF FF    LDA $____       ; load from incremented address pulled off stack
0F98    F0 06       BEQ $0FA0       ; break out if zero

0F9A    20 A9 0F    JSR $0FA9       ; otherwise call $0FA9 (display character)
0F9D    4C 8D 0F    JMP $0F8D       ; increment by one again

0FA0    AD 97 0F    LDA $0F97       ; put the address in $0F96-$0F97 back on to stack
0FA3    48          PHA             ; .
0FA4    AD 96 0F    LDA $0F96       ; .
0FA7    48          PHA             ; .

0FA8    60          RTS             ;

;; display character (in-game graphics?)

0FA9    C9 8D       CMP #$8D        ; if it's a new line,
0FAB    F0 13       BEQ $0FC0       ;     go to $0FC0
0FAD    29 7F       AND #$7F        ; get low 7-bits
0FAF    A6 CE       LDX $CE         ; put col in X
0FB1    E0 28       CPX #$28        ; compare to #$28
0FB3    90 05       BCC $0FBA       ; if col < #$28, go to $0FBA
0FB5    48          PHA             ; otherwise we need to scroll
0FB6    20 C0 0F    JSR $0FC0       ; by going to $0FC0
0FB9    68          PLA             ;

0FBA    20 D7 15    JSR $15D7       ; call $15D7 (accumulator has character code)
0FBD    E6 CE       INC $CE         ; increment col
0FBF    60          RTS             ; return

;; scroll/newline (in-game graphics?)

0FC0    A9 18       LDA #$18        ; set col to #$18 (24)
0FC2    85 CE       STA $CE         ; .
0FC4    E6 CF       INC $CF         ; increment row
0FC6    A5 CF       LDA $CF         ;
0FC8    C9 18       CMP #$18        ; compare row to #$18
0FCA    90 05       BCC $0FD1       ; if row < #$18, return

0FCC    C6 CF       DEC $CF         ; otherwise decrement row
0FCE    20 94 15    JSR $1594       ;

0FD1    60          RTS             ;

;; PRINT FOLLOWING NULL-TERMINATED STRING

; put the return value (location of string to print) in $0326 and in $FE
; put the stack pointer in $0328
; loop over the string, displaying it with output routine until null hit
; put null in $B8 (why?) and return to point just after string

0FD2  68            PLA
0FD3  8D 26 03      STA $0326
0FD6  85 FE         STA $FE
0FD8  68            PLA
0FD9  8D 27 03      STA $0327
0FDC  85 FF         STA $FF
0FDE  BA            TSX
0FDF  8E 28 03      STX $0328
0FE2  A0 00         LDY #$00
0FE4  E6 FE         INC $FE
0FE6  D0 02         BNE $0FEA

0FE8  E6 FF         INC $FF

0FEA  B1 FE         LDA ($FE),Y
0FEC  F0 08         BEQ $0FF6

0FEE  09 80         ORA #$80
0FF0  20 FF 0F      JSR $0FFF
0FF3  4C E4 0F      JMP $0FE4

; hit null terminator
0FF6  85 B8         STA $B8
0FF8  A5 FF         LDA $FF
0FFA  48            PHA
0FFB  A5 FE         LDA $FE
0FFD  48            PHA

0FFE  60            RTS

0FFF  6C 36 00      JMP ($0036)   ; OUTPUT ROUTINE

;; set $FE/$FF vector to character data for character $D4

; $FE = 32 x (character_num - 1)
; $FF = 0xEC

1002    A5 D4       LDA $D4         ; load character number
1004    38          SEC             ;
1005    E9 01       SBC #$01        ;
1007    20 06 15    JSR $1506       ; x32
100A    85 FE       STA $FE         ;
100C    A9 EC       LDA #$EC        ;
100E    85 FF       STA $FF         ; $FE/$FF vector = 0xECXX where X is 0x20 times ($D4 - 1)
1010    60          RTS             ;

;; display player name? (how is this different from $104A ?)

1011    20 02 10    JSR $1002       ; set $FE/$FF vector based on $D4
1014    A9 00       LDA #$00        ;
1016    85 DA       STA $DA         ; _DA = 0x00

1018    A4 DA       LDY $DA         ; do {
101A    B1 FE       LDA ($FE),Y     ;
101C    F0 08       BEQ $1026       ; if _FE[_DA] == 0, break
101E    E6 DA       INC $DA         ;   _DA++
1020    A5 DA       LDA $DA         ;
1022    C9 0F       CMP #$0F        ;
1024    90 F2       BCC $1018       ; } while _DA < 0x0F

1026    A9 0F       LDA #$0F        ;
1028    38          SEC             ;
1029    E5 DA       SBC $DA         ;
102B    4A          LSR A           ;
102C    18          CLC             ;
102D    65 CE       ADC $CE         ; text col?
102F    85 CE       STA $CE         ; text col?
```

```
1031    20 02 10    JSR $1002       ; set $FE/$FF vector based on $D4
1034    A9 00       LDA #$00        ;
1036    85 DA       STA $DA         ; _DA = 0x00

1038    A4 DA       LDY $DA         ; do {
103A    B1 FE       LDA ($FE),Y     ;
103C    F0 0B       BEQ $1049       ;   if _FE[_DA] == 0, break
103E    20 A9 0F    JSR $0FA9       ;   print character in _FE[_DA]
1041    E6 DA       INC $DA         ;   _DA++
1043    A5 DA       LDA $DA         ;
1045    C9 0F       CMP #$0F        ;
1047    90 EF       BCC $1038       ; } while _DA < 0x0F

1049    60          RTS             ;

;; display player name?

104A    20 02 10    JSR $1002       ; set $FE/$FF vector based on $D4
104D    A9 00       LDA #$00        ;
104F    85 DA       STA $DA         ; _DA = 0x00

1051    A4 DA       LDY $DA         ; do {
1053    B1 FE       LDA ($FE),Y     ;
1055    F0 0B       BEQ $1062       ;   if _FE[_DA] == 0, break
1057    20 A9 0F    JSR $0FA9       ;   print character in _FE[_DA]
105A    E6 DA       INC $DA         ;   _DA++
105C    A5 DA       LDA $DA         ;
105E    C9 08       CMP #$08        ;
1060    90 EF       BCC $1051       ; } while _DA < 0x08

1062    60          RTS             ;

;; set text col, row to X, Y then output cursor...

1063    86 CE       STX $CE         ; text col
1065    84 CF       STY $CF         ; text row

;; rotate cursor, print it, and decrement text column

1067    CE 7B 10    DEC $107B       ;
106A    AD 7B 10    LDA $107B       ;
106D    29 7F       AND #$7F        ;
106F    09 7C       ORA #$7C        ;
1071    D0 02       BNE $1075       ;

;; print space and decrement text column

1073    A9 20       LDA #$20        ; set character to space

1075    20 A9 0F    JSR $0FA9       ; output character
1078    C6 CE       DEC $CE         ; decrement text col
107A    60          RTS             ;

;; DATA: cursor state?

107B  00

;; output a byte as two digits (assuming each nibble is < 10)

107C    85 DA       STA $DA         ; store accumulator in _DA
107E    20 01 15    JSR $1501       ; shift acc 4 bits to right (i.e. // 16)
1081    18          CLC             ;
1082    69 30       ADC #$30        ; add #$30 to convert digit to character code
1084    20 A9 0F    JSR $0FA9       ; print that character code
1087    A5 DA       LDA $DA         ; load back original accumulator
1089    29 0F       AND #$0F        ; % 16

; display single digit

108B    18          CLC             ;
108C    69 30       ADC #$30        ; add #$30 to convert digit to character code
108E    4C A9 0F    JMP $0FA9       ; print that character code

;;

1091    20 20 16    JSR $1620       ; store $CE/$CF in $1677/$1678 and $00 in $1676

1094    18 00 04 05 04 05 04 05 04 05 04 05 04 05 04 05 04 FF

10A6    A2 08       LDX #$08        ; X = range(0x08, 0x48)

10A8    BD 00 E0    LDA $E000,X     ;
10AB    85 FE       STA $FE         ; _FE = E000[X]
10AD    BD C0 E0    LDA $E0C0,X     ;
10B0    85 FF       STA $FF         ; _FF = E0C0[X]

10B2    A0 18       LDY #$18        ; Y = range(0x18, 0x27)
10B4    A9 80       LDA #$80        ;

10B6    91 FE       STA ($FE),Y     ; do { store 0x80 in index 0x18 of vector at $FE/$FF
10B8    C8          INY             ;   Y++
10B9    C0 27       CPY #$27        ;
10BB    90 F9       BCC $10B6       ; } while Y < 0x27

10BD    E8          INX             ; X++
10BE    E0 48       CPX #$48        ;
10C0    90 E6       BCC $10A8       ; loop while X < 0x48

10C2    60          RTS             ;

;; DISPLAY STATS

10C3    A9 E4       LDA #$E4        ; set up $FE/$FF vector as $E400
10C5    85 FF       STA $FF         ; .
10C7    A9 00       LDA #$00        ; .
10C9    85 FE       STA $FE         ; .
10CB    A2 00       LDX #$00        ; X = 0

10CD    BD 00 ED    LDA $ED00,X     ; loops over X = [0, 7]
10D0    F0 04       BEQ $10D6       ; if zero, skip

10D2    A9 00       LDA #$00        ;
10D4    F0 04       BEQ $10DA       ; BRA $10DA

10D6    A0 2F       LDY #$2F        ;
10D8    B1 FE       LDA ($FE),Y     ; A = _FE[0x2F]

10DA    A0 25       LDY #$25        ;
10DC    91 FE       STA ($FE),Y     ;
10DE    18          CLC             ; .
10DF    A5 FE       LDA $FE         ; .
10E1    69 80       ADC #$80        ; .
10E3    85 FE       STA $FE         ; _FE += 0x08
10E5    90 02       BCC $10E9       ; .

10E7    E6 FF       INC $FF         ; carry over to _FF if necessary

10E9    E8          INX             ; X++
10EA    E0 08       CPX #$08        ; if X != 0x08
10EC    D0 DF       BNE $10CD       ;   loop back

10EE    20 63 12    JSR $1263       ; persist col, row to non-zeropage
10F1    A5 D4       LDA $D4         ;
10F3    48          PHA             ; store value of $D4
10F4    A5 0F       LDA $0F         ; load party size
10F6    85 D4       STA $D4         ; and put it in $D4

; display stats for character in slot $D4

10F8    A6 D4       LDX $D4         ;
10FA    20 02 10    JSR $1002       ; set $FE/$FF vector based on $D4
10FD    A5 D4       LDA $D4         ;
10FF    85 CF       STA $CF         ; set text row to be player number
1101    A9 18       LDA #$18        ;
1103    85 CE       STA $CE         ; text col = 0x18
1105    A5 D4       LDA $D4         ;
1107    20 8B 10    JSR $108B       ; display single digit (player number)
110A    A9 AD       LDA #$AD        ;
110C    20 A9 0F    JSR $0FA9       ; output "-"?
110F    20 4A 10    JSR $104A       ; display player name?
1112    A9 23       LDA #$23        ;
1114    85 CE       STA $CE         ; text col = 0x23 (player health column)
1116    20 02 10    JSR $1002       ; set $FE/$FF vector based on $D4
1119    A0 18       LDY #$18        ;
111B    B1 FE       LDA ($FE),Y     ; _FE[0x18] (most-significant digit of player health)
111D    20 8B 10    JSR $108B       ; display single digit
1120    A0 19       LDY #$19        ;
1122    B1 FE       LDA ($FE),Y     ; _FE[0x19] (lower two digits of player heath)
1124    20 7C 10    JSR $107C       ; output a byte as two digits
1127    A0 12       LDY #$12        ;
1129    B1 FE       LDA ($FE),Y     ; _FE[0x12] (player status)
112B    20 A9 0F    JSR $0FA9       ; output character
112E    20 0C 15    JSR $150C       ;
1131    C6 D4       DEC $D4         ; move on to next character down
1133    D0 C3       BNE $10F8       ; if not finished, loop back

;; print food / avatar status / gold / ship strength

1135    A2 18       LDX #$18        ; X = 0x18 (text column)
1137    A0 0A       LDY #$0A        ; Y = 0x0A (text row)
1139    20 81 0F    JSR $0F81       ; set text col, row to X, Y then print:

113C    C6 BA 00                                      F:^G

113F    A0 10       LDY #$10        ;
1141    B9 00 ED    LDA $ED00,Y     ; first two digits of food = _ED00[0x10]
1144    20 7C 10    JSR $107C       ; output a byte as two digits
1147    A0 11       LDY #$11        ;
1149    B9 00 ED    LDA $ED00,Y     ; next two digits of good = _ED00[0x11]
114C    20 7C 10    JSR $107C       ; output a byte as two digits
114F    A9 A0       LDA #$A0        ;
1151    20 A9 0F    JSR $0FA9       ; print space
1154    A5 C6       LDA $C6         ; avatar status character?
1156    20 A9 0F    JSR $0FA9       ; print _C6

1159    A5 0E       LDA $0E         ;
115B    C9 14       CMP #$14        ;
115D    90 1A       BCC $1179       ; if _0E < 0x14, show ship strength, else:
115F    20 85 0F    JSR $0F85       ; print (in-game graphics):

1162    A0 C7 BA 00                                    G:^@

1166    A0 13       LDY #$13        ;
1168    B9 00 ED    LDA $ED00,Y     ; first two digits of gold = _ED00[0x13]
116B    20 7C 10    JSR $107C       ; output a byte as two digits
116E    A0 14       LDY #$14        ;
1170    B9 00 ED    LDA $ED00,Y     ; next two digits of gold = _ED00[0x14]
1173    20 7C 10    JSR $107C       ; output a byte as two digits
1176    4C 87 11    JMP $1187       ; skip ship strength

;; display ship strength

1179    20 85 0F    JSR $0F85       ; print (in-game graphics):

117C    A0 D3 C8 D0 BA 00                                 SHP:^@

1182    A5 1B       LDA $1B         ; load ship strength
1184    20 7C 10    JSR $107C       ; output a byte as two digits
1187    68          PLA             ; recover original value of $D4
1188    85 D4       STA $D4         ;
118A    4C 6E 12    JMP $126E       ; restore $CE/$CF from $118D/$118E then return

;; DATA (used to temporarily persist cursor when we need to write elsewhere)

118D    00 00

;; randomly change wind direction

118F    20 4A 15    JSR $154A       ; generate random byte
1192    D0 0D       BNE $11A1       ; 1 in 256 chance of changing wind

1194    20 4A 15    JSR $154A       ; generate a random byte for wind direction
1197    20 57 12    JSR $1257       ; convert A to sign(A)
119A    18          CLC             ; change wind by that sign
119B    65 F5       ADC $F5         ; .
119D    29 03       AND #$03        ; .
119F    85 F5       STA $F5         ; .

11A1    20 63 12    JSR $1263       ; persist col, row to non-zeropage
11A4    A9 17       LDA #$17        ; set text row to #$17
11A6    85 CF       STA $CF         ; .
11A8    A9 06       LDA #$06        ; set $CE to #$06
11AA    85 CE       STA $CE         ;
11AC    20 85 0F    JSR $0F85       ; print (in-game graphics)

; #$1E is > part of border
11AF    1E D7 C9 CE C4 A0 00                            .WIND ^@

11B6    A6 F5       LDX $F5         ;
11B8    F0 0B       BEQ $11C5       ; if $F5 == 0, go to $11C5
11BA    CA          DEX             ;
11BB    F0 0E       BEQ $11CB       ; if $F5 == 1, go to $11CB
11BD    CA          DEX             ;
11BE    F0 11       BEQ $11D1       ; if $F5 == 2, go to $11D1
11C0    CA          DEX             ;
11C1    F0 14       BEQ $11D7       ; if $F5 == 3, go to $11D7
11C3    D0 15       BNE $11DA       ;

; if $F5 == 0

11C5    20 4D 12    JSR $124D       ; print ' WEST'
11C8    4C DA 11    JMP $11DA       ;

; if $F5 == 1

11CB    20 2F 12    JSR $122F       ; print 'NORTH'
11CE    4C DA 11    JMP $11DA       ;

; if $F5 == 2

11D1    20 43 12    JSR $1243       ; print ' EAST'
11D4    4C DA 11    JMP $11DA       ;

; if $F5 == 3

11D7    20 39 12    JSR $1239       ; print 'SOUTH'

; in all cases...

11DA    A9 1D       LDA #$1D        ; < part of border
11DC    20 A9 0F    JSR $0FA9       ; display it
11DF    4C 6E 12    JMP $126E       ; restore $CE/$CF from $118D/$118E (and return)



;; in dungeon, we display what direction we're facing (stored in _10)

11E2    20 63 12    JSR $1263       ; persist $CE/$CF into $118D/$118E
11E5    A9 17       LDA #$17        ; set $CF to 0x17
11E7    85 CF       STA $CF         ; .
11E9    A9 07       LDA #$07        ; set $CE to 0x07
11EB    85 CE       STA $CE         ; .

11ED    20 85 0F    JSR $0F85       ; print

11F0    C4 C9 D2 BA A0 00                 DIR: ^@

11F6    A6 10       LDX $10         ; if _10 == 0
11F8    F0 08       BEQ $1202       ;   go print NORTH
11FA    CA          DEX             ; if _10 == 1
11FB    F0 0B       BEQ $1208       ;   go print  EAST
11FD    CA          DEX             ; if _01 == 2
11FE    F0 0E       BEQ $120E       ;   go print SOUTH
1200    D0 12       BNE $1214       ; otherwise, go print  WEST

1202    20 2F 12    JSR $122F       ; print "NORTH" (in-game graphics)
1205    4C 17 12    JMP $1217       ;

1208    20 43 12    JSR $1243       ; print " EAST" (in-game graphics)
120B    4C 17 12    JMP $1217       ;

120E    20 39 12    JSR $1239       ; print "SOUTH" (in-game graphics)
1211    4C 17 12    JMP $1217       ;

1214    20 4D 12    JSR $124D       ; print " WEST" (in-game graphics)

;; next we display what dungeon level we're on

1217    A9 00       LDA #$00        ; set _CF == 0x00
1219    85 CF       STA $CF         ; .
121B    A9 0B       LDA #$0B        ; set _CE == 0x0B
121D    85 CE       STA $CE         ; .
121F    A9 CC       LDA #$CC        ; 'L'
1221    20 A9 0F    JSR $0FA9       ; display it
1224    A5 0C       LDA $0C         ; A = _0C
1226    18          CLC             ;
1227    69 01       ADC #$01        ; A += 1
1229    20 8B 10    JSR $108B       ; display single digit
122C    4C 6E 12    JMP $126E       ; restore $CE/$CF from $118D/$118E and then return

;; print "NORTH" (in-game graphics)

122F    20 85 0F    JSR $0F85       ;

1232    CE CF D2 D4 C8 00                             NORTH^@

1238    60          RTS             ;

;; print "SOUTH" (in-game graphics)

1239    20 85 0F    JSR $0F85       ;

123C    D3 CF D5 D4 C8 00                             SOUTH^G

1242    60          RTS             ;

;; print " EAST" (in-game graphics)

1243    20 85 0F    JSR $0F85       ;

1246    A0 C5 C1 D3 D4 00                              EAST^@

124C    60          RTS             ;

;; print " WEST" (in-game graphics)

124D    20 85 0F    JSR $0F85       ;

1250    A0 D7 C5 D3 D4 00                              WEST^@

1256    60          RTS             ;

;; convert A to its sign: #$00, #$01, or #$FF

1257    C9 00       CMP #$00        ; if A == 0
1259    F0 07       BEQ $1262       ; return (with #$00 in A)
125B    30 03       BMI $1260       ;
125D    A9 01       LDA #$01        ; if A > 0, return #$01 in A
125F    60          RTS             ;

1260    A9 FF       LDA #$FF        ; else return #$FF in A
1262    60          RTS             ;

;; persist $CE/$CF into $118D/$118E

1263    A5 CE       LDA $CE         ;
1265    8D 8D 11    STA $118D       ;
1268    A5 CF       LDA $CF         ; text row
126A    8D 8E 11    STA $118E       ;
126D    60          RTS             ;

;; restore $CE/$CF from $118D/$118E

126E    AD 8D 11    LDA $118D       ;
1271    85 CE       STA $CE         ;
1273    AD 8E 11    LDA $118E       ;
1276    85 CF       STA $CF         ; text row
1278    60          RTS             ;

;;

1279    C6 CE
127B  20 B5 0A
127E  38 E9 B0 C9 0A B0 F6 85 D7 85 D6 20 8B 10
128C  20 B5 0A C9 8D F0 36 C9 88 F0 E2 38 E9 B0 C9 0A
129C  B0 EE 85 D6 20 8B 10 20 B5 0A C9 8D F0 08 C9 88
12AC  D0 F5 C6 CE 10 DA A5 D7 20 07 15 05 D6 85 D7 A2
12BC  00 F8 38 E9 01 90 03 E8 D0 F9 86 D6 D8
12C9  60            RTS

;; get player number as input

12CA    20 B5 0A    JSR $0AB5       ; get input?
12CD    F0 09       BEQ $12D8       ;

12CF    38          SEC             ; .
12D0    E9 B0       SBC #$B0        ; subtrack $B0 (so '1' -> 0x01 and so on)
12D2    C9 09       CMP #$09        ; if < 0x09
12D4    90 02       BCC $12D8       ;     skip
12D6    A9 00       LDA #$00        ; otherwise set to 0x00

12D8    85 D4       STA $D4         ; store chosen player number in _D4
12DA    20 8B 10    JSR $108B       ; display single digit (player number)
12DD    20 C0 0F    JSR $0FC0       ; scroll/newline
12E0    A5 D4       LDA $D4         ; load _D4 back into accumlator
12E2    60          RTS             ; and return

;;
; walk backwards from the end of the DATA @12F1. If we hit a value that == A, we
; return with A = that value. If we get to the start of our DATA without a
; match, return with A = 0xFF

12E3    A2 28       LDX #$28        ; X = 0x28
12E5    CA          DEX             ; X--
12E6    30 06       BMI $12EE       ; break on minus
12E8    DD F1 12    CMP $12F1,X     ; compare A with _12F1[X]
12EB    D0 F8       BNE $12E5       ; if not equal, loop back to $12E5
12ED    60          RTS             ; if equal, return

12EE    A9 FF       LDA #$FF        ; .
12F0    60          RTS             ; return 0xFF

;; DATA
; possible the terrain types that can be traversed

12F1    03 04 05 06 07 09 0A 0B 0C 10 11 12 13 14 15 16
1301    17 18 19 1A 1B 1C 1D 1E 3C 3E 3F 43 44 46 47 49
1311    4A 4C 4C 4C 8C 8D 8E 8F

;; set the hires page one to all 0x80

1319    A9 20       LDA #$20        ; put $2000 in $FE-$FF
131B    85 FF       STA $FF         ; .
131D    A0 00       LDY #$00        ; .
131F    84 FE       STY $FE         ; .
1321    A9 80       LDA #$80        ; A = 0x80

1323    91 FE       STA ($FE),Y     ; store 0x80 in XX00-XXFF where XX ranged from $20 to $3F
1325    C8          INY             ; Y++
1326    D0 FB       BNE $1323       ;

1328    E6 FF       INC $FF         ;
132A    A6 FF       LDX $FF         ;
132C    E0 40       CPX #$40        ;
132E    90 F3       BCC $1323       ; if X < $40, reppet

1330    60          RTS             ;

;; clear screen?
; X goes from 0x08 to 0xB8
; Y goes from 0x16 to 0x00
; use X on E000 and E0C0 to get base address
; use Y on base address

1331    A2 08       LDX #$08        ; X = 0x08

1333    BD 00 E0    LDA $E000,X     ; _FE = E000 + X
1336    85 FE       STA $FE         ; .
1338    BD C0 E0    LDA $E0C0,X     ; .
133B    85 FF       STA $FF         ; _FF = E0C0 + X

133D    A0 16       LDY #$16        ; Y = 0x16

133F    A9 00       LDA #$00        ; A = 0x00
1341    91 FE       STA ($FE),Y     ; write 0x00 to
1343    88          DEY             ; Y--
1344    D0 FB       BNE $1341       ; loop back

1346    E8          INX             ; X++
1347    E0 B8       CPX #$B8        ; if X < 0xB8
1349    90 E8       BCC $1333       ;

134B    60          RTS             ;

;; invert the hires screen

134C    A2 08       LDX #$08        ;

134E    BD 00 E0    LDA $E000,X     ;
1351    85 FE       STA $FE         ;
1353    BD C0 E0    LDA $E0C0,X     ;
1356    85 FF       STA $FF         ;

1358    A0 16       LDY #$16        ;

135A    B1 FE       LDA ($FE),Y     ;
135C    49 FF       EOR #$FF        ;
135E    91 FE       STA ($FE),Y     ;
1360    88          DEY             ;
1361    D0 F7       BNE $135A       ;

1363    E8          INX             ;
1364    E0 B8       CPX #$B8        ;
1366    90 E6       BCC $134E       ;

1368    60          RTS             ;

;;

1369    85 DA       STA $DA         ;
136B    A9 00       LDA #$00        ;
136D    E0 00       CPX #$00        ;
136F    F0 06       BEQ ---         ;
1371    18          CLC             ;
1372    65 DA       ADC $DA         ;
1374    CA          DEX             ;
1375    D0 FA       BNE ---         ;
1377    60          RTS             ;

1378    A5 03       LDA $03         ;
137A    20 07 15    JSR $1507       ;
137D    85 FC       STA $FC         ;
137F    A5 02       LDA $02         ;
1381    29 0F       AND #$0F        ;
1383    05 FC       ORA $FC         ;
1385    85 FC       STA $FC         ;
1387    A5 03       LDA $03         ;
1389    29 10       AND #$10        ;
138B    0A          ASL A           ;
138C    05 02       ORA $02         ;
138E    20 01 15    JSR $1501       ;
1391    18          CLC             ;
1392    69 E8       ADC #$E8        ;
1394    85 FD       STA $FD         ;

1396    60          RTS             ;

;;

1397    A9 00       LDA #$00        ;
1399    20 AB 13    JSR $13AB       ; call $13AB with A = 0x00
139C    A9 01       LDA #$01        ;
139E    20 AB 13    JSR $13AB       ; call $13AB with A = 0x01
13A1    A9 02       LDA #$02        ;
13A3    20 AB 13    JSR $13AB       ; call $13AB with A = 0x02
13A6    A9 4C       LDA #$4C        ;
13A8    4C AB 13    JMP $13AB       ; call $13AB with A = 0x4C (and return)

;; D000 in two different banks contains the shape files

13AB    A8          TAY             ; Y = A
13AC    2C 83 C0    BIT $C083       ; RD LC RAM bank2, WR-enable LC RAM
13AF    20 B5 13    JSR $13B5       ;
13B2    2C 8B C0    BIT $C08B       ; RD LC RAM bank1, WR-enable LC RAM

; cycle shapes (perhaps this is what animates tiles? although it goes through 16 states)

13B5    B9 00 DF    LDA $DF00,Y     ;
13B8    48          PHA             ; temp = DF00[Y]
13B9    B9 00 DE    LDA $DE00,Y     ;
13BC    99 00 DF    STA $DF00,Y     ; DF00[Y] = DE00[Y]
13BF    B9 00 DD    LDA $DD00,Y     ;
13C2    99 00 DE    STA $DE00,Y     ; DE00[Y] = DD00[Y]
13C5    B9 00 DC    LDA $DC00,Y     ;
13C8    99 00 DD    STA $DD00,Y     ; DD00[Y] = DC00[Y]
13CB    B9 00 DB    LDA $DB00,Y     ;
13CE    99 00 DC    STA $DC00,Y     ; DC00[Y] = DB00[Y]
13D1    B9 00 DA    LDA $DA00,Y     ;
13D4    99 00 DB    STA $DB00,Y     ; DB00[Y] = DA00[Y]
13D7    B9 00 D9    LDA $D900,Y     ;
13DA    99 00 DA    STA $DA00,Y     ; DA00[Y] = D900[Y]
13DD    B9 00 D8    LDA $D800,Y     ;
13E0    99 00 D9    STA $D900,Y     ; D900[Y] = D800[Y]
13E3    B9 00 D7    LDA $D700,Y     ;
13E6    99 00 D8    STA $D800,Y     ; D800[Y] = D700[Y]
13E9    B9 00 D6    LDA $D600,Y     ;
13EC    99 00 D7    STA $D700,Y     ; D700[Y] = D600[Y]
13EF    B9 00 D5    LDA $D500,Y     ;
13F2    99 00 D6    STA $D600,Y     ; D600[Y] = D500[Y]
13F5    B9 00 D4    LDA $D400,Y     ;
13F8    99 00 D5    STA $D500,Y     ; D500[Y] = D400[Y]
13FB    B9 00 D3    LDA $D300,Y     ;
13FE    99 00 D4    STA $D400,Y     ; D400[Y] = D300[Y]
1401    B9 00 D2    LDA $D200,Y     ;
1404    99 00 D3    STA $D300,Y     ; D300[Y] = D200[Y]
1407    B9 00 D1    LDA $D100,Y     ;
140A    99 00 D2    STA $D200,Y     ; D200[Y] = D100[Y]
140D    B9 00 D0    LDA $D000,Y     ;
1410    99 00 D1    STA $D100,Y     ; D100[Y] = D000[Y]
1413    68          PLA             ;
1414    99 00 D0    STA $D000,Y     ; D000[Y] = temp (original DF00[Y])
1417    60          RTS             ;

;;

1418    A2 D0       LDX #$D0        ;
141A    A9 00       LDA #$00        ;
141C    85 FE       STA $FE         ; $FE == 0x00

141E    86 FF       STX $FF         ; $FF == 0xD0 ($FE/$FF = $D000)
1420    20 7B 15    JSR $157B       ; some random number generator?
1423    29 55       AND #$55        ;
1425    2C 8B C0    BIT $C08B       ; RD LC RAM bank1, WR-enable LC RAM
1428    A0 44       LDY #$44        ;
142A    91 FE       STA ($FE),Y     ; initially _D000[0x44] = ...
142C    09 80       ORA #$80        ;
142E    A0 46       LDY #$46        ;
1430    91 FE       STA ($FE),Y     ; initially _D000[0x46] = ...
1432    48          PHA             ;
1433    A0 4A       LDY #$4A        ;
1435    31 FE       AND ($FE),Y     ;
1437    A0 4B       LDY #$4B        ;
1439    51 FE       EOR ($FE),Y     ;
143B    09 80       ORA #$80        ;
143D    91 FE       STA ($FE),Y     ; initially _D000[]
143F    68          PLA             ;
1440    2C 83 C0    BIT $C083       ; RD LC RAM bank2, WR-enable LC RAM
1443    A0 45       LDY #$45        ;
1445    91 FE       STA ($FE),Y     ;
1447    29 7F       AND #$7F        ;
1449    A0 47       LDY #$47        ;
144B    91 FE       STA ($FE),Y     ;
144D    20 7B 15    JSR $157B       ; some seeding of the random number generator??
1450    29 2A       AND #$2A        ;
1452    A0 44       LDY #$44        ;
1454    91 FE       STA ($FE),Y     ;
1456    09 80       ORA #$80        ;
1458    A0 46       LDY #$46        ;
145A    91 FE       STA ($FE),Y     ;
145C    48          PHA             ;
145D    A0 4A       LDY #$4A        ;
145F    31 FE       AND ($FE),Y     ;
1461    A0 4B       LDY #$4B        ;
1463    51 FE       EOR ($FE),Y     ;
1465    09 80       ORA #$80        ;
1467    91 FE       STA ($FE),Y     ;
1469    68          PLA             ;
146A    2C 8B C0    BIT $C08B       ; RD LC RAM bank1, WR-enable LC RAM
146D    A0 45       LDY #$45        ;
146F    91 FE       STA ($FE),Y     ;
1471    29 7F       AND #$7F        ;
1473    A0 47       LDY #$47        ;
1475    91 FE       STA ($FE),Y     ;
1477    E8          INX             ;
1478    E0 E0       CPX #$E0        ;
147A    90 A2       BCC $141E       ;
147C    60          RTS             ;

;;

147D    20 7B 15    JSR $157B       ; some seeding of the random number generator??
1480    30 0F       BMI $1491       ; 50% change (?) of branching to $1491
1482    2C 8B C0    BIT $C08B       ;
1485    AE 0A D3    LDX $D30A       ;
1488    AC 0A D4    LDY $D40A       ;
148B    8C 0A D3    STY $D30A       ;
148E    8E 0A D4    STX $D40A       ;

1491    20 7B 15    JSR $157B       ; some seeding of the random number generator??
1494    30 0F       BMI $14A5       ;
1496    2C 83 C0    BIT $C083       ;
1499    AE 0B D1    LDX $D10B       ;
149C    AC 0B D2    LDY $D20B       ;
149F    8C 0B D1    STY $D10B       ;
14A2    8E 0B D2    STX $D20B       ;

14A5    20 7B 15    JSR $157B       ; some seeding of the random number generator??
14A8    30 1E       BMI $14C8       ;
14AA    2C 8B C0    BIT $C08B       ;
14AD    AE 12 D2    LDX $D212       ;
14B0    AC 12 D3    LDY $D312       ;
14B3    8C 12 D2    STY $D212       ;
14B6    8E 12 D3    STX $D312       ;
14B9    2C 83 C0    BIT $C083       ;
14BC    AE 12 D2    LDX $D212       ;
14BF    AC 12 D3    LDY $D312       ;
14C2    8C 12 D2    STY $D212       ;
14C5    8E 12 D3    STX $D312       ;

14C8    20 7B 15    JSR $157B       ; some seeding of the random number generator??
14CB    30 1E       BMI $14EB       ;
14CD    2C 8B C0    BIT $C08B       ;
14D0    AE 10 D2    LDX $D210       ;
14D3    AC 10 D3    LDY $D310       ;
14D6    8C 10 D2    STY $D210       ;
14D9    8E 10 D3    STX $D310       ;
14DC    2C 83 C0    BIT $C083       ;
14DF    AE 10 D2    LDX $D210       ;
14E2    AC 10 D3    LDY $D310       ;
14E5    8C 10 D2    STY $D210       ;
14E8    8E 10 D3    STX $D310       ;

14EB    20 7B 15    JSR $157B       ; some seeding of the random number generator??
14EE    30 0F       BMI $14FF       ;
14F0    2C 8B C0    BIT $C08B       ;
14F3    AE 0E D1    LDX $D10E       ;
14F6    AC 0E D2    LDY $D20E       ;
14F9    8C 0E D1    STY $D10E       ;
14FC    8E 0E D2    STX $D20E       ;

14FF    60          RTS             ;

;;

1500  4A            LSR A

;; divide by 16

1501  4A            LSR A
1502  4A            LSR A
1503  4A            LSR A
1504  4A            LSR A
1505  60            RTS

;; multiply by 32

1506  0A            ASL A

1507  0A            ASL A
1508  0A            ASL A
1509  0A            ASL A
150A  0A            ASL A
150B  60            RTS

;;  process any keypress onto $B0 buffer
; space resets buffer

150C    AD 00 C0    LDA $C000       ; if no key press,
150F    10 1C       BPL $152D       ;   return
1511    C9 A0       CMP #$A0        ; if not [SPACE],
1513    D0 04       BNE $1519       ;   skip

1515    A0 00       LDY #$00        ; if [SPACE],
1517    84 B8       STY $B8         ;   reset index to 0

1519    C9 FF       CMP #$FF        ;
151B    D0 02       BNE $151F       ;

151D    A9 88       LDA #$88        ; ^H

151F    A4 B8       LDY $B8         ; Y = index into _B0
1521    C0 08       CPY #$08        ; if Y >= #$08,
1523    B0 08       BCS $152D       ;     return

1525    99 B0 00    STA $00B0,Y     ; store key pressed in _B0[Y]
1528    E6 B8       INC $B8         ; increment index
152A    2C 10 C0    BIT $C010       ; unlatch key

152D    60          RTS             ;

;;
; basically doing a for Y in range [0, _B8]: _B0[Y] = _B1[Y]

152E    A5 B8       LDA $B8         ; only continue if $B8 isn't zero
1530    F0 17       BEQ $1549       ; .
1532    A5 B0       LDA $B0         ; load $B0 and push to stack
1534    48          PHA             ; .
1535    C6 B8       DEC $B8         ; decrement $B8
1537    F0 0D       BEQ $1546       ; if it's now zero, branch and finish up

1539    A0 00       LDY #$00        ;

153B    B9 B1 00    LDA $00B1,Y     ; copy _B1[Y]
153E    99 B0 00    STA $00B0,Y     ; to _B0[Y]
1541    C8          INY             ; from Y running until _B8
1542    C4 B8       CPY $B8         ;
1544    90 F5       BCC $153B       ;

1546    68          PLA             ; pull original $B0 from stack
1547    C9 00       CMP #$00        ; why?
1549    60          RTS             ;

;; calculates some kind of (random?) number and puts it in ACCUMULATOR
; keeps X intact
; calculates A based on data that follows

154A    8A          TXA             ;
154B    48          PHA             ;
154C    18          CLC             ;
154D    A2 0E       LDX #$0E        ;
154F    AD 7A 15    LDA $157A       ;

1552    7D 6B 15    ADC $156B,X     ;
1555    9D 6B 15    STA $156B,X     ;
1558    CA          DEX             ;
1559    10 F7       BPL $1552       ;
155B    A2 0F       LDX #$0F        ;

155D    FE 6B 15    INC $156B,X     ;
1560    D0 03       BNE $1565       ;
1562    CA          DEX             ;
1563    10 F8       BPL $155D       ;
1565    68          PLA             ;
1566    AA          TAX             ;
1567    AD 6B 15    LDA $156B       ;
156A    60          RTS             ;

;; DATA

156B    64 76 85 54 F6 5C 76 1F E7 12 A7 6B 93 C4 6E
157A    1B

;; some seeding of the random number generator??

157B    AD 6E 15    LDA $156E       ;
157E    6D 6D 15    ADC $156D       ;
1581    8D 6D 15    STA $156D       ;
1584    4D 6C 15    EOR $156C       ;
1587    8D 6C 15    STA $156C       ;
158A    6D 6B 15    ADC $156B       ;
158D    8D 6B 15    STA $156B       ;
1590    8D 6E 15    STA $156E       ;
1593    60          RTS             ;

;;
; for X in [0x60, 0xB8]:
;   for Y in [0x18, 0x28]:

1594    A2 60       LDX #$60        ; X = #$60 (96)
1596    A0 18       LDY #$18        ; Y = #$18 (24)
1598    BD 00 E0    LDA $E000,X     ; $E060 = $28
159B    8D B4 15    STA $15B4       ;
159E    BD C0 E0    LDA $E0C0,X     ; $E120 = $22
15A1    8D B5 15    STA $15B5       ;
15A4    BD 08 E0    LDA $E008,X     ; $E068 = $A8
15A7    8D B1 15    STA $15B1       ;
15AA    BD C8 E0    LDA $E0C8,X     ; $E128 = $22
15AD    8D B2 15    STA $15B2       ;

; copy
15B0    B9 FF FF    LDA $____,Y     ; $22A8 + $18 = $22C0
15B3    99 FF FF    STA $____,Y     ; $2228 + $18 = $2240

15B6    C8          INY             ; increment Y
15B7    C0 28       CPY #$28        ; loop if Y < #$28
15B9    90 F5       BCC $15B0       ; .
15BB    E8          INX             ; increment X
15BC    E0 B8       CPX #$B8        ; loop if X < #$B8
15BE    90 D6       BCC $1596       ; .
15C0    20 20 16    JSR $1620       ; store $CE/$CF in $1677/$1678 and $00 in $1676

15C3    18
15C4  17 20 20 20 20 20 20 20
15CC  20 20 20 20 20 20 20 20 20 FF 60

;; display character given in accumulator

; reads: A, $CE, $CF, TBLS, HTXT,
; changes: A, X, Y, $BD, $BE, $BF

; throughout, Y seems to hold the text column


15D7    85 BD       STA $BD         ; store the character to display in $BD
15D9    A5 CF       LDA $CF         ; take $CF (text row)
15DB    0A          ASL A           ; shift
15DC    0A          ASL A           ; shift
15DD    0A          ASL A           ; shift ==  x 8
15DE    85 BE       STA $BE         ; store the row x 8 in $BE
15E0    A9 E4       LDA #$E4        ; set page to read to $E4
15E2    8D FD 15    STA $15FD       ; .
15E5    A9 08       LDA #$08        ; store #$08
15E7    85 BF       STA $BF         ; in $BF
15E9    A4 CE       LDY $CE         ; Y = $CE (text col)

15EB    A6 BE       LDX $BE         ; X = row x 8
15ED    BD 00 E0    LDA $E000,X     ;
15F0    8D FF 15    STA $15FF       ; high byte of where to write
15F3    BD C0 E0    LDA $E0C0,X     ;
15F6    8D 00 16    STA $1600       ; low byte of where to write
15F9    A6 BD       LDX $BD         ; index is the character to write
15FB    BD 00 FF    LDA $____,X     ; (set initially to $E400)
15FE    99 FF FF    STA $____,Y     ;
1601    18          CLC             ;
1602    AD FC 15    LDA $15FC       ; add #$80 to what's in $15FC
1605    69 80       ADC #$80        ; .
1607    8D FC 15    STA $15FC       ; .
160A    90 03       BCC $160F       ; if carry, increment $15FD
160C    EE FD 15    INC $15FD       ; .
160F    E6 BE       INC $BE         ; increment $BE
1611    C6 BF       DEC $BF         ; decrement $BF (started at #$08)
1613    D0 D6       BNE $15EB       ; loop back if $BF not zero yet
1615    A0 00       LDY #$00        ;
1617    60          RTS             ;

;; calls $1625 but with $1676 = #$80

1618    A9 80       LDA #$80        ;
161A    8D 76 16    STA $1676       ; some sort of variable
161D    4C 25 16    JMP $1625       ;

;; calls $1625 but with $1676 = #$00

1620    A9 00       LDA #$00        ;
1622    8D 76 16    STA $1676       ;

;;

; temporarily store $CE/$CF in $1677/$1678

1625    A5 CE       LDA $CE         ; text col
1627    8D 77 16    STA $1677       ;
162A    A5 CF       LDA $CF         ; text row
162C    8D 78 16    STA $1678       ;

; put stack return address in $FC/$FD

162F    68          PLA             ;
1630    85 FC       STA $FC         ;
1632    68          PLA             ;
1633    85 FD       STA $FD         ;

1635    20 6F 16    JSR $166F       ; increment $FC/$FD
1638    A0 00       LDY #$00        ; (Y not really used)
163A    B1 FC       LDA ($FC),Y     ; get the next character
163C    85 CE       STA $CE         ; text col
163E    20 6F 16    JSR $166F       ; increment $FC/$FD
1641    B1 FC       LDA ($FC),Y     ;
1643    85 CF       STA $CF         ; text row

1645    20 6F 16    JSR $166F       ; increment $FC/$FD
1648    B1 FC       LDA ($FC),Y     ;
164A    30 12       BMI $165E       ; branch if high bit set
164C    20 D7 15    JSR $15D7       ; display character given in accumulator
164F    2C 76 16    BIT $1676       ; if $1676 high bit set
1652    30 05       BMI $1659       ;    increment $CF
1654    E6 CE       INC $CE         ; else increment text col
1656    4C 45 16    JMP $1645       ;

1659    E6 CF       INC $CF         ; increment row
165B    4C 45 16    JMP $1645       ;

; put $FC/$FD back as stack return address

165E    A5 FD       LDA $FD         ;
1660    48          PHA             ;
1661    A5 FC       LDA $FC         ;
1663    48          PHA             ;

; restore $CE/$CF from $1677/$1678

1664    AD 77 16    LDA $1677       ;
1667    85 CE       STA $CE         ; text col
1669    AD 78 16    LDA $1678       ;
166C    85 CF       STA $CF         ; text row

166E    60          RTS             ;

;; increment $FC-$FD

166F    E6 FC       INC $FC         ; increment $FC
1671    D0 02       BNE $1675       ; and carry to $FD
1673    E6 FD       INC $FD         ; .
1675    60          RTS             ; .

;; DATA

1676  00 00 00

;;

1679    20 19 13      JSR $1319     ; set the hires page one to all 0x80
167C    20 20 16      JSR $1620     ; store $CE/$CF in $1677/$1678 and $00 in $1676

167F  00 00 10 05 04 05 04 05 04 05 04 05 1E
168C  20 20 1D 04 05 04 05 04 05 04 05 04 01 04 05 04
169C  05 04 05 04 05 04 05 04 05 04 05 04 13 FF 20 18
16AC  16 00 01 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A 0A
16BC  0A 0A 0A 0A 0A 0A 0A 0A 0A 14 FF 20 20 16 01 17
16CC  03 02 03 02 03 02 03 02 03 02 03 02 03 02 03 02
16DC  03 02 03 02 03 02 FF 20 18 16 17 01 0D 0D 0D 0D
16EC  0D 0D 0D 0D 09 0D 09 0D 0D 0D 0D 0D 0D 0D 0D 0D
16FC  0D 0D 0B FF 20 18 16 27 01 09 09 09 09 09 09 09
170C  09 01 09 05 FF 20 20 16 18 09 06 07 06 07 06 07
171C  06 07 06 07 06 07 06 07 06 FF 20 20 16 18 0B 06
172C  07 06 07 06 07 06 07 06 07 06 07 06 07 06 FF 60
173C  00 00 00 00 00 00 00 00 00 04 02 01 01 02 04 00
174C  00 04 06 07 07 06 04 00 00 0C 0E 1F 1F 0E 0C 00
175C  00 0C 1E 3F 3F 1E 0C 00 00 0C 1C 3E 3E 1C 0C 00
176C  00 08 18 38 38 18 08 00 00 08 10 20 20 10 08 00
177C  18 AD 55 18 69 40 8D 55 18 D0 7C EE 56 18 EE 56
178C  18 AD 56 18 29 E0 4A 4A A8 B9 3C 17 8D 0B 20 B9
179C  3D 17 8D 0B 24 B9 3E 17 8D 0B 28 B9 3F 17 8D 0B
17AC  2C B9 40 17 8D 0B 30 B9 41 17 8D 0B 34 B9 42 17
17BC  8D 0B 38 B9 43 17 8D 0B 3C 18 AD 57 18 69 06 8D
17CC  57 18 29 E0 4A 4A A8 B9 3C 17 8D 0C 20 B9 3D 17
17DC  8D 0C 24 B9 3E 17 8D 0C 28 B9 3F 17 8D 0C 2C B9
17EC  40 17 8D 0C 30 B9 41 17 8D 0C 34 B9 42 17 8D 0C
17FC  38 B9 43 17 8D 0C 3C AD 56 18 20 00 15 85 12 AD
180C  57 18 20 00 15 85 13 AD 56 18 29 1F F0 05 C9 1E
181C  F0 0C 60 20 37 18 A5 F1 18 69 40 85 ED 60 20 37
182C  18 A5 F1 49 03 18 69 40 85 ED 60 AD 56 18 29 E0
183C  20 00 15 AA BD 58 18 85 EE BD 60 18 85 EF AD 55
184C  18 29 C0 2A 2A 2A 85 F1 60 00 00 00 E0 60 26 32
185C  A6 68 17 BB 85 66 E0 25 13 C2 7E A7

;; request disk switch
; A = desired disk number
; 1 = ULTIMA
; 2 = BRITANNIA
; 3 = TOWN
; 4 = DUNGEON

1868    85 DE       STA $DE         ; store A in $DE (desired disk number)
186A    A5 DE       LDA $DE         ; A = $DE (desired disk number)
186C    C9 02       CMP #$02        ; if 2 (BRITANNIA)
186E    F0 5F       BEQ $18CF       ;
1870    C9 04       CMP #$04        ; if 4 (DUNGEON)
1872    F0 5B       BEQ $18CF       ;

1874    A9 01       LDA #$01        ; .
1876    85 DF       STA $DF         ; _DF = 0x01
1878    A5 DE       LDA $DE         ; .
187A    C5 D2       CMP $D2         ; if _D2 == _DE (if disk in drive 1 is the desired one)
187C    F0 63       BEQ $18E1       ;

187E    20 85 0F    JSR $0F85       ; PRINT

1881  8D D0 CC C5 C1 D3 C5 A0 D0 CC C1 C3 C5 A0 D4 C8 C5 8D 00     ^MPLEASE PLACE THE^M^@

1894    20 09 19    JSR $1909       ; print disk name based on $DE
1897    20 85 0F    JSR $0F85       ; PRINT

1894  A0 C4 C9 D3 CB 8D C9 CE D4 CF A0 C4 D2 C9 D6 C5 A0 00  DISK^MINTO DRIVE ^@

18AC    A5 DF       LDA $DF         ; get the drive number from $DF
18AE    20 8B 10    JSR $108B       ; print drive number
18B1    20 85 0F    JSR $0F85       ; PRINT

18B4  8D C1 CE C4 A0 D0 D2 C5 D3 D3 A0 DB C5 D3 C3 DD 8D 00 ^MAND PRESS [ESC]^M^@

18C6    20 B5 0A    JSR $0AB5       ;
18C9    C9 9B       CMP #$9B        ;
18CB    D0 F9       BNE $18C6       ;
18CD    F0 12       BEQ $18E1       ;

18CF    A5 D1       LDA $D1         ;
18D1    C9 02       CMP #$02        ;
18D3    90 9F       BCC $1874       ;
18D5    A9 02       LDA #$02        ;
18D7    85 DF       STA $DF         ;
18D9    A5 DE       LDA $DE         ;
18DB    C5 D3       CMP $D3         ;
18DD    F0 02       BEQ $18E1       ;
18DF    D0 9D       BNE $187E       ;

18E1    A5 DF       LDA $DF         ;
18E3    18          CLC             ;
18E4    69 B0       ADC #$B0        ;
18E6    8D F8 18    STA $18F8       ;
18E9    20 D2 0F    JSR $0FD2       ;

18EC  84 C2 CC CF C1 C4 C4 C9 D3 CB AC C4   ^DBLOADDISK,D
18F8  B1                                    1
18F9  8D 00                                 ^M^@

18FB    A6 DF       LDX $DF         ;
18FD    A5 D0       LDA $D0         ;
18FF    95 D1       STA $D1,X       ;
1901    C5 DE       CMP $DE         ;
1903    F0 03       BEQ $1908       ;
1905    4C 6A 18    JMP $186A       ;
1908    60          RTS             ;

;;;
;; print name of disk based on value of $DE
;
; $DE = 2   BRITANNIA
; $DE = 3   TOWNE
; $DE = 4   UNDERWORLD

1909    A6 DE       LDX $DE         ;
190B    CA          DEX             ;
190C    CA          DEX             ;
190D    D0 0E       BNE $191D       ;
190F    20 85 0F    JSR $0F85       ;

1912    C2 D2 C9 D4 C1 CE CE C9 C1 00                 BRITANNIA^@

191C    60          RTS             ;

;;

191D    CA          DEX             ;
191E    D0 0A       BNE $192A       ;
1920    20 85 0F    JSR $0F85       ;

1923    D4 CF D7 CE C5 00                             TOWNE^@

1929    60          RTS             ;

;;

192A    20 85 0F    JSR $0F85       ;

192D  D5 CE C4 C5 D2 D7 CF D2 CC C4 00                UNDERWORLD^@

1938  60            RTS

;;; if _CD != 0, call _1948[A*2] (a particular sound effect?) for duration in X

1939    0A          ASL A           ; A = A * 2
193A    A8          TAY             ; Y = A
193B    A5 CD       LDA $CD         ; A = _CD
193D    F0 08       BEQ $1947       ; return if A == 0
193F    B9 49 19    LDA $1949,Y     ; A = _1949[Y]
1942    48          PHA             ; push to stack
1943    B9 48 19    LDA $1948,Y     ; A = _1948[Y]
1946    48          PHA             ; push to stack
1947    60          RTS             ; return (to what was put on stack)

;; table of addresses used by above

1948    61 19                       ; $1961
194A    75 19                       ; $1975
194C    83 19                       ; $1983
194E    93 19                       ; $1993
1050    A4 19                       ; $19A4
1052    B4 19                       ; $19B4
1054    C4 19                       ; $19C4
1056    D6 19                       ; $19D6
1058    E8 19                       ; $19E8
105A    27 1A                       ; $1A27
195C    F9 19                       ; 0x0A => $19FA
195E    0F 1A                       ; $1A0F
1960    80 1A                       ; $1A80

;;

1962    A0 06       LDY #$06        ;
1964    20 4A 15    JSR $154A       ; random number generator?
1967    29 3F       AND #$3F        ;
1969    09 20       ORA #$20        ;
196B    AA          TAX             ;
196C    CA          DEX             ;
196D    D0 FD       BNE ---         ;
196F    2C 30 C0    BIT $C030       ;
1972    88          DEY             ;
1973    D0 EF       BNE ---         ;
1975    60          RTS             ;

;;

1976    A0 32       LDY #$32        ;
1978    48          PHA             ;
1979    68          PLA             ;
197A    CA          DEX             ;
197B    D0 FB       BNE ---         ;
197D    2C 30 C0    BIT $C030       ;
1980    88          DEY             ;
1981    D0 F5       BNE ---         ;
1983    60          RTS             ;

;;

1984    A0 32       LDY #$32        ;
1986    EA          NOP             ;
1987    EA          NOP             ;
1988    CA          DEX             ;
1989    D0 FB       BNE ---         ;
198B    2C 30 C0    BIT $C030       ;
198E    88          DEY             ;
198F    D0 F5       BNE ---         ;
1991    4C 76 19    JMP $1976       ;
1994    A2 00       LDX #$00        ;
1996    86 DA       STX $DA         ;
1998    E8          INX             ;
1999    D0 FD       BNE ---         ;
199B    2C 30 C0    BIT $C030       ;
199E    C6 DA       DEC $DA         ;
19A0    A6 DA       LDX $DA         ;
19A2    D0 F4       BNE ---         ;
19A4    60          RTS             ;

;;

19A5    A9 FF       LDA #$FF        ;
19A7    AA          TAX             ;
19A8    A8          TAY             ;
19A9    CA          DEX             ;
19AA    D0 FD       BNE ---         ;
19AC    2C 30 C0    BIT $C030       ;
19AF    88          DEY             ;
19B0    98          TYA             ;
19B1    AA          TAX             ;
19B2    30 F5       BMI ---         ;
19B4    60          RTS             ;

;;

19B5    A9 80       LDA #$80        ;
19B7    AA          TAX             ;
19B8    A8          TAY             ;
19B9    CA          DEX             ;
19BA    D0 FD       BNE ---         ;
19BC    2C 30 C0    BIT $C030       ;
19BF    C8          INY             ;
19C0    98          TYA             ;
19C1    AA          TAX             ;
19C2    30 F5       BMI ---         ;
19C4    60          RTS             ;

;;

19C5    A0 FF       LDY #$FF        ;
19C7    20 4A 15    JSR $154A       ; random number generator?
19CA    29 7F       AND #$7F        ;
19CC    AA          TAX             ;
19CD    CA          DEX             ;
19CE    D0 FD       BNE ---         ;
19D0    2C 30 C0    BIT $C030       ;
19D3    88          DEY             ;
19D4    D0 F1       BNE ---         ;
19D6    60          RTS             ;

;;

19D7    A0 FF       LDY #$FF        ;
19D9    20 4A 15    JSR $154A       ; random number generator?
19DC    09 80       ORA #$80        ;
19DE    AA          TAX             ;
19DF    CA          DEX             ;
19E0    D0 FD       BNE ---         ;
19E2    2C 30 C0    BIT $C030       ;
19E5    88          DEY             ;
19E6    D0 F1       BNE ---         ;
19E8    60          RTS             ;

;;

19E9    A2 7F       LDX #$7F        ;
19EB    86 DA       STX $DA         ;
19ED    CA          DEX             ;
19EE    D0 FD       BNE ---         ;
19F0    2C 30 C0    BIT $C030       ;
19F3    C6 DA       DEC $DA         ;
19F5    A6 DA       LDX $DA         ;
19F7    D0 F4       BNE ---         ;
19F9    60          RTS             ;

;;

19FA    86 F0       STX $F0         ;
19FC    20 4A 15    JSR $154A       ; random number generator?
19FF    A2 28       LDX #$28        ;
1A01    A8          TAY             ;
1A02    88          DEY             ;
1A03    D0 FD       BNE ---         ;
1A05    2C 30 C0    BIT $C030       ;
1A08    CA          DEX             ;
1A09    D0 F6       BNE ---         ;
1A0B    C6 F0       DEC $F0         ;
1A0D    D0 ED       BNE ---         ;
1A0F    60          RTS             ;

;;

1A10    A9 40       LDA #$40        ;
1A12    A0 20       LDY #$20        ;
1A14    AA          TAX             ;
1A15    48          PHA             ;
1A16    68          PLA             ;
1A17    CA          DEX             ;
1A18    D0 FB       BNE ---         ;
1A1A    2C 30 C0    BIT $C030       ;
1A1D    88          DEY             ;
1A1E    D0 F4       BNE ---         ;
1A20    18          CLC             ;
1A21    69 01       ADC #$01        ;
1A23    C9 C0       CMP #$C0        ;
1A25    90 EB       BCC ---         ;
1A27    60          RTS             ;

;;

1A28    8E 80 1A    STX $1A80       ;
1A2B    A9 01       LDA #$01        ;
1A2D    8D 7F 1A    STA $1A7F       ;
1A30    A9 30       LDA #$30        ;
1A32    85 F0       STA $F0         ;
1A34    AE 80 1A    LDX $1A80       ;
1A37    CA          DEX             ;
1A38    D0 FD       BNE ---         ;
1A3A    2C 30 C0    BIT $C030       ;
1A3D    AE 7F 1A    LDX $1A7F       ;
1A40    CA          DEX             ;
1A41    D0 FD       BNE ---         ;
1A43    2C 30 C0    BIT $C030       ;
1A46    C6 F0       DEC $F0         ;
1A48    D0 EA       BNE ---         ;
1A4A    CE 80 1A    DEC $1A80       ;
1A4D    EE 7F 1A    INC $1A7F       ;
1A50    AD 7F 1A    LDA $1A7F       ;
1A53    C9 1B       CMP #$1B        ;
1A55    D0 D9       BNE ---         ;
1A57    A9 30       LDA #$30        ;
1A59    85 F0       STA $F0         ;
1A5B    AE 80 1A    LDX $1A80       ;
1A5E    CA          DEX             ;
1A5F    D0 FD       BNE ---         ;
1A61    2C 30 C0    BIT $C030       ;
1A64    AE 7F 1A    LDX $1A7F       ;
1A67    CA          DEX             ;
1A68    D0 FD       BNE ---         ;
1A6A    2C 30 C0    BIT $C030       ;
1A6D    C6 F0       DEC $F0         ;
1A6F    D0 EA       BNE ---         ;
1A71    CE 7F 1A    DEC $1A7F       ;
1A74    EE 80 1A    INC $1A80       ;
1A77    AD 7F 1A    LDA $1A7F       ;
1A7A    C9 00       CMP #$00        ;
1A7C    D0 D9       BNE ---         ;
1A7E    60          RTS             ;

;;

1A7F    00
1A80    00

;;

1A81    A9 C0       LDA #$C0        ;
1A83    A0 20       LDY #$20        ;
1A85    AA          TAX             ;
1A86    48          PHA             ;
1A87    68          PLA             ;
1A88    CA          DEX             ;
1A89    D0 FB       BNE ---         ;
1A8B    2C 30 C0    BIT $C030       ;
1A8E    88          DEY             ;
1A8F    D0 F4       BNE ---         ;
1A91    38          SEC             ;
1A92    E9 01       SBC #$01        ;
1A94    C9 40       CMP #$40        ;
1A96    B0 EB       BCS ---         ;
1A98    60          RTS             ;

;;

1A99    48          PHA             ;
1A9A    A8          TAY             ;
1A9B    A9 09       LDA #$09        ;
1A9D    85 FE       STA $FE         ;
1A9F    A9 1B       LDA #$1B        ;
1AA1    85 FF       STA $FF         ;
1AA3    A2 00       LDX #$00        ;
1AA5    86 D8       STX $D8         ;
1AA7    A1 FE       LDA ($FE,X)     ;
1AA9    10 06       BPL ---         ;
1AAB    20 02 1B    JSR $1B02       ;
1AAE    4C A7 1A    JMP $1AA7       ;
1AB1    88          DEY             ;
1AB2    F0 03       BEQ ---         ;
1AB4    4C AB 1A    JMP $1AAB       ;
1AB7    20 02 1B    JSR $1B02       ;
1ABA    A2 00       LDX #$00        ;
1ABC    A1 FE       LDA ($FE,X)     ;
1ABE    10 05       BPL ---         ;
1AC0    E6 D8       INC $D8         ;
1AC2    4C B7 1A    JMP $1AB7       ;
1AC5    E6 D8       INC $D8         ;
1AC7    A9 0F       LDA #$0F        ;
1AC9    38          SEC             ;
1ACA    E5 D8       SBC $D8         ;
1ACC    4A          LSR A           ;
1ACD    18          CLC             ;
1ACE    65 CE       ADC $CE         ; text col?
1AD0    85 CE       STA $CE         ; text col?
1AD2    68          PLA             ;

1AD3    A8          TAY             ;
1AD4    A9 09       LDA #$09        ; _FE = 0x09
1AD6    85 FE       STA $FE         ; .
1AD8    A9 1B       LDA #$1B        ; _FF = 0x1B
1ADA    85 FF       STA $FF         ; .
1ADC    A2 00       LDX #$00        ;

1ADE    A1 FE       LDA ($FE,X)     ; A = _1B09[X]
1AE0    10 06       BPL $1AE8       ; if highbit zero, break

1AE2    20 02 1B    JSR $1B02       ; inc $FE/$FF
1AE5    4C DE 1A    JMP $1ADE       ; back to

1AE8    88          DEY             ; Y--
1AE9    F0 03       BEQ $1AEE       ; break if zero
1AEB    4C E2 1A    JMP $1AE2       ; otherwise loop back

1AEE    20 02 1B    JSR $1B02       ; inc $FE/$FF
1AF1    A2 00       LDX #$00        ;
1AF3    A1 FE       LDA ($FE,X)     ; A =
1AF5    10 06       BPL $1AFD       ; if highbit zero, break

1AF7    20 A9 0F    JSR $0FA9       ; print character
1AFA    4C EE 1A    JMP $1AEE       ;

1AFD    09 80       ORA #$80        ;
1AFF    4C A9 0F    JMP $0FA9       ;

;; inc $FE/$FF with carry

1B02    E6 FE       INC $FE         ; _FE++
1B04    D0 02       BNE $1B08       ; if wrap
1B06    E6 FF       INC $FF         ;   _F++
1B08    60          RTS             ;

;;

1B09    00

;; note these are no-high-bit-terminated

1B0A    D0 C9 D2 C1 D4 45           PIRATE      ; 00
1B10    D0 C9 D2 C1 D4 45           PIRATE      ; 01
1B16    CE C9 D8 C9 45              NIXIE       ; 02
1B1B    D3 D1 D5 C9 44              SQUID       ; 03
1B20    D3 C5 D2 D0 C5 CE 54        SERPENT     ; 04
1B27    D3 C5 C1 C8 CF D2 D3 45     SEAHORSE    ; 05
1B2F    D7 C8 C9 D2 CC D0 CF CF 4C  WHIRLPOOL   ; 06
1B38    D4 D7 C9 D3 D4 C5 52        TWISTER     ; 07
1B3F    D2 C1 54                    RAT         ; 08
1B42    C2 C1 54                    BAT         ; 09
1B45    D3 D0 C9 C4 C5 52           SPIDER      ; 0A
1B4B    C7 C8 CF D3 54              GHOST       ; 0B
1B50    D3 CC C9 CD 45              SLIME       ; 0C
1B55    D4 D2 CF CC 4C              TROLL       ; 0D
1B5A    C7 D2 C5 CD CC C9 4E        GREMLIN     ; 0E
1B61    CD C9 CD C9 43              MIMIC       ; 0F
1B66    D2 C5 C1 D0 C5 52           REAPER      ; 10
1B6C    C9 CE D3 C5 C3 D4 53        INSECTS     ; 11
1B73    C7 C1 DA C5 52              GAZER       ; 12
1B78    D0 C8 C1 CE D4 CF 4D        PHANTOM     ; 13
1B7F    CF D2 43                    ORC         ; 14
1B82    D3 CB C5 CC C5 D4 CF 4E     SKELETON    ; 15
1B8A    D2 CF C7 D5 45              ROGUE       ; 16
1B8F    D0 D9 D4 C8 CF 4E           PYTHON      ; 17
1B95    C5 D4 D4 C9 4E              ETTIN       ; 18
1B9A    C8 C5 C1 C4 CC C5 D3 53     HEADLESS    ; 19
1BA2    C3 D9 C3 CC CF D0 53        CYCLOPS     ; 1A
1BA9    D7 C9 D3 50                 WISP        ; 1B
1BAD    CD C1 C7 45                 MAGE        ; 1C
1BB1    CC C9 C3 C8 45              LICHE       ; 1D
1BB6    CC C1 D6 C1 A0 CC C9 DA C1 D2 44    LAVA LIZARD   ; 1E
1BC1    DA CF D2 4E                         ZORN          ; 1F
1BC5    C4 C1 C5 CD CF 4E                   DAEMON        ; 20
1BCB    C8 D9 C4 D2 41                      HYDRA         ; 21
1BD0    C4 D2 C1 C7 CF 4E                   DRAGON        ; 22
1BD6    C2 C1 CC D2 CF 4E                   BALRON        ; 23

;; weapons

1BDC    C8 C1 CE C4 53                        HANDS
        D3 D4 C1 C6 46                        STAFF
        C4 C1 C7 C7 C5 52                     DAGGER
1BEC    D3 CC C9 CE 47                        SLING
        CD C1 C3 45                           MACE
        C1 D8 45                              AXE
        D3 D7 CF D2 44                        SWORD
1BFD    C2 CF 57                              BOW
        C3 D2 CF D3 D3 C2 CF 57               CROSSBOW
        C6 CC C1 CD C9 CE C7 A0 CF C9 4C      FLAMING OIL
        C8 C1 CC C2 C5 D2 44                  HALBERD
        CD C1 C7 C9 C3 A0 C1 D8 45            MAGIC AXE
        CD C1 C7 C9 C3 A0 D3 D7 CF D2 44      MAGIC SWORD
        CD C1 C7 C9 C3 A0 C2 CF 57            MAGIC BOW
        CD C1 C7 C9 C3 A0 D7 C1 CE 44         MAGIC WAND
        CD D9 D3 D4 C9 C3 A0 D3 D7 CF D2 44   MYSTIC SWORD

;; armour

        D3 CB C9 4E                           SKIN
        C3 CC CF D4 48                        CLOTH
        CC C5 C1 D4 C8 C5 52                  LEATHER
        C3 C8 C1 C9 CE A0 CD C1 C9 4C         CHAIN MAIL
        D0 CC C1 D4 C5 A0 CD C1 C9 4C         PLATE MAIL
        CD C1 C7 C9 C3 A0 C3 C8 C1 C9 4E      MAGIC CHAIN
1C7C    CD C1 C7 C9 C3 A0 D0 CC C1 D4 45      MAGIC PLATE
        CD D9 D3 D4 C9 C3 A0 D2 CF C2 45      MYSTIC ROBE

1C8C    C8 CE 44                              HND
        D3 D4 46                              STF
        C4 C1 47                              DAG
        D3 CC 4E                              SLN
        CD C1 43                              MAC
        C1 D8 45                              AXE
        D3 D7 44                              SWD
        C2 CF 57                              BOW
        D8 C2 4F                              XBO
        CF C9 4C                              OIL
        C8 C1 4C                              HAL
        AB C1 58                              +AX
        AB D3 57                              +SW
        AB C2 4F                              +BO
1CBC    D7 CE 44                              WND
        DE D3 57                              ^SW

        CD C1 C7 45                           MAGE
        C2 C1 D2 44                           BARD
        C6 C9 C7 C8 D4 C5 52                  FIGHTER
        C4 D2 D5 C9 44                        DRUID
        D4 C9 CE CB C5 52                     TINKER
1CDC    D0 C1 CC C1 C4 C9 4E                  PALADIN
        D2 C1 CE C7 C5 52                     RANGER
        D3 C8 C5 D0 C8 C5 D2 44               SHEPHERD

        C7 D5 C1 D2 44                        GUARD
        CD C5 D2 C3 C8 C1 CE 54               MERCHANT
        C2 C1 D2 44                           BARD
        CA C5 D3 D4 C5 52                     JESTER
        C2 C5 C7 C7 C1 52                     BEGGAR
        C3 C8 C9 CC 44                        CHILD
        C2 D5 CC 4C                           BULL
        CC CF D2 C4 A0 C2 D2 C9 D4 C9 D3 48   LORD BRITISH

1D1C    D3 D5 CC C6 D5 D2 A0 C1 D3 48         SULFUR ASH
        C7 C9 CE D3 C5 CE 47                  GINSENG
        C7 C1 D2 CC C9 43                     GARLIC
        D3 D0 C9 C4 C5 D2 A0 D3 C9 CC 4B      SPIDER SILK
        C2 CC CF CF C4 A0 CD CF D3 53         BLOOD MOSS
        C2 CC C1 C3 CB A0 D0 C5 C1 D2 4C      BLACK PEARL
        CE C9 C7 C8 D4 D3 C8 C1 C4 45         NIGHTSHADE
        CD C1 CE C4 D2 C1 CB 45               MANDRAKE

1D6C    C1 D7 C1 CB C5 4E                     AWAKEN
        C2 CC C9 CE 4B                        BLINK
        C3 D5 D2 45                           CURE
        C4 C9 D3 D0 C5 CC 4C                  DISPELL
        C5 CE C5 D2 C7 59                     ENERGY
        C6 C9 D2 C5 C2 C1 CC 4C               FIREBALL
        C7 C1 D4 45                           GATE
        C8 C5 C1 4C                           HEAL
        C9 C3 C5 C2 C1 CC 4C                  ICEBALL
        CA C9 CE 58                           JINX
        CB C9 CC 4C                           KILL
        CC C9 C7 C8 54                        LIGHT
1DAC    CD C1 C7 C9 C3 A0 CD C9 D3 4C         MAGIC MISL
        CE C5 C7 C1 D4 45                     NEGATE
1DBC    CF D0 C5 4E                           OPEN
        D0 D2 CF D4 C5 C3 D4 C9 CF 4E         PROTECTION
        D1 D5 C9 C3 CB CE C5 D3 53            QUICKNESS
        D2 C5 D3 D3 D5 D2 C5 C3 54            RESSURECT
1DDC    D3 CC C5 C5 50                        SLEEP
        D4 D2 C5 CD CD CF 52                  TREMMOR
        D5 CE C4 C5 C1 44                     UNDEAD
        D6 C9 C5 57                           VIEW
        D7 C9 CE C4 53                        WINDS
        D8 AD C9 54                           X-IT
        D9 AD D5 50                           Y-UP
        DA AD C4 CF D7 4E                     Z-DOWN

        C2 D2 C9 D4 C1 CE CE C9 41                  BRITANNIA
        D4 C8 C5 A0 CC D9 C3 C1 C5 D5 4D            THE LYCAEUM
        C5 CD D0 C1 D4 C8 A0 C1 C2 C2 C5 59         EMPATH ABBEY
        D3 C5 D2 D0 C1 CE D4 D3 A0 C8 CF CC 44      SERPANTS HOLD
        CD CF CF CE C7 CC CF 57                     MOONGLOW
        C2 D2 C9 D4 C1 C9 4E                        BRITAIN
        CA C8 C5 CC CF 4D                           JHELOM
        D9 C5 57                                    YEW
        CD C9 CE CF 43                              MINOC
        D4 D2 C9 CE D3 C9 43                        TRINSIC
        D3 CB C1 D2 C1 A0 C2 D2 C1 45               SKARA BRAE
        CD C1 C7 C9 CE C3 C9 41                     MAGINCIA
        D0 C1 D7 53                                 PAWS
1E6C    C2 D5 C3 C3 C1 CE C5 C5 D2 D3 A0 C4 C5 4E   BUCCANEERS DEN
        D6 C5 D3 D0 C5 52                           VESPER
        C3 CF D6 45                                 COVE

        C4 C5 C3 C5 C9 54                           DECEIT
        C4 C5 D3 D0 C9 D3 45                        DESPISE
        C4 C1 D3 D4 C5 D2 44                        DASTERD
        D7 D2 CF CE 47                              WRONG
        C3 CF D6 C5 D4 CF D5 53                     COVETOUS
        D3 C8 C1 CD 45                              SHAME
        C8 D9 CC CF D4 C8 45                        HYLOTHE
        D4 C8 C5 A0 C7 D2 C5 C1 D4 8D               THE GREAT^M
          D3 D4 D9 C7 C9 C1 CE A0 C1 C2 D9 D3 D3 21   STYGIAN ABYSS!

        C8 CF CE C5 D3 D4 59                        HONESTY
        C3 CF CD D0 C1 D3 D3 C9 CF 4E               COMPASSION
        D6 C1 CC CF 52                              VALOR
        CA D5 D3 D4 C9 C3 45                        JUSTICE
        D3 C1 C3 D2 C9 C6 C9 C3 45                  SACRIFICE
        C8 CF CE CF 52                              HONOR
        D3 D0 C9 D2 C9 D4 D5 C1 CC C9 D4 59         SPIRITUALITY
        C8 D5 CD C9 CC C9 D4 59                     HUMILITY

1F08  A5 04         LDA $04
1F0A  20 07 15      JSR $1507     ; shift acc 4 bits to left (i.e. x 16)
1F0D  49 FF         EOR #$FF
1F0F  38            SEC
1F10  65 FA         ADC $FA
1F12  85 06         STA $06
1F14  C9 20         CMP #$20
1F16  B0 13         BCS $1F2B
1F18  A5 05         LDA $05
1F1A  20 07 15      JSR $1507     ; shift acc 4 bits to left (i.e. x 16)
1F1D  49 FF         EOR #$FF
1F1F  38            SEC
1F20  65 FB         ADC $FB
1F22  85 07         STA $07
1F24  C9 20         CMP #$20
1F26  B0 03         BCS $1F2B
1F28  4C 2E 1F      JMP $1F2E
1F2B  A9 FF         LDA #$FF
1F2D  60            RTS

;;

; $07 x16 -> $FC
; $06 AND 00001111 and OR'd with $FC and -> $FC
; so at this point $FC high nibble is low nibble of $7 and $FC low nibble is low nibble of $6
; $07 AND 00010000 and ASL'd so 00X00000 (where X is 5th bit of $07)
; that is OR'd with $06 and then divided by 16 and added to #$E8 = 11101000
; this is then stored in $FD
; the value at memory location at $FC/$FD is then loaded into ACC

; so...
; if $06 $07 is abcd efgh  ijkl mnop ...
; mnop efgh -> $FC
; 1110 10(l|c)0 -> $FD
; == $E8XX or $EAXX

1F2E  A5 07         LDA $07
1F30  20 07 15      JSR $1507     ; shift acc 4 bits to left (i.e. x 16)
1F33  85 FC         STA $FC
1F35  A5 06         LDA $06
1F37  29 0F         AND #$0F
1F39  05 FC         ORA $FC
1F3B  85 FC         STA $FC
1F3D  A5 07         LDA $07
1F3F  29 10         AND #$10
1F41  0A            ASL A
1F42  05 06         ORA $06
1F44  20 01 15      JSR $1501     ; shift acc 4 bits to right (i.e. // 16)
1F46  18            CLC
1F47  69 E8         ADC #$E8
1F49  85 FD         STA $FD
1F4C  A0 00         LDY #$00
1F4E  B1 FC         LDA ($FC),Y
1F50  60            RTS

;; DATA (at least at start)

1F51  00 0B 16 21 2C 37 42 4D 58 63 6E
1F5C  38 A5 FB E5 01 18 69 05 A8 B9 51 1F 85 DA 38 A5
1F6C  FA E5 00 18 69 05 18 65 DA 85 FE A9 02 85 FF A0
1F7C  00 B1 FE
1F7F  60

;;

1F80    A4 07       LDY $07         ;
1F82    B9 51 1F    LDA $1F51,Y     ;
1F85    18          CLC             ;
1F86    65 06       ADC $06         ;
1F88    A8          TAY             ; Y = $1F51[_07] + _06
1F89    B9 00 02    LDA $0200,Y     ;

1F8C    60          RTS             ;

;;

1F8D    20 80 1F    JSR $1F80       ; Y = $1F51[_07] + _06
1F90    B9 80 02    LDA $0280,Y     ;
1F93    60          RTS             ;

;;

1F94    A5 0B       LDA $0B         ;
1F96    C9 01       CMP #$01        ;
1F98    F0 07       BEQ $1FA1       ;
1F9A    C9 02       CMP #$02        ;
1F9C    F0 0E       BEQ $1FAC       ;
1F9E    A9 FF       LDA #$FF        ;
1FA0    60          RTS             ;

;;

1FA1    A5 00       LDA $00         ;
1FA3    85 06       STA $06         ;
1FA5    A5 01       LDA $01         ;
1FA7    85 07       STA $07         ;
1FA9    4C 2E 1F    JMP $1F2E       ;

;;

1FAC    A5 00       LDA $00         ;
1FAE    85 FA       STA $FA         ;
1FB0    A5 01       LDA $01         ;
1FB2    85 FB       STA $FB         ;
1FB4    4C B7 1F    JMP $1FB7       ;

;;

1FB7    A5 FB       LDA $FB         ;
1FB9    85 FD       STA $FD         ;
1FBB    A9 00       LDA #$00        ;
1FBD    46 FD       LSR $FD         ;
1FBF    6A          ROR A           ;
1FC0    46 FD       LSR $FD         ;
1FC2    6A          ROR A           ;
1FC3    46 FD       LSR $FD         ;
1FC5    6A          ROR A           ;
1FC6    65 FA       ADC $FA         ;
1FC8    85 FC       STA $FC         ;
1FCA    18          CLC             ;
1FCB    A5 FD       LDA $FD         ;
1FCD    69 E8       ADC #$E8        ;
1FCF    85 FD       STA $FD         ;
1FD1    A0 00       LDY #$00        ;
1FD3    B1 FC       LDA ($FC),Y     ;
1FD5    60          RTS             ;

;;

1FD6    A5 0C       LDA $0C         ;
1FD8    4A          LSR A           ;
1FD9    4A          LSR A           ;
1FDA    18          CLC             ;
1FDB    69 E8       ADC #$E8        ;
1FDD    85 FF       STA $FF         ;
1FDF    A5 0C       LDA $0C         ;
1FE1    29 03       AND #$03        ;
1FE3    0A          ASL A           ;
1FE4    0A          ASL A           ;
1FE5    0A          ASL A           ;
1FE6    05 07       ORA $07         ;
1FE8    0A          ASL A           ;
1FE9    0A          ASL A           ;
1FEA    0A          ASL A           ;
1FEB    05 06       ORA $06         ;
1FED    85 FE       STA $FE         ;
1FEF    A0 00       LDY #$00        ;
1FF1    B1 FE       LDA ($FE),Y     ;
1FF3    60          RTS             ;

;;

1FF4    30 41 30 41 30 41 30 41 30 41 30 41
```