---
grand_parent: DISK 1
parent: ULT4
nav_order: 1
---

# ULT4 full listing

```
ORG $2800 (actually loaded into $4000)
LEN $4800

;; vectors

4000    4C 09 40    JMP $4009       ;
4003    4C DE 6D    JMP $6DDE       ;
4006    4C 8B 70    JMP $708B       ;

4009    A9 00       LDA #$00        ;
400B    20 20 03    JSR $0320       ; SEL?
400E    2C 8B C0    BIT $C08B       ; bank switch?
4011    2C 8B C0    BIT $C08B       ; bank switch?
4014    20 12 08    JSR $0812       ; set the hires page one to all 0x80

4017    A9 0D       LDA #$0D        ; _CF = 0x0D (text row)
4019    85 CF       STA $CF         ; .
401B    A9 18       LDA #$18        ; _CE = 0x18 (text column)
401D    85 CE       STA $CE         ; .
401F    A9 00       LDA #$00        ; _0B = 0x00
4021    85 0B       STA $0B         ; .
4023    A9 25       LDA #$25        ; _C6 = 0x25 (avatar status character?)
4025    85 C6       STA $C6         ; .
4027    A9 02       LDA #$02        ; .
4029    20 42 08    JSR $0842       ; request disk switch to BRITANNIA
402C    A5 0F       LDA $0F         ; if party size is zero
402E    F0 03       BEQ $4033       ; go to $4033

4030    4C 8D 40    JMP $408D       ;

;; load party

4033    20 1B 08    JSR $081B       ; PRINT

4036    84 C2 CC CF C1 C4 A0 D0 D2 D4 D9 AC C1 A4 B0 8D 00                      ^DBLOAD PRTY,A$0^M^@

4047    A5 0F       LDA $0F         ; if party size is non-zero
4049    D0 18       BNE $4063       ; go to $4063
404B    20 21 08    JSR $0821       ; PRINT

404E    8D CE CF A0 C1 C3 D4 C9 D6 C5 A0 C7 C1 CD C5 A1 8D 00                   ^MNO ACTIVE GAME!^M^@

4060    4C 60 40    JMP $4060       ; hang

;; load list and roster

4063    20 1B 08    JSR $081B       ; PRINT

4066  84 C2 CC CF C1 C4 A0 CC C9 D3 D4 AC C1 A4 C5 C5 B0 B0 8D                  ^DBLOAD LIST,A$EE00^M
4079  84 C2 CC CF C1 C4 A0 D2 CF D3 D4 AC C1 A4 C5 C3 B0 B0 8D 00               ^DBLOAD ROST,A$EC00^M^@

408D    A9 00       LDA #$00        ;
408F    85 0D       STA $0D         ; _0D = 0x00
4091    85 F4       STA $F4         ; _F4 = 0x00
4093    85 0A       STA $0A         ; _0A = 0x00
4095    85 14       STA $14         ; _14 = 0x00
4097    A9 01       LDA #$01        ;
4099    85 0B       STA $0B         ; _0B = 0x01
409B    85 F5       STA $F5         ; _F5 = 0x01
409D    20 45 08    JSR $0845       ; DISPLAY STATS
40A0    20 03 08    JSR $0803       ; ???
40A3    A9 FF       LDA #$FF        ;
40A5    85 CD       STA $CD         ; _CD = 0xFF
40A7    A9 CF       LDA #$CF        ; A = 0xCF
40A9    20 20 03    JSR $0320       ; SEL ???

;;

40AC    20 4B 08    JSR $084B       ; ???
40AF    20 16 64    JSR $6416       ;
40B2    20 AA 84    JSR $84AA       ; maybe checks to see if anyone is awake in the party ???
40B5    10 03       BPL $40BA       ; skip
40B7    4C 36 41    JMP $4136       ; print Zzzzz then return

;;

40BA    20 21 08    JSR $0821       ;

40BD    8D 1E 00                                                                ^M.^@

40C0    20 00 08    JSR $0800       ; get a key ?
40C3    C9 00       CMP #$00        ; if not ^@
40C5    D0 03       BNE $40CA       ;   branch
40C7    4C 45 41    JMP $4145       ; pass

;;

40CA    C9 A0       CMP #$A0        ; if not ' '
40CC    D0 03       BNE $40D1       ;   branch
40CE    4C 45 41    JMP $4145       ; pass

;;

40D1    C9 8D       CMP #$8D        ; if not ^M
40D3    D0 03       BNE $40D8       ;   branch
40D5    4C 7D 42    JMP $427D       ;

;;

40D8    C9 8B       CMP #$8B        ; if not ^K
40DA    D0 03       BNE $40DF       ;   branch
40DC    4C 7D 42    JMP $427D       ;

;;

40DF    C9 8A       CMP #$8A        ; if not ^J
40E1    D0 03       BNE $40E6       ;   branch
40E3    4C 63 43    JMP $4363       ;

;;

40E6    C9 AF       CMP #$AF        ; if not /
40E8    D0 03       BNE $40ED       ;   branch
40EA    4C 63 43    JMP $4363       ;

;;

40ED    C9 88       CMP #$88        ; if not ^H (left arrow)
40EF    D0 03       BNE $40F4       ;   branch
40F1    4C 3C 44    JMP $443C       ;

;;

40F4    C9 95       CMP #$95        ; if not ^U (right arrow)
40F6    D0 03       BNE $40FB       ;   branch
40F8    4C 04 45    JMP $4504       ;

;;

40FB    C9 93       CMP #$93        ; if not ^S
40FD    D0 1D       BNE $411C       ;   branch
40FF    20 05 87    JSR $8705       ; print newline
4102    A9 00       LDA #$00        ;
4104    85 EA       STA $EA         ; _EA = 0x00

4106    A6 EA       LDX $EA         ;
4108    BD 00 ED    LDA $ED00,X     ;
410B    20 33 08    JSR $0833       ; output a byte as two digits
410E    E6 EA       INC $EA         ; _EA++
4110    A5 EA       LDA $EA         ; A = _EA
4112    C9 08       CMP #$08        ; if < 0x08
4114    90 F0       BCC $4106       ;   loop back

4116    20 05 87    JSR $8705       ; print newline
4119    4C AC 40    JMP $40AC       ;

;;

411C    C9 C1       CMP #$C1        ; if < 'A'
411E    90 31       BCC $4151       ;    go to $4151
4120    C9 DB       CMP #$DB        ; if > 'Z'
4122    B0 2D       BCS $4151       ;    go to $4151
4124    38          SEC             ;
4125    E9 C1       SBC #$C1        ; subtract 0xC1 (so 'A' => 0 and so on)
4127    0A          ASL A           ;
4128    A8          TAY             ; Y = 2 * A
4129    B9 49 42    LDA $4249,Y     ; get low byte of handler address
412C    85 FE       STA $FE         ;
412E    B9 4A 42    LDA $424A,Y     ; get high byte of handler address
4131    85 FF       STA $FF         ;
4133    6C FE 00    JMP (00FE)      ; jump to handler

;;

4136    20 21 08    JSR $0821       ; print

4139    8D 1E DA FA FA FA FA 8D 00                                              ^M>Zzzzz^M^@

4142    4C 84 63    JMP $6384       ; return to main loop

;; pass

4145    20 21 08    JSR $0821       ; print

4148    D0 E1 F3 F3 8D 00                                                       Pass^M^@

414E    C 84 63    JMP $6384        ; return to main loop

;; invalid character

4151    20 21 08    JSR $0821       ; print

4154    D7 C8 C1 D4 BF 8D 00                                                    WHAT?^M^@

415B    A9 02       LDA #$02        ; .
415D    20 54 08    JSR $0854       ; play sound 0x02
4160    4C 84 63    JMP $6384       ; return to main loop

;;

4163    20 21 08    JSR $0821       ; print

4166    CE CF D4 A0 C1 A0 D0 CC C1 D9 C5 D2 A1 8D 00                            NOT A PLAYER!^M^@

4175    4C 84 63    JMP $6384       ;

;;

4178    20 21 08    JSR $0821       ; print

417B    CF CE CC D9 A0 CF CE A0 C6 CF CF D4 A1 8D 00                            ONLY ON FOOT!^M^@

418A    4C 5B 41    JMP $415B       ; make error sound then jump back to $6384

;;

418D    20 21 08    JSR $0821       ; print

4190    CE CF D4 C8 C9 CE C7 A0 D4 C8 C5 D2 C5 A1 8D 00                         NOTHING THERE!^M^@

41A0    4C 84 63    JMP $6384       ; return to main loop

;;

41A3    20 21 08    JSR $0821       ; print

41A6    D3 CC CF D7 A0 D0 D2 CF C7 D2 C5 D3 D3 A1 8D 00                         SLOW PROGRESS!^M^@

41B6    60          RTS             ; return (rather than $6384)

;;

41B7    20 21 08    JSR $0821       ; print

41BA    CE CF D4 A0 C8 C5 D2 C5 A1 8D 00                                        NOT HERE!^M^@

41C5    4C 84 63    JMP $6384       ; return to main loop

;;

41C8    20 21 08    JSR $0821       ; print

41CB    C3 C1 CE A7 D4 A1 8D 00                                                 CAN'T!^M^@

41D3    4C 84 63    JMP $6384       ; return to main loop

;;

41D6    20 21 08    JSR $0821       ; print

41D9    C1 C2 CF D2 D4 C5 C4 A1 8D 00                                           ABORTED!^M^@

41E3    4C 84 63    JMP $6384       ; return to main loop

;;

41E6    20 45 08    JSR $0845       ; DISPLAY STATS
41E9    20 21 08    JSR $0821       ; print

41EC    C4 CF CE C5 AE 8D 00                                                    DONE.^M^@

41F3    4C 84 63    JMP $6384       ; return to main loop

;;

41F6    20 21 08    JSR $0821       ; print

41F9    D9 CF D5 A0 C8 C1 D6 C5 A0 CE CF CE C5 A1 8D 00                         YOU HAVE NONE!^M^@

4209    4C 84 63    JMP $6384       ; return to main loop

;;

420C    20 21 08    JSR $0821       ; print

420F    C4 C9 D3 C1 C2 CC C5 C4 A1 8D 00                                        DISABLED!^M^@

421A    4C 84 63    JMP $6384       ; return to main loop

;;

421D    20 21 08    JSR $0821       ; print

4220    C2 CC CF C3 CB C5 C4 A1 8D 00                                           BLOCKED!^M^@

422A    A9 01       LDA #$01        ;
422C    20 54 08    JSR $0854       ; play sound 0x01
422F    A9 00       LDA #$00        ; _B8 = 0x00
4231    85 B8       STA $B8         ; .
4233    4C 84 63    JMP $6384       ; return to main loop

;;

4236    20 21 08    JSR $0821       ; print

4239    C4 D2 C9 C6 D4 A0 CF CE CC D9 A1 8D 00                                  DRIFT ONLY!^M^@

4246    4C 84 63    JMP $6384       ; return to main loop


;; handlers

4249    03 47         ; 'A' => $4703
424B    B1 47         ; 'B' => $47B1
424D    82 48         ; 'C' => $4882
424F    3A 4F         ; 'D' => $4F3A
4251    18 50         ; 'E' => $5018
4253    D7 52         ; 'F' => $52D7
4255    97 53         ; 'G' => $5397
4257    37 55         ; 'H' => $5537
4259    A8 55         ; 'I' => $55A8
425B    D4 55         ; 'J' => $55D4
425D    0C 56         ; 'K' => $560C
425F    B2 56         ; 'L' => $56B2
4261    45 57         ; 'M' => $5745
4263    7F 57         ; 'N' => $577F
4265    27 58         ; 'O' => $5827
4267    74 58         ; 'P' => $5874
4269    E7 58         ; 'Q' => $58E7
426B    CC 59         ; 'R' => $59CC
426D    B6 5A         ; 'S' => $5AB6
426F    EB 5A         ; 'T' => $5AEB
4271    55 5C         ; 'U' => $5C55
4273    92 5C         ; 'V' => $5C92
4275    BE 5C         ; 'W' => $5CBE
4277    6D 5D         ; 'X' => $5D6D
4279    D4 5D         ; 'Y' => $5DD4
427B    0F 5E         ; 'Z' => $5E0F

;; ^M or ^K

427D    A5 0B       LDA $0B         ; if _0B
427F    C9 03       CMP #$03        ;   != 0x03
4281    D0 03       BNE $4286       ;     skip

4283    4C 2C 43    JMP $432C       ; jump to 'Advance' (if _0B == 0x03)

4286    A5 0E       LDA $0E         ; if _0E
4288    C9 18       CMP #$18        ;   != 0x18
428A    D0 03       BNE $428F       ;      skip

428C    4C 36 42    JMP $4236       ; jump to 'DRIFT ONLY!' (if _0E == 0x18)

428F    C9 11       CMP #$11        ; if _0E != 0x11
4291    D0 03       BNE $4296       ;   skip

4293    4C C7 42    JMP $42C7       ; jump to (if _0E == 0x11)

4296    C9 10       CMP #$10        ; if _0E == 0x10
4298    F0 20       BEQ $42BA       ;
429A    C9 12       CMP #$12        ; if _0E == 0x12
429C    F0 1C       BEQ $42BA       ;
429E    C9 13       CMP #$13        ; if _E == 0x13
42A0    F0 18       BEQ $42BA       ;

42A2    A9 00       LDA #$00        ;
42A4    20 54 08    JSR $0854       ; play sound 0x00
42A7    20 05 84    JSR $8405       ; print 'North^M'
42AA    A5 14       LDA $14         ; if _14 == 00
42AC    F0 06       BEQ ---         ;   go to $42B4
42AE    20 EA 42    JSR $42EA       ; otherwise
42B1    20 4B 08    JSR $084B       ;

42B4    20 EA 42    JSR $42EA       ;
42B7    4C 84 63    JMP $6384       ; return to main loop

42BA    A9 11       LDA #$11        ;
42BC    85 0E       STA $0E         ;
42BE    20 DE 45    JSR $45DE       ; print 'Turn '
42C1    20 05 84    JSR $8405       ; print 'North^M'
42C4    4C 84 63    JMP $6384       ; return to main loop

42C7    20 E8 45    JSR $45E8       ; print 'Sail '
42CA    20 05 84    JSR $8405       ; print 'North^M'
42CD    A5 C9       LDA $C9         ;
42CF    20 F1 46    JSR $46F1       ;
42D2    10 03       BPL $42D7       ;
42D4    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

42D7    A9 01       LDA #$01        ;
42D9    20 A3 6B    JSR $6BA3       ;
42DC    F0 06       BEQ $42E4       ;
42DE    20 A3 41    JSR $41A3       ; display 'SLOW PROGRESS!'
42E1    4C 84 63    JMP $6384       ; return to main loop

42E4    20 0F 08    JSR $080F       ; something about south wind
42E7    4C 84 63    JMP $6384       ; return to main loop

;;

42EA    A5 C8       LDA $C8         ; load _C8
42EC    C9 0E       CMP #$0E        ; if _C8 == 0x0E,
42EE    F0 0B       BEQ $42FB       ;     abort move and show 'BLOCKED!'
42F0    A5 C9       LDA $C9         ; load _C9
42F2    C9 0E       CMP #$0E        ; if _C9 == 0x0E,
42F4    F0 0A       BEQ $4300       ;     skip to $4300
42F6    20 48 08    JSR $0848       ; if _C9 in the data at 12F1
42F9    10 05       BPL $4300       ; skip to $4300
42FB    68          PLA             ; otherwise pull two bytes off
42FC    68          PLA             ;    stack (to skip returning?)
42FD    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

4300    A5 C9       LDA $C9         ; load _C9 (some kind of terrain type)
4302    20 BC 46    JSR $46BC       ; different chance of $FF vs $00 depending on _C9
4305    F0 06       BEQ $430D       ; randomly proceed with move
4307    20 A3 41    JSR $41A3       ; otherwise, display 'SLOW PROGRESS!'
430A    4C 2B 43    JMP $432B       ; return

430D    A9 00       LDA #$00        ; .
430F    20 54 08    JSR $0854       ; play sound A
4312    A5 0B       LDA $0B         ; A = _0B
4314    C9 01       CMP #$01        ; if _0B != 0x01
4316    D0 08       BNE $4320       ; skip
4318    20 0F 08    JSR $080F       ; if _0B == 0x01, call $080F
431B    A5 C9       LDA $C9         ; A = _C9
431D    4C 84 87    JMP $8784       ; jump to $8784

4320    C6 01       DEC $01         ; _01--
4322    A5 01       LDA $01         ; A = _01
4324    10 05       BPL ---         ; if highbit is zero, return
4326    68          PLA             ; otherwise abort return address
4327    68          PLA             ; .
4328    4C 4C 46    JMP $464C       ; and jump to $464C

432B    60          RTS             ;

;;

432C    20 21 08    JSR $0821       ; print

432F    C1 E4 F6 E1 EE E3 E5 8D 00                                              Advance^M^@

4338    A5 C9       LDA $C9         ;
433A    20 E2 80    JSR $80E2       ;
433D    10 03       BPL $4342       ;

433F    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

4342    A6 10       LDX $10         ;
4344    18          CLC             ;
4345    A5 00       LDA $00         ;
4347    7D 5B 43    ADC $435B,X     ;
434A    29 07       AND #$07        ;
434C    85 00       STA $00         ;
434E    18          CLC             ;
434F    A5 01       LDA $01         ;
4351    7D 5F 43    ADC $435F,X     ;
4354    29 07       AND #$07        ;
4356    85 01       STA $01         ;
4358    4C 84 63    JMP $6384       ; return to main loop

;; DATA

435B    00 01 00 FF FF 00 01 00

;; ^J or /

4363    A5 0B       LDA $0B         ;
4365    C9 03       CMP #$03        ;
4367    D0 1E       BNE ---         ;
4369    20 21 08    JSR $0821       ; print

436C    D2 E5 F4 F2 E5 E1 F4 8D 00                      Retreat..

4375    A5 CC       LDA $CC         ;
4377    20 F4 80    JSR $80F4       ;
437A    10 03       BPL ---         ;
437C    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

437F    A5 10       LDA $10         ;
4381    49 02       EOR #$02        ;
4383    AA          TAX             ;
4384    4C 44 43    JMP $4344       ;
4387    A5 0E       LDA $0E         ;
4389    C9 18       CMP #$18        ;
438B    D0 03       BNE ---         ;
438D    4C 36 42    JMP $4236       ; jump to 'DRIFT ONLY!'

4390    C9 13       CMP #$13        ;
4392    D0 03       BNE ---         ;
4394    4C DF 43    JMP $43DF       ;

4397    C9 12       CMP #$12        ;
4399    F0 37       BEQ ---         ;
439B    C9 10       CMP #$10        ;
439D    F0 33       BEQ ---         ;
439F    C9 11       CMP #$11        ;
43A1    F0 2F       BEQ ---         ;
43A3    A9 00       LDA #$00        ;
43A5    20 54 08    JSR $0854       ;
43A8    20 10 84    JSR $8410       ; print 'South^M'
43AB    A5 14       LDA $14         ;
43AD    F0 06       BEQ ---         ;
43AF    20 02 44    JSR $4402       ;
43B2    20 4B 08    JSR $084B       ;
43B5    20 02 44    JSR $4402       ;
43B8    A5 00       LDA $00         ;
43BA    C9 E5       CMP #$E5        ;
43BC    90 11       BCC ---         ;
43BE    C9 EA       CMP #$EA        ;
43C0    B0 0D       BCS ---         ;
43C2    A5 01       LDA $01         ;
43C4    C9 D4       CMP #$D4        ;
43C6    90 07       BCC ---         ;
43C8    C9 D9       CMP #$D9        ;
43CA    B0 03       BCS ---         ;
43CC    4C 29 46    JMP $4629       ;

43CF    4C 84 63    JMP $6384       ; return to main loop

43D2    A9 13       LDA #$13        ;
43D4    85 0E       STA $0E         ;
43D6    20 DE 45    JSR $45DE       ; print 'Turn '
43D9    20 10 84    JSR $8410       ; print 'South^M'
43DC    4C 84 63    JMP $6384       ; return to main loop

43DF    20 E8 45    JSR $45E8       ; print 'Sail '
43E2    20 10 84    JSR $8410       ; print 'South^M'
43E5    A5 CA       LDA $CA         ;
43E7    20 F1 46    JSR $46F1       ;
43EA    10 03       BPL ---         ;
43EC    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

43EF    A9 03       LDA #$03        ;
43F1    20 A3 6B    JSR $6BA3       ;
43F4    F0 06       BEQ ---         ;
43F6    20 A3 41    JSR $41A3       ; display 'SLOW PROGRESS!'
43F9    4C 84 63    JMP $6384       ; return to main loop

43FC    20 0C 08    JSR $080C       ;
43FF    4C 84 63    JMP $6384       ; return to main loop

4402    A5 CA       LDA $CA         ;
4404    20 48 08    JSR $0848       ;
4407    10 05       BPL ---         ;
4409    68          PLA             ;
440A    68          PLA             ;
440B    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

440E    A5 CA       LDA $CA         ;
4410    20 BC 46    JSR $46BC       ;
4413    F0 06       BEQ ---         ;
4415    20 A3 41    JSR $41A3       ; display 'SLOW PROGRESS!'
4418    4C 3B 44    JMP $443B       ;

441B    A9 00       LDA #$00        ;
441D    20 54 08    JSR $0854       ;
4420    A5 0B       LDA $0B         ;
4422    C9 01       CMP #$01        ;
4424    D0 08       BNE ---         ;
4426    20 0C 08    JSR $080C       ;
4429    A5 CA       LDA $CA         ;
442B    4C 84 87    JMP $8784       ;

442E    E6 01       INC $01         ;
4430    A5 01       LDA $01         ;
4432    C9 20       CMP #$20        ;
4434    90 05       BCC ---         ;
4436    68          PLA             ;
4437    68          PLA             ;
4438    4C 4C 46    JMP $464C       ;

443B    60          RTS             ;

;; ^H / left arrow

443C    A5 0B       LDA $0B         ;
443E    C9 03       CMP #$03        ;
4440    D0 1E       BNE ---         ;
4442    20 DE 45    JSR $45DE       ; print 'Turn '
4445    20 21 08    JSR $0821       ; print

4448    EC E5 E6 F4 8D 00                                                       left^M^@

444C    38          SEC             ;
444D    A5 10       LDA $10         ;
444F    E9 01       SBC #$01        ;
4451    29 03       AND #$03        ;
4453    85 10       STA $10         ;
4455    20 00 8C    JSR $8C00       ; ? redraw dungeon
4458    20 72 08    JSR $0872       ; in dungeon, display direction and level
445B    4C AC 40    JMP $40AC       ;

445E    A5 0E       LDA $0E         ;
4460    C9 18       CMP #$18        ;
4462    D0 03       BNE ---         ;
4464    4C 36 42    JMP $4236       ; jump to 'DRIFT ONLY!'

4467    C9 10       CMP #$10        ;
4469    D0 03       BNE ---         ;
446B    4C A9 44    JMP $44A9       ;

446E    C9 11       CMP #$11        ;
4470    F0 28       BEQ ---         ;
4472    C9 13       CMP #$13        ;
4474    F0 24       BEQ ---         ;
4476    C9 12       CMP #$12        ;
4478    F0 20       BEQ ---         ;
447A    C9 15       CMP #$15        ;
447C    D0 04       BNE ---         ;
447E    A9 14       LDA #$14        ;
4480    85 0E       STA $0E         ;
4482    A9 00       LDA #$00        ;
4484    20 54 08    JSR $0854       ;
4487    20 25 84    JSR $8425       ; print 'West^M'
448A    A5 14       LDA $14         ;
448C    F0 06       BEQ ---         ;
448E    20 CC 44    JSR $44CC       ;
4491    20 4B 08    JSR $084B       ;
4494    20 CC 44    JSR $44CC       ;
4497    4C 84 63    JMP $6384       ; return to main loop

449A    A9 10       LDA #$10        ;
449C    85 0E       STA $0E         ;
449E    20 DE 45    JSR $45DE       ; print 'Turn '
44A1    20 25 84    JSR $8425       ; print 'West^M'
44A4    4C 84 63    JMP $6384       ; return to main loop

44A7    20 E8 45    JSR $45E8       ; print 'Sail '
44AA    20 25 84    JSR $8425       ; print 'West^M'
44AD    A5 CC       LDA $CC         ;
44AF    20 F1 46    JSR $46F1       ;
44B2    10 03       BPL ---         ;
44B4    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

44B7    A9 00       LDA #$00        ;
44B9    20 A3 6B    JSR $6BA3       ;
44BC    F0 06       BEQ ---         ;
44BE    20 A3 41    JSR $41A3       ; display 'SLOW PROGRESS!'
44C1    4C 84 63    JMP $6384       ; return to main loop

44C4    20 09 08    JSR $0809       ;
44C7    4C 84 63    JMP $6384       ; return to main loop

44CA    A5 CC       LDA $CC         ;
44CC    20 48 08    JSR $0848       ;
44CF    10 05       BPL ---         ;
44D1    68          PLA             ;
44D2    68          PLA             ;
44D3    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

44D6    A5 CC       LDA $CC         ;
44D8    20 BC 46    JSR $46BC       ;
44DB    F0 06       BEQ ---         ;
44DD    20 A3 41    JSR $41A3       ; display 'SLOW PROGRESS!'
44E0    4C 03 45    JMP $4503       ;

44E3    A9 00       LDA #$00        ;
44E5    20 54 08    JSR $0854       ;
44E8    A5 0B       LDA $0B         ;
44EA    C9 01       CMP #$01        ;
44EC    D0 08       BNE ---         ;
44EE    20 09 08    JSR $0809       ;
44F1    A5 CC       LDA $CC         ;
44F3    4C 84 87    JMP $8784       ;

44F6    C6 00       DEC $00         ;
44F8    A5 00       LDA $00         ;
44FA    10 05       BPL ---         ;
44FC    68          PLA             ;
44FD    68          PLA             ;
44FE    4C 4C 46    JMP $464C       ;

4501    60          RTS             ;


;; ^U / right arrow

4504    A5 0B       LDA $0B         ; are we in dungeon?
4506    C9 03       CMP #$03        ;
4508    D0 1F       BNE $4529       ; if not, skip
450A    20 DE 45    JSR $45DE       ; print 'Turn '

450D    20 21 08    JSR $0821       ; print

4510    F2 E9 E7 E8 F4 8D 00                                                    right^M^@

4517    18          CLC             ; turn right in dungeon
4518    A5 10       LDA $10         ; .
451A    69 01       ADC #$01        ; .
451C    29 03       AND #$03        ; .
451E    85 10       STA $10         ; .
4520    20 00 8C    JSR $8C00       ; ? redraw dungeon
4523    20 72 08    JSR $0872       ; in dungeon, display direction and level
4526    4C AC 40    JMP $40AC       ; back to main loop ?

;;

4529    A5 0E       LDA $0E         ; if _0E == 0x18, go to $4236 balloon 'DRIFT ONLY!'
452B    C9 18       CMP #$18        ; otherwise continue
452D    D0 03       BNE $4532       ; .
452F    4C 36 42    JMP $4236       ; .

4532    C9 12       CMP #$12        ; if _0E == 0x12, go to $4572 'Sail East' (as already facing east)
4534    D0 03       BNE $4539       ; otherwise continue
4536    4C 72 45    JMP $4572       ; .

4539    C9 11       CMP #$11        ; if _0E == 0x11, go to $4565
453B    F0 28       BEQ $4565       ; .
453D    C9 13       CMP #$13        ; if _0E == 0x13, go to $4565
453F    F0 24       BEQ $4565       ; .
4541    C9 10       CMP #$10        ; if _0E == 0x10, go to $4565
4543    F0 20       BEQ $4565       ; .
4545    C9 14       CMP #$14        ; if _0E == 0x14  on a horse facing left
4547    D0 04       BNE $454D       ; .
4549    A9 15       LDA #$15        ; .
454B    85 0E       STA $0E         ; _0E := 0x15  switch to a horse facing right
454D    A9 00       LDA #$00        ; .
454F    20 54 08    JSR $0854       ; play sound 00
4552    20 1B 84    JSR $841B       ; print 'East^M'
4555    A5 14       LDA $14         ;
4557    F0 06       BEQ $455F       ;
4559    20 A4 45    JSR $45A4       ;
455C    20 4B 08    JSR $084B       ;
455F    20 A4 45    JSR $45A4       ;
4562    4C 84 63    JMP $6384       ; return to main loop

;; ship turning to east
4565    20 DE 45    JSR $45DE       ; print 'Turn '
4568    20 1B 84    JSR $841B       ; print 'East^M'
456B    A9 12       LDA #$12        ;
456D    85 0E       STA $0E         ; _0E = 0x12 (change icon to east-facing ship)
456F    4C 84 63    JMP $6384       ; return to main loop

4572    20 E8 45    JSR $45E8       ; print 'Sail '
4575    20 1B 84    JSR $841B       ; print 'East^M'
4578    A5 CB       LDA $CB         ;
457A    20 F1 46    JSR $46F1       ;
457D    10 03       BPL ---         ;
457F    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'
4582    A9 02       LDA #$02        ;
4584    20 A3 6B    JSR $6BA3       ;
4587    F0 06       BEQ ---         ;
4589    20 A3 41    JSR $41A3       ;
458C    4C 84 63    JMP $6384       ; return to main loop

458F    20 06 08    JSR $0806       ;
4592    A5 00       LDA $00         ;
4594    C9 DD       CMP #$DD        ;
4596    D0 09       BNE ---         ;
4598    A5 01       LDA $01         ;
459A    C9 E0       CMP #$E0        ;
459C    D0 03       BNE ---         ;
459E    4C F2 45    JMP $45F2       ;
45A1    4C 84 63    JMP $6384       ; return to main loop

45A4    A5 CB       LDA $CB         ;
45A6    20 48 08    JSR $0848       ;
45A9    10 05       BPL $45B0       ;
45AB    68          PLA             ;
45AC    68          PLA             ;
45AD    4C 1D 42    JMP $421D       ; jump to 'BLOCKED!'

45B0    A5 CB       LDA $CB         ;
45B2    20 BC 46    JSR $46BC       ;
45B5    F0 06       BEQ ---         ;
45B7    20 A3 41    JSR $41A3       ;
45BA    4C DD 45    JMP $45DD       ;
45BD    A9 00       LDA #$00        ;
45BF    20 54 08    JSR $0854       ;
45C2    A5 0B       LDA $0B         ;
45C4    C9 01       CMP #$01        ;
45C6    D0 08       BNE ---         ;
45C8    20 06 08    JSR $0806       ;
45CB    A5 CB       LDA $CB         ;
45CD    4C 84 87    JMP $8784       ;
45D0    E6 00       INC $00         ;
45D2    A5 00       LDA $00         ;
45D4    C9 20       CMP #$20        ;
45D6    90 05       BCC ---         ;
45D8    68          PLA             ;
45D9    68          PLA             ;
45DA    4C 4C 46    JMP $464C       ;
45DD    60          RTS             ;

45DE    20 21 08    JSR $0821       ; PRINT

45E1    D4 F5 F2 EE A0 00                                                       Turn ^@

45E7    60          RTS             ;

45E8    20 21 08    JSR $0821       ; PRINT

45EB    D3 E1 E9 EC A0 00                                                       Sail ^@

45F1    60          RTS             ;

45F2    A0 07       LDY #$07        ;
45F4    B9 11 46    LDA $4611,Y     ;
45F7    99 20 EE    STA $EE20,Y     ;
45FA    B9 19 46    LDA $4619,Y     ;
45FD    99 40 EE    STA $EE40,Y     ;
4600    A9 80       LDA #$80        ;
4602    99 60 EE    STA $EE60,Y     ;
4605    B9 21 46    LDA $4621,Y     ;
4608    99 00 EE    STA $EE00,Y     ;
460B    88          DEY             ;
460C    10 E6       BPL ---         ;
460E    4C 84 63    JMP $6384       ; return to main loop

4611    E0 E0 E2 E3 E4 E5 E5 E4
4619    DC E4 DC E4 E3 E1 DF DE
4621    82 82 82 82 83 83 81 81

4629    A5 C6       LDA $C6         ;
462B    C9 5F       CMP #$5F        ;
462D    F0 1A       BEQ ---         ;
462F    A0 07       LDY #$07        ;
4631    A9 F0       LDA #$F0        ;
4633    99 60 EE    STA $EE60,Y     ;
4636    99 00 EE    STA $EE00,Y     ;
4639    A9 E7       LDA #$E7        ;
463B    99 20 EE    STA $EE20,Y     ;
463E    A5 01       LDA $01         ;
4640    18          CLC             ;
4641    69 01       ADC #$01        ;
4643    99 40 EE    STA $EE40,Y     ;
4646    88          DEY             ;
4647    10 E8       BPL ---         ;
4649    4C 84 63    JMP $6384       ; return to main loop

464C    A9 00       LDA #$00        ;
464E    20 20 03    JSR $0320       ;
4651    20 21 08    JSR $0821       ; print

4654    CC C5 C1 D6 C9 CE C7 AE AE AE 8D 00                                     LEAVING...^M^@

4660    2C 10 C0    BIT $C010       ;
4663    A9 00       LDA #$00        ;
4665    85 0B       STA $0B         ;
4667    A9 02       LDA #$02        ; .
4669    20 42 08    JSR $0842       ; request disk switch to BRITANNIA
466C    20 1B 08    JSR $081B       ; print

466F    84 C2 CC CF C1 C4 A0 D4 CC D3 D4 8D 00                                  ^DBLOAD TLST^M^@

467C    A5 08       LDA $08         ;
467E    85 00       STA $00         ;
4680    A5 09       LDA $09         ;
4682    85 01       STA $01         ;
4684    20 03 08    JSR $0803       ;
4687    A9 01       LDA #$01        ;
4689    85 0B       STA $0B         ;
468B    A9 00       LDA #$00        ;
468D    85 0A       STA $0A         ;
468F    A5 00       LDA $00         ;
4691    C9 EF       CMP #$EF        ;
4693    D0 18       BNE ---         ;
4695    A5 01       LDA $01         ;
4697    C9 F0       CMP #$F0        ;
4699    D0 12       BNE ---         ;
469B    A9 18       LDA #$18        ;
469D    8D 7F EE    STA $EE7F       ;
46A0    8D 1F EE    STA $EE1F       ;
46A3    A9 E9       LDA #$E9        ;
46A5    8D 3F EE    STA $EE3F       ;
46A8    A9 F2       LDA #$F2        ;
46AA    8D 5F EE    STA $EE5F       ;
46AD    20 18 08    JSR $0818       ;
46B0    A9 CF       LDA #$CF        ;
46B2    20 20 03    JSR $0320       ;
46B5    A9 00       LDA #$00        ;
46B7    85 B8       STA $B8         ;
46B9    4C 84 63    JMP $6384       ; return to main loop

;;
; randomly returns $FF or $00 depending on A
; A == 0x03   1 in 4 FF
; A == 0x05   1 in 8 FF
; A == 0x06   1 in 4 FF
; A == 0x07   1 in 4 FF
; A == 0x46   1 in 2 FF
; otherwise   0 change of FF

46BC    C9 03       CMP #$03        ; if A == 0x03
46BE    F0 1F       BEQ $46DF       ;   go to $46DF (25% chance of $FF)
46C0    C9 05       CMP #$05        ; if A == 0x05
46C2    F0 12       BEQ $46D6       ;   go to $46D6 (12.5% chance of $FF)
46C4    C9 06       CMP #$06        ; if A == 0x06
46C6    F0 17       BEQ $46DF       ;   go to 46DF (25% chance of $FF)
46C8    C9 07       CMP #$07        ; if A == 0x07
46CA    F0 1C       BEQ $46E8       ;   go to 46E8 (50% change of $FF)
46CC    C9 46       CMP #$46        ; if A == 0x46
46CE    F0 18       BEQ $46E8       ;   go to 46E8 (50% chance of $FF)
46D0    A9 00       LDA #$00        ; otherwise
46D2    60          RTS             ;   return with A = 0x00 (0% chance of $FF)

46D3    A9 FF       LDA #$FF        ;
46D5    60          RTS             ;

;; return 0xFF with 12.5% otherwise 0x00

46D6    20 4E 08    JSR $084E       ; random number
46D9    29 07       AND #$07        ; if all three lower bits 0 (12.5% chance)
46DB    F0 F6       BEQ $46D3       ; return with A = 0xFF
46DD    D0 F1       BNE $46D0       ; otherwise, return with A = 0x00

;; return 0xFF with 25% otherwise 0x00

46DF    20 4E 08    JSR $084E       ; random number
46E2    29 03       AND #$03        ; if both two lower bits 0 (25% chance)
46E4    F0 ED       BEQ $46D3       ; return with A = 0xFF
46E6    D0 E8       BNE $46D0       ; otherwise, return with A = 0x00

;; return 0xFF with 50% otherwise 0x00

46E8    20 4E 08    JSR $084E       ; random number
46EB    29 01       AND #$01        ; if lowest bit 0 (50% change)
46ED    F0 E1       BEQ $46D0       ; return with A = 0x00
46EF    D0 E2       BNE $46D3       ; otherwise, return with A = 0xFF

;;

46F1    C9 02       CMP #$02        ;
46F3    90 08       BCC ---         ;
46F5    C9 8C       CMP #$8C        ;
46F7    90 07       BCC ---         ;
46F9    C9 90       CMP #$90        ;
46FB    B0 03       BCS ---         ;
46FD    A9 00       LDA #$00        ;
46FF    60          RTS             ;

4700    A9 FF       LDA #$FF        ;
4702    60          RTS             ;

;; 'A' handler (Attack)

4703    20 21 08    JSR $0821       ; print

4706    C1 F4 F4 E1 E3 EB AD 00                                                 Attack-^@

470E    A5 0B       LDA $0B         ; .
4710    C9 03       CMP #$03        ; if _0B != 0x03
4712    D0 03       BNE $4717       ;     skip

4714    4C 51 41    JMP $4151       ; jump to 'WHAT?' (if _0B == 0x03)

4717    20 96 83    JSR $8396       ;
471A    D0 03       BNE $471F       ;

471C    4C 45 41    JMP $4145       ; jump to 'Pass'

471F    20 2F 84    JSR $842F       ;
4722    10 03       BPL $4727       ;

4724    4C 8D 41    JMP $418D       ; jump to 'NOTHING THERE!'

4727    86 D8       STX $D8         ;
4729    BD 60 EE    LDA $EE60,X     ;
472C    C9 8C       CMP #$8C        ;
472E    F0 F4       BEQ $4724       ;

4730    C9 8E       CMP #$8E        ;
4732    F0 F0       BEQ $4724       ;
4734    A5 0B       LDA $0B         ;
4736    C9 02       CMP #$02        ;
4738    D0 1A       BNE $4754       ;
473A    A2 1F       LDX #$1F        ;

473C    BD 60 EE    LDA $EE60,X     ;
473F    C9 50       CMP #$50        ;
4741    F0 04       BEQ $4747       ;
4743    C9 5E       CMP #$5E        ;
4745    D0 0A       BNE $4751       ;
4747    A9 FF       LDA #$FF        ;
4749    9D C0 EE    STA $EEC0,X     ;
474C    A9 00       LDA #$00        ;
474E    9D E0 EE    STA $EEE0,X     ;
4751    CA          DEX             ;
4752    10 E8       BPL $473C       ;

4754    A5 0B       LDA $0B         ; get _0B
4756    C9 02       CMP #$02        ; if _0B == 0x02
4758    F0 0A       BEQ $4764       ;   go to $4764
475A    A6 D8       LDX $D8         ; get _D8
475C    BD 60 EE    LDA $EE60,X     ; A = _EE60[_D8]
475F    20 DB 87    JSR $87DB       ; if not in [$00, $8A, $90, $94, $98, $B4, $CC]
4762    30 15       BMI $4779       ;   skip

; if _0B == $02 or
; if _EE60[_D8] in [$00, $8A, $90, $94, $98, $B4, $CC]
; reduce some virtues

4764    A0 01       LDY #$01        ; Y = 0x01
4766    A9 05       LDA #$05        ; A = 0x05
4768    20 80 85    JSR $8580       ; subtract _ED00[1] by 5
476B    A0 03       LDY #$03        ; Y = 0x03
476D    A9 03       LDA #$03        ; A = 0x03
476F    20 80 85    JSR $8580       ; subtract _ED00[3] by 3
4772    A0 05       LDY #$05        ; Y = 0x05
4774    A9 03       LDA #$03        ; A = 0x03
4776    20 80 85    JSR $8580       ; subtract _ED00[5] by 3

4779    A6 D8       LDX $D8         ;
477B    BD 60 EE    LDA $EE60,X     ;
477E    85 C0       STA $C0         ;
4780    BD 20 EE    LDA $EE20,X     ;
4783    85 C1       STA $C1         ;
4785    85 FA       STA $FA         ;
4787    BD 40 EE    LDA $EE40,X     ;
478A    85 C2       STA $C2         ;
478C    85 FB       STA $FB         ;
478E    A5 0B       LDA $0B         ;
4790    C9 01       CMP #$01        ;
4792    D0 08       BNE $479C       ;
4794    20 81 08    JSR $0881       ;
4797    85 C3       STA $C3         ;
4799    4C A1 47    JMP $47A1       ;

479C    20 93 08    JSR $0893       ;
479F    85 C3       STA $C3         ;

47A1    A9 00       LDA #$00        ;
47A3    A6 D8       LDX $D8         ;
47A5    9D 60 EE    STA $EE60,X     ;
47A8    9D 00 EE    STA $EE00,X     ;
47AB    9D C0 EE    STA $EEC0,X     ;
47AE    4C B0 6F    JMP $6FB0       ; enter combat

;; 'B' handler (Board, Mount)

47B1    A5 0E       LDA $0E         ; check location?
47B3    C9 1F       CMP #$1F        ; is it #$1F?
47B5    F0 0F       BEQ $47C6       ; then skip, otherwise
47B7    20 21 08    JSR $0821       ; PRINT

47BA    C2 EF E1 F2 E4 A0 BC AD 00                                              Board <-^@

47C3    4C C8 41    JMP $41C8       ; print "CAN'T" and return to main loop

; depending on value of $C8 we do different things:
;
; #$14, #$15                $4822   Mount horse!
; #$10, #$11, #$12, #$13    $47F6   Board frigate!
; #$18                      $483B   Board balloon
; otherwise                 $4151   print "WHAT?" and return to main loop
;
; there are four variants of frigate because there are four tiles
; (facing different directions); similarly two horse tiles
;
; $C8 must be: what tile player is currently on (offset in SHP file)


47C6    A5 C8       LDA $C8         ; if $C8
47C8    C9 14       CMP #$14        ; == #$14
47CA    F0 56       BEQ $4822       ; go to $4822
47CC    C9 15       CMP #$15        ; or if $C8 == #$15
47CE    F0 52       BEQ $4822       ; also go to $4822, otherwise
47D0    20 21 08    JSR $0821       ; PRINT

47D3    C2 EF E1 F2 E4 A0 00                                                    Board ^@

47DA    A5 C8       LDA $C8         ; if $C8
47DC    C9 10       CMP #$10        ;   == #$10
47DE    F0 16       BEQ $47F6       ; frigate
47E0    C9 11       CMP #$11        ; or == #$11
47E2    F0 12       BEQ $47F6       ; frigate
47E4    C9 12       CMP #$12        ; or == #$12
47E6    F0 0E       BEQ $47F6       ; frigate
47E8    C9 13       CMP #$13        ; or == #$13
47EA    F0 0A       BEQ $47F6       ; frigate
47EC    C9 18       CMP #$18        ; but if $18
47EE    D0 03       BNE $47F3       ; .
47F0    4C 3B 48    JMP $483B       ; balloon, otherwise

47F3    4C 51 41    JMP $4151       ; print "WHAT?" and return to main loop

; frigate

47F6    A9 10       LDA #$10        ;
47F8    20 53 48    JSR $4853       ;
47FB    20 21 08    JSR $0821       ; PRINT

47FE    E6 F2 E9 E7 E1 F4 E5 A1 8D 00                                           frigate!^M^@

; if $00 != $4820 or $01 != $4821, set ship stength to #$50
; either way, return to main loop aftwards

4808    A5 00       LDA $00         ; if $00 != $4820
480A    CD 20 48    CMP $4820       ; .
480D    D0 0A       BNE $4819       ; set ship strength to #$50
480F    A5 01       LDA $01         ; if $01 != $4821
4811    CD 21 48    CMP $4821       ; .
4814    D0 03       BNE $4819       ; set ship strength to #$50
4816    4C 84 63    JMP $6384       ; return to main loop

4819    A9 50       LDA #$50        ; set ship strength to #$50
481B    85 1B       STA $1B         ; .
481D    4C 84 63    JMP $6384       ; return to main loop

4820    00 00                       ; some sort of variables; not set in this file



4822    A9 14       LDA #$14        ;
4824    20 53 48    JSR $4853       ;
4827    20 21 08    JSR $0821       ; PRINT

482A    CD EF F5 EE F4 A0 E8 EF F2 F3 E5 A1 8D 00                               Mount horse!^M^@

4838    4C 84 63    JMP $6384       ; return to main loop

483B    A9 18       LDA #$18        ;
483D    20 53 48    JSR $4853       ;
4840    20 21 08    JSR $0821       ; PRINT

4843    E2 E1 EC EC EF EF EE 8D 00                                              balloon..

484C    A9 00       LDA #$00        ;
484E    85 F4       STA $F4         ;
4850    4C 84 63    JMP $6384       ; return to main loop

;;

; in frigate case, A is #$10
; in horse case, A is #$14
; in balloon case, A is #$18
; notice these are the lowest in the ranges $C8 is checked for
; which suggests they represent a broader category here

4853    85 D8       STA $D8         ;
4855    A2 1F       LDX #$1F        ;
4857    BD 60 EE    LDA $EE60,X     ;
485A    29 FC       AND #$FC        ;
485C    C5 D8       CMP $D8         ;
485E    D0 0E       BNE $486E       ;
4860    BD 20 EE    LDA $EE20,X     ;
4863    C5 00       CMP $00         ;
4865    D0 07       BNE 486E$       ;
4867    BD 40 EE    LDA $EE40,X     ;
486A    C5 01       CMP $01         ;
486C    F0 07       BEQ $4875       ;
486E    CA          DEX             ;
486F    E0 08       CPX #$08        ;
4871    B0 E4       BCS $4857       ;
4873    90 08       BCC $487D       ;
4875    A9 00       LDA #$00        ;
4877    9D 60 EE    STA $EE60,X     ;
487A    9D 00 EE    STA $EE00,X     ;
487D    A5 C8       LDA $C8         ; set $0E = $C8
487F    85 0E       STA $0E         ; .
4881    60          RTS             ;

;; 'C' handler (Cast)

4882    20 21 08

4885    C3 E1 F3 F4 A0 F3 F0 Cast sp
488C    E5 EC EC BA 8D F0 EC E1 F9 E5 F2 AD 00 20 5D 08 ell:.player-. ].
489C    D0 03 4C 63 41 C5 0F F0 02 B0 F7 85 D4 20 A9 7E P.LcAE.p.0w.T )~
48AC    10 03 4C 0C 42 20 0B 63 20 21 08 D3 D0 C5 CC CC ..L.B .c !.SPELL
48BC    AD 00 20 9A 84 48 20 69 08 20 45 08 A5 0B 10 03 -. ..H i. E.%...
48CC    20 01 85 68 38 E9 C1 C9 1A 90 03 4C 51 41 8D 37  ..h8iAI...LQA.7
48DC    49 18 69 65 20 7E 08 20 21 08 A1 8D 00 AC 37 49 I.ie ~. !.!..,7I
48EC    B9 40 ED D0 03 4C F6 41 F8 38 E9 01 99 40 ED D8 9@mP.LvAx8i..@mX
48FC    20 2D 08 A0 16 B1 FE AE 37 49 F8 38 FD 6C 49 D8  -. .1~.7Ix8}lIX
490C    B0 15 20 21 08 CD AE D0 AE A0 D4 CF CF A0 CC CF 0. !.M.P. TOO LO
491C    D7 A1 8D 00 4C 01 4F 91 FE AD 37 49 0A A8 B9 38 W!..L.O.~-7I.(98
492C    49 85 FE B9 39 49 85 FF 6C FE 00 00 86 49 B7 49 I.~99I..l~...I7I
493C    1F 4A 44 4A D5 4A 59 4B CC 4B 1F 4C 48 4C 61 4C .JDJUJYKLK.LHLaL
494C    6F 4C 88 4C 9B 4C B4 4C BC 4C CD 4C D5 4C DD 4C oL.L.L4L<LMLUL]L
495C    0C 4D 35 4D 90 4D B0 4D C0 4D F1 4D FA 4D 35 4E .M5M.M0M@MqMzM5N
496C    05 15 05 20 10 15 40 10 20 30 25 05 05 20 05 15 ... ..@. 0%.. ..
497C    20 45 15 30 15 15 10 15 10 05 20 21 08 D7 C8 CF  E.0...... !.WHO
498C    AD 00 20 5D 08 D0 03 4C 01 4F 20 D8 4E 20 2D 08 -. ].P.L.O XN -.
499C    A0 12 B1 FE C9 D3 D0 EF A9 C7 91 FE A5 0B 10 08  .1~ISPo)G.~%...
49AC    20 DC 7E A6 D4 9D 9F EF 4C E6 41 A5 0E C9 14 B0  \~&T..oLfA%.I.0
49BC    03 4C 01 4F 20 B9 4E 20 2B 4F 20 96 83 20 D8 4E .L.O 9N +O .. XN
49CC    A5 00 25 01 C9 C0 90 03 4C 01 4F 20 FB 49 C9 FF %.%.I@..L.O {II.
49DC    D0 F9 20 0D 4A C9 FF F0 D8 20 48 08 30 F4 A5 FA Py .JI.pX H.0t%z
49EC    85 00 A5 FB 85 01 20 23 03 20 03 08 4C E6 41 18 ..%{.. #. ..LfA.
49FC    A5 FA 65 F8 85 FA 18 A5 FB 65 F9 85 FB 20 81 08 %zex.z.%{ey.{ ..
4A0C    60 38 A5 FA E5 F8 85 FA 38 A5 FB E5 F9 85 FB 20 `8%zex.z8%{ey.{
4A1C    81 08 60 20 21 08 D7 C8 B0 AD 00 20 5D 08 D0 03 ..` !.WH0-. ].P.
4A2C    4C 01 4F 20 D8 4E 20 2D 08 A0 12 B1 FE C9 D0 D0 L.O XN -. .1~IPP
4A3C    EF A9 C7 91 FE 4C E6 41 A5 0B C9 03 F0 66 20 2B o)G.~LfA%.I.pf +
4A4C    4F A5 0B 30 31 C9 01 D0 1F F0 03 4C 01 4F 20 96 O%.01I.P.p.L.O .
4A5C    83 F0 F8 20 D8 4E 20 81 08 C9 44 90 EE C9 48 B0 .px XN ..ID.nIH0
4A6C    EA A5 C8 91 FC 4C E6 41 20 96 83 F0 DE 20 D8 4E j%H.|LfA ..p^ XN
4A7C    20 93 08 4C 65 4A 20 81 83 F0 D0 20 D8 4E 20 8D  ..LeJ ..pP XN .
4A8C    08 85 C8 18 A5 06 65 F8 85 06 18 A5 07 65 F9 85 ..H.%.ex...%.ey.
4A9C    07 20 8D 08 C9 44 90 B3 C9 48 B0 AF A5 C8 99 80 . ..ID.3IH0/%H..
4AAC    02 4C E6 41 20 D8 4E 18 A6 10 A5 00 7D 5B 43 85 .LfA XN.&.%.}[C.
4ABC    06 18 A5 01 7D 5F 43 85 07 20 96 08 29 F0 C9 A0 ..%.}_C.. ..)pI
4ACC    D0 89 A9 00 91 FE 4C E6 41 20 21 08 D4 D9 D0 C5 P.)..~LfA !.TYPE
4ADC    AD 00 20 9A 84 A2 44 C9 D0 F0 12 E8 C9 CC F0 0D -. .."DIPp.hILp.
4AEC    E8 C9 C6 F0 08 E8 C9 D3 F0 03 4C 01 4F 86 EA A5 hIFp.hISp.L.O.j%
4AFC    0B 30 29 C9 03 D0 F3 20 D8 4E A6 10 18 A5 00 7D .0)I.Ps XN&..%.}
4B0C    5B 43 85 06 18 A5 01 7D 5F 43 85 07 20 96 08 D0 [C...%.}_C.. ..P
4B1C    D9 A5 EA 29 03 09 A0 91 FE 4C E6 41 20 2B 4F 20 Y%j).. .~LfA +O
4B2C    81 83 F0 C6 18 A5 06 65 F8 85 06 C9 0B B0 BB 18 ..pF.%.ex..I.0;.
4B3C    A5 07 65 F9 85 07 C9 0B B0 B0 20 D8 4E 20 8D 08 %.ey..I.00 XN ..
4B4C    20 48 08 30 A5 A5 EA 99 80 02 4C E6 41 20 9E 4E  H.0%%j...LfA .N
4B5C    20 2B 4F 20 81 83 D0 08 A9 00 8D FD EF 4C 01 4F  +O ..P.)..}oL.O
4B6C    20 D8 4E A9 4F 8D FD EF A5 06 8D FE EF A5 07 8D  XN)O.}o%..~o%..
4B7C    FF EF 20 1A 83 30 E1 20 57 08 20 FC 82 30 F3 A9 .o ..0a W. |.0s)
4B8C    06 20 54 08 AD FD EF C9 4D F0 21 C9 4E F0 13 C9 . T.-}oIMp!INp.I
4B9C    4F F0 05 A9 E8 4C BF 4B A9 80 20 FD 7E 09 18 4C Op.)hL?K). }~..L
4BAC    BF 4B A9 E0 20 FD 7E 09 20 4C BF 4B A9 40 20 FD ?K)` }~. L?K)@ }
4BBC    7E 09 10 A2 00 8E FD EF 85 DC 20 03 7C 4C E6 41 ~.."..}o.\ .|LfA
4BCC    A5 0E C9 14 90 20 A5 0B C9 01 F0 03 4C B9 4E 20 %.I.. %.I.p.L9N
4BDC    21 08 D4 CF A0 D0 C8 C1 D3 C5 AD 00 20 9A 84 38 !.TO PHASE-. ..8
4BEC    E9 B1 C9 08 90 03 4C 01 4F 85 EA 20 D8 4E A6 EA i1I...L.O.j XN&j
4BFC    BD 0F 4C 85 00 BD 17 4C 85 01 20 23 03 20 03 08 =.L..=.L.. #. ..
4C0C    4C E6 41 E0 60 26 32 A6 68 17 BB 85 66 E0 25 13 LfA``&2&h.;.f`%.
4C1C    C2 7E A7 20 21 08 D7 C8 CF AD 00 20 5D 08 D0 03 B~' !.WHO-. ].P.
4C2C    4C 01 4F 20 D8 4E 20 BB 7E 30 F5 A9 19 20 FD 7E L.O XN ;~0u). }~
4C3C    18 69 4B 20 28 85 20 F8 85 4C E6 41 20 9E 4E 20 .iK (. x.LfA .N
4C4C    2B 4F 20 81 83 D0 03 4C 01 4F 20 D8 4E A9 4E 8D +O ..P.L.O XN)N.
4C5C    FD EF 4C 74 4B 20 D8 4E A9 CA 85 C6 A9 0A 85 C7 }oLtK XN)J.F)..G
4C6C    4C E6 41 20 9E 4E 20 2B 4F 20 81 83 D0 03 4C 01 LfA .N +O ..P.L.
4C7C    4F 20 D8 4E A9 8C 8D FD EF 4C 74 4B 20 D8 4E A9 O XN)..}oLtK XN)
4C8C    64 85 11 A5 0B C9 03 D0 03 20 06 8C 4C E6 41 20 d..%.I.P. ..LfA
4C9C    9E 4E 20 2B 4F 20 81 83 D0 03 4C 01 4F 20 D8 4E .N +O ..P.L.O XN
4CAC    A9 4D 8D FD EF 4C 74 4B 20 D8 4E A9 CE 4C 66 4C )M.}oLtK XN)NLfL
4CBC    20 D8 4E A5 0B 30 07 A9 00 85 D4 4C CD 53 4C 8D  XN%.0.)..TLMSL.
4CCC    7D 20 D8 4E A9 D0 4C 66 4C 20 D8 4E A9 D1 4C 66 } XN)PLfL XN)QLf
4CDC    4C A5 0B 10 03 4C 01 4F 20 21 08 D7 C8 CF AD 00 L%...L.O !.WHO-.
4CEC    20 5D 08 D0 03 4C 01 4F 20 D8 4E 20 2D 08 A0 12  ].P.L.O XN -. .
4CFC    B1 FE C9 C4 F0 03 4C 01 4F A9 C7 91 FE 4C E6 41 1~IDp.L.O)G.~LfA
4D0C    20 9E 4E 20 D8 4E A2 0F BD 50 EF F0 16 20 15 4F  .N XN".=Pop. .O
4D1C    F0 11 C9 FC F0 0D 20 4E 08 DD 40 EF 90 05 A9 01 p.I|p. N.]@o..).
4D2C    9D 70 EF CA 10 E2 4C E6 41 20 9E 4E 20 D8 4E 20 .poJ.bLfA .N XN
4D3C    0B 87 A2 0F BD 50 EF F0 45 C9 5E F0 41 BD 40 EF ..".=PopEI^pA=@o
4D4C    C9 C0 B0 3A 20 4E 08 30 0C 29 01 D0 31 A9 17 9D I@0: N.0.).P1)..
4D5C    40 EF 4C 8A 4D 86 EA BD 00 EF 8D FE EF BD 10 EF @oL.M.j=.o.~o=.o
4D6C    8D FF EF A9 4F 8D FD EF 20 57 08 A9 06 20 54 08 ..o)O.}o W.). T.
4D7C    A9 00 8D FD EF A9 FF 85 DC 20 03 7C A6 EA CA 10 )..}o)..\ .|&jJ.
4D8C    B3 4C E6 41 20 9E 4E 20 D8 4E A2 0F BD 50 EF 20 3LfA .N XN".=Po
4D9C    15 4F D0 0A 20 4E 08 30 05 A9 17 9D 40 EF CA 10 .OP. N.0.)..@oJ.
4DAC    EB 4C E6 41 A5 0B 10 03 4C E1 4C 20 D8 4E 20 9B kLfA%...LaL XN .
4DBC    58 4C E6 41 20 B9 4E 20 2B 4F 20 96 83 D0 03 4C XLfA 9N +O ..P.L
4DCC    01 4F 20 D8 4E A5 F8 D0 0E A5 F9 10 05 A9 01 4C .O XN%xP.%y..).L
4DDC    EC 4D A9 03 4C EC 4D 10 05 A9 00 4C EC 4D A9 02 lM).LlM..).LlM).
4DEC    85 F5 4C E6 41 20 80 4E 20 D8 4E 4C 4C 46 20 80 .uLfA .N XNLLF .
4DFC    4E 20 D8 4E 20 74 4E C6 0C 10 03 4C 4C 46 A9 20 N XN tNF...LLF)
4E0C    85 F0 20 4E 08 29 07 85 06 20 4E 08 29 07 85 07 .p N.)... N.)...
4E1C    20 96 08 F0 09 C6 F0 D0 E9 E6 0C 4C 01 4F A5 06  ..p.FpPif.L.O%.
4E2C    85 00 A5 07 85 01 4C E6 41 20 80 4E 20 D8 4E 20 ..%...LfA .N XN
4E3C    74 4E A5 0C C9 07 90 03 20 01 4F E6 0C A9 20 85 tN%.I... .Of.) .
4E4C    F0 20 4E 08 29 07 85 06 20 4E 08 29 07 85 07 20 p N.)... N.)...
4E5C    96 08 F0 09 C6 F0 D0 E9 C6 0C 4C 01 4F A5 06 85 ..p.FpPiF.L.O%..
4E6C    00 A5 07 85 01 4C E6 41 A5 0A C9 18 D0 05 68 68 .%...LfA%.I.P.hh
4E7C    4C 01 4F 60 A5 0B C9 03 F0 17 20 21 08 C4 D5 CE L.O`%.I.p. !.DUN
4E8C    C7 C5 CF CE A0 CF CE CC D9 A1 8D 00 68 68 4C 01 GEON ONLY!..hhL.
4E9C    4F 60 A5 0B 30 16 20 21 08 C3 CF CD C2 C1 D4 A0 O`%.0. !.COMBAT
4EAC    CF CE CC D9 A1 8D 00 68 68 4C 01 4F 60 A5 0B C9 ONLY!..hhL.O`%.I
4EBC    01 F0 18 20 21 08 CF D5 D4 C4 CF CF D2 D3 A0 CF .p. !.OUTDOORS O
4ECC    CE CC D9 A1 8D 00 68 68 4C 01 4F 60 AC 37 49 BE NLY!..hhL.O`,7I>
4EDC    6C 49 A9 0A 20 54 08 20 78 08 AD 37 49 18 69 60 lI). T. x.-7I.i`
4EEC    AA A9 09 20 54 08 20 78 08 A5 C6 C9 CE D0 05 68 *). T. x.%FINP.h
4EFC    68 4C 01 4F 60 20 21 08 C6 C1 C9 CC C5 C4 A1 8D hL.O` !.FAILED!.
4F0C    00 A9 08 20 54 08 4C 84 63 C9 9C F0 0F C9 BC F0 .). T.L.cI.p.I<p
4F1C    0B C9 C4 F0 07 C9 E4 F0 03 A9 FF 60 A9 00 60 20 .IDp.Idp.).`).`
4F2C    21 08 C4 C9 D2 C5 C3 D4 C9 CF CE AD 00 60       !.DIRECTION-.`

;; 'D' handler (Descend)

4F3A    A5 0E C9 18 F0 4D 20 21 08

4F43    C4 E5 F3 E3 E5 EE E4 A0 00                      Descend .
4F4C    A5 0A C9 01 F0 61 A5 0B C9 03 F0 03 4C 51 41 A5 %.I.pa%.I.p.LQA%
4F5C    C8 29 F0 C9 20 F0 07 C9 30 F0 03 4C 51 41 E6 0C H)pI p.I0p.LQAf.
4F6C    20 21 08 E4 EF F7 EE A1 8D D4 EF A0 EC E5 F6 E5  !.down!.To leve
4F7C    EC A0 00 18 A5 0C 69 01 20 66 08 20 05 87 4C 84 l ..%.i. f. ..L.
4F8C    63 20 21 08 CC E1 EE E4 A0 C2 E1 EC EC EF EF EE c !.Land Balloon
4F9C    8D 00 A5 C8 C9 04 D0 09 A9 00 85 F4 85 0D 4C 84 ..%HI.P.)..t..L.
4FAC    63 4C B7 41 4C 51 41 A5 0E C9 1F F0 03 4C 78 41 cL7ALQA%.I.p.LxA
4FBC    A5 C8 C9 1C D0 EE A5 01 C9 02 F0 26 20 21 08 F4 %HI.Pn%.I.p& !.t
4FCC    EF 8D E6 E9 F2 F3 F4 A0 E6 EC EF EF F2 A1 8D 00 o.first floor!..
4FDC    A9 00 20 20 03 A9 C1 20 91 52 A9 01 20 20 03 4C ).  .)A .R).  .L
4FEC    84 63 20 21 08 E9 EE F4 EF 8D F4 E8 E5 A0 E4 E5 .c !.into.the de
4FFC    F0 F4 E8 F3 A1 8D 00 A9 05 85 FA 85 FB A9 EF 85 pths!..)..z.{)o.
500C    08 A9 F0 85 09 A9 17 85 0A 4C C5 50

;; 'E' handler (Enter)

5018    20 21 08

501B    C5                                              E
501C    EE F4 E5 F2 A0 00 A5 0A F0 03 4C 51 41 A2 20 CA nter .%.p.LQA" J
502C    10 03 4C 51 41 BD 12 52 C5 00 D0 F3 BD 32 52 C5 ..LQA=.RE.Ps=2RE
503C    01 D0 EC 86 0A E6 0A A5 00 85 08 A5 01 85 09 A5 .Pl..f.%...%...%
504C    C8 C9 09 F0 49 C9 0A D0 03 4C 3B 51 C9 0B D0 03 HI.pII.P.L;QI.P.
505C    4C A3 51 C9 0C D0 03 4C 93 51 C9 0E D0 03 4C A3 L#QI.P.L.QI.P.L#
506C    51 C9 1D D0 03 4C 2E 51 C9 1E D0 03 4C CE 51 C9 QI.P.L.QI.P.LNQI
507C    46 F0 07 A9 00 85 0A 4C 51 41 AD 0E ED C9 77 F0 Fp.)...LQA-.mIwp
508C    07 A9 00 85 0A 4C C8 41 20 52 52 4C AA 50 20 21 .)...LHA RRL*P !
509C    08 E4 F5 EE E7 E5 EF EE A1 8D 00 20 52 52 A5 0E .dungeon!.. RR%.
50AC    C9 1F F0 07 A9 00 85 0A 4C 78 41 A9 01 85 FA 85 I.p.)...LxA)..z.
50BC    FB A9 00 20 20 03 20 61 52 A9 00 20 20 03 A9 04 {).  . aR).  .).
50CC    20 42 08 20 1B 08 84 C2 CC CF C1 C4 A0 C4 CE C7  B. ...BLOAD DNG
50DC    C4 AC C1 A4 B8 C3 B0 B0 8D 00 20 05 51 A9 03 85 D,A$8C00.. .Q)..
50EC    0B A9 01 85 10 A5 FA 85 00 A5 FB 85 01 A9 00 85 .)...%z..%{..)..
50FC    0C A9 C4 20 20 03 4C 84 63 18 A5 0A 69 9F 8D 1A .)D  .L.c.%.i...
510C    51 20 1B 08 84 C2 CC CF C1 C4 A0 C4 CE C7 C0 AC Q ...BLOAD DNG@,
511C    C1 A4 C5 B8 B0 B0 8D 00 A2 00 8A 9D 00 EE E8 D0 A$E800.."....nhP
512C    FA 60 20 21 08 F2 F5 E9 EE A1 8D 00 4C 46 51 20 z` !.ruin!..LFQ
513C    21 08 F4 EF F7 EE E5 A1 8D 00 A9 00 20 20 03 20 !.towne!..).  .
514C    52 52 20 84 52 A9 0F 85 01 A9 01 85 00 A5 0A 38 RR .R)...)...%.8
515C    E9 05 C9 08 B0 29 85 D8 A5 0F 85 D4 A5 D4 C9 02 i.I.0).X%..T%TI.
516C    90 1D 20 2D 08 A0 11 B1 FE C5 D8 F0 05 C6 D4 4C .. -. .1~EXp.FTL
517C    68 51 A9 00 A2 1F 9D 60 EE 9D 00 EE 9D C0 EE A9 hQ)."..`n..n.@n)
518C    D4 20 20 03 4C 84 63 20 21 08 F6 E9 EC EC E1 E7 T  .L.c !.villag
519C    E5 A1 8D 00 4C 46 51 A9 00 20 20 03 20 21 08 E3 e!..LFQ).  . !.c
51AC    E1 F3 F4 EC E5 A1 8D 00 20 52 52 20 84 52 A9 0F astle!.. RR .R).
51BC    85 00 A9 1E 85 01 A9 00 85 0C A9 C3 20 20 03 4C ..)...)...)C  .L
51CC    84 63 20 21 08 F4 E8 E5 8D F3 E8 F2 E9 EE E5 A0 .c !.the.shrine
51DC    EF E6 8D 00 A5 0A 18 69 7E 20 7E 08 20 05 87 A9 of..%..i~ ~. ..)
51EC    00 20 20 03 20 1B 08 84 C2 CC CF C1 C4 A0 D3 C8 .  . ...BLOAD SH
51FC    D2 CE AC C1 A4 B8 B8 B0 B0 8D 00 20 00 88 A9 01 RN,A$8800.. ..).
520C    20 20 03 4C 84 63 56 DA 1C 92 E8 52 24 3A 9F 6A   .L.cVZ..hR$:.j
521C    16 BB 62 88 C9 88 F0 5B 48 7E 9C 3A EF E9 E9 80 .;b.I.p[H~.:oii.
522C    24 49 CD 51 E7 E7 6B 6B 32 F1 87 6A DE 2B 14 B8 $IMQggkk2q.j^+.8
523C    80 A9 91 9E 3B 5A 49 43 A8 14 1B 66 F0 E9 42 5C .)..;ZIC(..fpiB\
524C    E5 0B 2D CF D8 D8 20 05 87 A5 0A 18 69 7E 20 7B e.-OXX ..%..i~ {
525C    08 20 05 87 60 A9 02 20 42 08 20 1B 08 84 C2 D3 . ..`). B. ...BS
526C    C1 D6 C5 A0 D4 CC D3 D4 AC C1 A4 C5 C5 B0 B0 AC AVE TLST,A$EE00,
527C    CC A4 B1 B0 B0 8D 00 60 20 61 52 A9 03 20 42 08 L$100..` aR). B.
528C    A5 0A 18 69 C0 8D A2 52 20 1B 08 8D 84 C2 CC CF %..i@."R ....BLO
529C    C1 C4 A0 CD C1 D0 C0 AC C1 A4 B8 C2 B0 B0 8D 00 AD MAP@,A$8B00..
52AC    A2 00 BD 00 8B 9D 00 E8 BD 00 8C 9D 00 E9 BD 00 ".=....h=....i=.
52BC    8D 9D 00 EA BD 00 8E 9D 00 EB BD 00 8F 9D 00 EE ...j=....k=....n
52CC    E8 D0 DF A9 02 85 0B 60 4C 51 41

;; 'F' handler (Fire)

52D7    20 21 08

52DA    C6 E9                                           Fi
52DC    F2 E5 AD 00 A5 0E C9 10 90 EE C9 14 B0 EA 20 96 re-.%.I..nI.0j .
52EC    83 D0 03 4C 45 41 A5 F8 F0 27 A5 0E C9 11 F0 2E .P.LEA%xp'%.I.p.
52FC    C9 13 F0 2A 20 21 08 CF CE CC D9 A0 C2 D2 CF C1 I.p* !.ONLY BROA
530C    C4 D3 C9 C4 C5 D3 A1 8D 00 A9 02 20 54 08 4C 84 DSIDES!..). T.L.
531C    63 A5 0E C9 10 F0 07 C9 12 F0 03 4C 00 53 A9 03 c%.I.p.I.p.L.S).
532C    20 54 08 A9 03 85 EA 20 3C 84 10 26 20 87 08 48  T.)..j <..& ..H
533C    A9 4D 91 FE 20 63 08 20 87 08 68 91 FE 18 A5 FA )M.~ c. ..h.~.%z
534C    65 F8 85 FA 18 A5 FB 65 F9 85 FB C6 EA D0 D8 4C ex.z.%{ey.{FjPXL
535C    84 63 86 D9 20 87 08 48 A9 4F 91 FE 20 63 08 A9 .c.Y ..H)O.~ c.)
536C    06 20 54 08 20 87 08 68 91 FE A5 D9 C9 08 B0 07 . T. ..h.~%YI.0.
537C    20 4E 08 29 03 D0 11 A9 00 A6 D9 BC 60 EE C0 5E  N.).P.).&Y<`n@^
538C    F0 06 9D 60 EE 9D 00 EE 4C 84 63

;; 'G' handler (Get chest)

5397    20 21 08

539A    C7 E5                                           Ge
539C    F4 A0 E3 E8 E5 F3 F4 AC A0 F7 E8 EF 8D F7 E9 EC t chest, who.wil
53AC    EC A0 EF F0 E5 EE AD 00 20 5D 08 D0 03 4C D6 41 l open-. ].P.LVA
53BC    C5 0F 90 05 F0 03 4C 63 41 20 A9 7E F0 03 4C 0C E...p.LcA )~p.L.
53CC    42 A5 0B C9 03 F0 57 A5 C8 C9 3C F0 03 4C B7 41 B%.I.pW%HI<p.L7A
53DC    A5 0A F0 23 20 90 08 C9 3C D0 1C A9 3E 91 FC A0 %.p# ..I<P.)>.|
53EC    00 A9 01 20 80 85 A0 03 A9 01 20 80 85 A0 05 A9 .). .. .). .. .)
53FC    01 20 80 85 4C 27 54 A2 1F BD 60 EE C9 3C D0 0E . ..L'T".=`nI<P.
540C    BD 20 EE C5 00 D0 07 BD 40 EE C5 01 F0 05 CA 10 = nE.P.=@nE.p.J.
541C    E8 30 08 A9 00 9D 60 EE 9D 00 EE 4C 42 54 A5 C8 h0.)..`n..nLBT%H
542C    C9 40 F0 03 4C B7 41 A5 00 85 06 A5 01 85 07 20 I@p.L7A%...%...
543C    96 08 A9 00 91 FE 20 4E 08 10 03 4C 04 55 29 03 ..)..~ N...L.U).
544C    85 DA 20 4E 08 25 DA 85 DA D0 0B 20 21 08 C1 C3 .Z N.%Z.ZP. !.AC
545C    C9 C4 00 4C 8B 54 C9 01 D0 0C 20 21 08 D3 CC C5 ID.L.TI.P. !.SLE
546C    C5 D0 00 4C 8B 54 C9 02 D0 0D 20 21 08 D0 CF C9 EP.L.TI.P. !.POI
547C    D3 CF CE 00 4C 8B 54 20 21 08 C2 CF CD C2 00 20 SON.L.T !.BOMB.
548C    21 08 A0 D4 D2 C1 D0 A1 8D 00 A5 D4 F0 18 20 2D !. TRAP!..%Tp. -
549C    08 A0 14 B1 FE 20 40 85 18 69 19 85 D9 A9 64 20 . .1~ @..i..Y)d
54AC    FD 7E C5 D9 B0 14 20 21 08 C5 D6 C1 C4 C5 C4 A1 }~EY0. !.EVADED!
54BC    8D 00 A9 08 20 54 08 4C 04 55 A5 DA D0 06 20 79 ..). T.L.U%ZP. y
54CC    86 4C 04 55 C9 01 D0 1A 20 2D 08 A0 12 A9 D3 91 .L.UI.P. -. .)S.
54DC    FE 20 F9 86 A5 0B 10 20 A9 38 A6 D4 9D 9F EF 4C ~ y.%.. )8&T..oL
54EC    04 55 C9 02 D0 0F 20 2D 08 A0 12 A9 D0 91 FE 20 .UI.P. -. .)P.~
54FC    F9 86 4C 04 55 20 88 86 20 21 08 D4 C8 C5 A0 C3 y.L.U .. !.THE C
550C    C8 C5 D3 D4 A0 C8 CF CC C4 D3 BA 8D 00 A9 64 20 HEST HOLDS:..)d
551C    FD 7E 20 28 85 20 34 86 A5 DA 20 33 08 20 21 08 }~ (. 4.%Z 3. !.
552C    AD C7 CF CC C4 A1 8D 00 4C 84 63                -GOLD!..L.c

;; 'H' handler (Hole up and camp)

5537    20 21 08

553A    C8 EF                                           Ho
553C    EC E5 A0 F5 F0 A0 A6 A0 E3 E1 ED F0 8D 00 A5 0A le up & camp..%.
554C    F0 09 A5 0B C9 03 F0 03 4C B7 41 A5 0E C9 1F F0 p.%.I.p.L7A%.I.p
555C    18 20 21 08 CD D5 D3 D4 A0 C2 C5 A0 CF CE A0 C6 . !.MUST BE ON F
556C    CF CF D4 A1 8D 00 4C 84 63 A9 00 20 20 03 20 1B OOT!..L.c).  . .
557C    08 84 C2 CC CF C1 C4 A0 C8 CF CC C5 AC C1 A4 B8 ..BLOAD HOLE,A$8
558C    B8 B0 B0 8D 00 20 00 88 20 45 08 A9 01 20 20 03 800.. .. E.).  .
559C    A5 0B C9 03 D0 03 20 06 8C 4C 84 63             %.I.P. ..L.c

;; 'I' handler (Ignite torch)

55A8    20 21 08

55AB    C9                                              I
55AC    E7 EE E9 F4 E5 A0 F4 EF F2 E3 E8 A1 8D 00 A0 08 gnite torch!.. .
55BC    20 5E 85 B0 03 4C F6 41 A9 64 85 11 A5 0B C9 03  ^.0.LvA)d..%.I.
55CC    D0 03 20 06 8C 4C 84 63                         P. ..L.c

;; 'J' handler (Jimmy lock)

55D4    20 21 08

55D7    CA E9 ED ED F9                                  Jimmy
55DC    A0 EC EF E3 EB AD 00 20 96 83 A5 0B C9 01 D0 03  lock-. ..%.I.P.
55EC    4C B7 41 20 93 08 C9 3A F0 03 4C B7 41 A0 0A 20 L7A ..I:p.L7A .
55FC    5E 85 B0 03 4C F6 41 A9 3B A0 00 91 FC 4C E6 41 ^.0.LvA); ..|LfA

;; 'K' handler (Klimb)

560C    20 21 08

560F    CB EC E9 ED E2 A0 00 A5 0A F0 43 C9 01          Klimb .%.pCI.
561C    F0 5B A5 0B C9 03 F0 03 4C 51 41 A5 C8 29 F0 C9 p[%.I.p.LQA%H)pI
562C    10 F0 04 C9 30 D0 F1 20 21 08 F5 F0 A1 8D 00 C6 .p.I0Pq !.up!..F
563C    0C 10 03 4C 4C 46 20 21 08 D4 EF A0 EC E5 F6 E5 ...LLF !.To leve
564C    EC A0 00 18 A5 0C 69 01 20 66 08 20 05 87 4C 84 l ..%.i. f. ..L.
565C    63 A5 0E C9 18 D0 C1 20 21 08 E1 EC F4 E9 F4 F5 c%.I.PA !.altitu
566C    E4 E5 8D 00 A9 FF 85 F4 85 0D 4C 84 63 A5 C8 C9 de..)..t..L.c%HI
567C    1B D0 A5 A5 0E C9 1F F0 06 20 05 87 4C 78 41 20 .P%%.I.p. ..LxA
568C    21 08 F4 EF 8D F3 E5 E3 EF EE E4 A0 E6 EC EF EF !.to.second floo
569C    F2 A1 8D 00 A9 00 20 20 03 A9 C0 20 91 52 A9 01 r!..).  .)@ .R).
56AC    20 20 03 4C 84 63

;; 'L' handler (Locate position)

56B2    20 21 08

56B5    CC EF E3 E1 F4 E5 A0                            Locate
56BC    F0 EF F3 E9 F4 E9 EF EE 8D 00 AD 0B ED D0 0C 20 position..-.mP.
56CC    21 08 D7 C9 D4 C8 A0 00 4C 51 41 20 21 08 F7 E9 !.WITH .LQA !.wi
56DC    F4 E8 A0 F3 E5 F8 F4 E1 EE F4 BA 8D 8D A0 EC E1 th sextant:.. la
56EC    F4 E9 F4 F5 E4 E5 BA 00 A5 01 4A 4A 4A 4A 18 69 titude:.%.JJJJ.i
56FC    C1 20 24 08 A9 A7 20 24 08 A5 01 29 0F 18 69 C1 A $.)' $.%.)..iA
570C    20 24 08 20 21 08 A2 8D EC EF EE E7 E9 F4 F5 E4  $. !.".longitud
571C    E5 BA 00 A5 00 4A 4A 4A 4A 18 69 C1 20 24 08 A9 e:.%.JJJJ.iA $.)
572C    A7 20 24 08 A5 00 29 0F 18 69 C1 20 24 08 A9 A2 ' $.%.)..iA $.)"
573C    20 24 08 20 05 87 4C 84 63

;; 'M' handler (Mix reagents)

5745    20 0B 63 20 21 08

564B    CD                                              M
574C    E9 F8 A0 F2 E5 E1 E7 E5 EE F4 F3 8D 00 A9 00 20 ix reagents..).
575C    20 03 20 1B 08 84 C2 CC CF C1 C4 A0 CD C9 D8 AC  . ...BLOAD MIX,
576C    C1 A4 B8 B8 B0 B0 8D 00 20 00 88 A9 01 20 20 03 A$8800.. ..).  .
577C    4C 84 63

;; 'N' handler (New order)

577F    20 21 08

5782    CE E5 F7 A0 EF F2 E4 E5 F2 BA                   New order:
578C    8D E5 F8 E3 E8 E1 EE E7 E5 A0 A3 00 20 5D 08 D0 .exchange #. ].P
579C    03 4C D6 41 C9 01 F0 67 C5 0F 90 05 F0 03 4C B7 .LVAI.pgE...p.L7
57AC    41 85 D5 20 2D 08 A5 FE 8D 25 58 A5 FF 8D 26 58 A.U -.%~.%X%..&X
57BC    20 21 08 A0 A0 A0 A0 F7 E9 F4 E8 A0 A3 00 20 5D  !.    with #. ]
57CC    08 D0 03 4C D6 41 C9 01 F0 35 C5 D5 D0 03 4C E6 .P.LVAI.p5EUP.Lf
57DC    41 C5 0F 90 05 F0 03 4C B7 41 20 2D 08 A0 1F AD AE...p.L7A -. .-
57EC    25 58 85 FC AD 26 58 85 FD B1 FE 48 B1 FC 91 FE %X.|-&X.}1~H1|.~
57FC    68 91 FC 88 10 F3 20 69 08 20 45 08 4C E6 41 20 h.|..s i. E.LfA
580C    30 08 20 21 08 8D D9 CF D5 A0 CD D5 D3 D4 A0 CC 0. !..YOU MUST L
581C    C5 C1 C4 A1 8D 00 4C D6 41 00 00                EAD!..LVA..

;; 'O' handler (Open)

5827    20 04 81 AD 2F 81 D0 F8 20 21 08

5832    CF F0 E5 EE AD 00 20 96 83 A5 Open-.
583C    0B C9 02 F0 03 4C B7 41 20 93 08 C9 3B F0 07 C9 .I.p.L7A ..I;p.I
584C    3A D0 F2 4C C8 41 A5 FA 8D 2D 81 A5 FB 8D 2E 81 :PrLHA%z.-.%{...
585C    A9 05 8D 2F 81 A9 3E 91 FC 20 21 08 CF D0 C5 CE )../.)>.| !.OPEN
586C    C5 C4 A1 8D 00 4C 84 63                         ED!..

;; 'P' handler (Peer)

5874    20 21 08

5877    D0 E5 E5 F2 A0                                  Peer
587C    E1 F4 A0 00 A0 09 20 5E 85 B0 03 4C 51 41 20 21 at . . ^.0.LQA !
588C    08 E1 A0 E7 E5 ED A1 8D 00 20 9B 58 4C 84 63 A9 .a gem!.. .XL.c)
589C    00 20 20 03 20 1B 08 84 C2 CC CF C1 C4 A0 D4 CD .  . ...BLOAD TM
58AC    C1 D0 AC C1 A4 B9 B0 B0 B0 8D 00 A9 03 20 20 03 AP,A$9000..).  .
58BC    20 00 90 A5 0B C9 03 D0 1C A9 00 20 20 03 20 1B  ..%.I.P.).  . .
58CC    08 84 C2 CC CF C1 C4 A0 C4 CE C7 C4 AC C1 A4 B8 ..BLOAD DNGD,A$8
58DC    C3 B0 B0 8D 00 A9 01 20 20 03 60                C00..).  .`

;; 'Q' handler (Quit & save)

58E7    20 21 08

58EA    D1 F5                                           Qu
58EC    E9 F4 A0 A6 A0 F3 E1 F6 E5 AE AE AE 8D 00 20 61 it & save..... a
58FC    59 A5 0A F0 03 4C 51 41 A9 00 20 20 03 A9 02 20 Y%.p.LQA).  .).
590C    42 08 20 1B 08 84 C2 D3 C1 D6 C5 A0 CC C9 D3 D4 B. ...BSAVE LIST
591C    AC C1 A4 C5 C5 B0 B0 AC CC A4 B1 B0 B0 8D 84 C2 ,A$EE00,L$100..B
592C    D3 C1 D6 C5 A0 D2 CF D3 D4 AC C1 A4 C5 C3 B0 B0 SAVE ROST,A$EC00
593C    AC CC A4 B2 B0 B0 8D 84 C2 D3 C1 D6 C5 A0 D0 D2 ,L$200..BSAVE PR
594C    D4 D9 AC C1 A4 B0 AC CC A4 B2 B0 8D 00 A9 01 20 TY,A$0,L$20..).
595C    20 03 4C 84 63 A5 1C C9 10 B0 22 C9 00 D0 23 A5  .L.c%.I.0"I.P#%
596C    1D C9 10 B0 22 C9 00 D0 23 A5 1E C9 10 B0 22 C9 .I.0"I.P#%.I.0"I
597C    00 D0 23 A5 1F C9 10 B0 22 C9 00 D0 23 A5 1C 20 .P#%.I.0"I.P#%.
598C    BE 59 A5 1C 20 C6 59 A5 1D 20 BE 59 A5 1D 20 C6 >Y%. FY%. >Y%. F
599C    59 A5 1E 20 BE 59 A5 1E 20 C6 59 A5 1F 20 BE 59 Y%. >Y%. FY%. >Y
59AC    A5 1F 20 C6 59 20 21 08 A0 ED EF F6 E5 F3 A1 8D %. FY !. moves!.
59BC    00 60 4A 4A 4A 4A 20 66 08 60 29 0F 20 66 08 60 .`JJJJ f.`). f.`

;; 'R' handler (Ready a weapon)

59CC    20 21 08
59CF    D2 E5 E1 E4 F9 A0 E1 A0 F7 E5 E1 F0 EF          Ready a weapo
59DC    EE 8D E6 EF F2 A0 F0 EC E1 F9 E5 F2 AD 00 20 5D n.for player-. ]
59EC    08 F0 06 C5 0F 90 05 F0 03 4C 63 41 20 0A 60 20 .p.E...p.LcA .`
59FC    21 08 D7 E5 E1 F0 EF EE BA 00 20 9A 84 48 20 69 !.Weapon:. ..H i
5A0C    08 20 45 08 A5 0B 10 03 20 01 85 68 38 E9 C1 C9 . E.%... ..h8iAI
5A1C    10 90 03 4C D6 41 85 EA C9 00 F0 0C 18 69 20 A8 ...LVA.jI.p..i (
5A2C    B9 00 ED D0 03 4C F6 41 20 2D 08 A0 17 B1 FE A6 9.mP.LvA -. .1~&
5A3C    EA 3D 6F 5A D0 03 4C 7F 5A A0 1E B1 FE F0 07 18 j=oZP.L.Z .1~p..
5A4C    69 20 A8 20 50 85 A0 1E A5 EA 91 FE F0 07 18 69 i ( P. .%j.~p..i
5A5C    20 A8 20 5E 85 A5 EA 18 69 25 20 7E 08 20 05 87  ( ^.%j.i% ~. ..
5A6C    4C E6 41 FF FF FF FF 7F 6F 6F 7E 7E FF 2C 0C 2E LfA.....oo~~.,..
5A7C    5E D0 FF 20 05 87 20 21 08 C1 A0 00 20 2D 08 A0 ^P. .. !.A . -.
5A8C    11 B1 FE 18 69 4D 20 7E 08 20 21 08 8D CD C1 D9 .1~.iM ~. !..MAY
5A9C    A0 CE CF D4 A0 D5 D3 C5 A0 C1 8D 00 A5 EA 18 69  NOT USE A..%j.i
5AAC    25 20 7E 08 20 05 87 4C D6 41

;; 'S' handler (Search)

5AB6    20 21 08

5AB9    D3 E5 E1 Sea
5ABC    F2 E3 E8 AE AE AE 8D 00 A9 00 20 20 03 20 1B 08 rch.....).  . ..
5ACC    84 C2 CC CF C1 C4 A0 D3 C5 C1 D2 AC C1 A4 B8 B8 .BLOAD SEAR,A$88
5ADC    B0 B0 8D 00 20 00 88 A9 01 20 20 03 4C 84 63    00.. ..).  .L.

;; 'T' handler (Talk)

5AEB    20 21 08
5AEE    D4 E1 EC EB AD 00 20 96 83 D0 03 4C 45 41       Talk-. ..P.LEA
5AFC    20 93 08 C9 60 90 21 C9 7E B0 1D 18 A5 FA 65 F8  ..I`.!I~0..%zex
5B0C    85 FA 18 A5 FB 65 F9 85 FB 20 2F 84 30 19 BD 60 .z.%{ey.{ /.0.=`
5B1C    EE C9 52 D0 12 4C CE 5B 20 2F 84 30 0A 86 EA A5 nIRP.LN[ /.0..j%
5B2C    0A F0 04 C9 11 90 1B 20 21 08 C6 D5 CE CE D9 AC .p.I... !.FUNNY,
5B3C    A0 CE CF 8D D2 C5 D3 D0 CF CE D3 C5 A1 8D 00 4C  NO.RESPONSE!..L
5B4C    84 63 85 E0 C6 E0 A6 EA BD E0 EE 85 E1 F0 D8 C6 .c.`F`&j=`n.apXF
5B5C    E1 A9 01 85 E2 A9 EF 85 E3 A9 00 20 20 03 A6 EA a)..b)o.c).  .&j
5B6C    BD 60 EE C9 5E D0 36 A9 02 20 42 08 20 1B 08 84 =`nI^P6). B. ...
5B7C    C2 CC CF C1 C4 A0 CC CF D2 C4 AC C1 A4 B8 B8 B0 BLOAD LORD,A$880
5B8C    B0 8D 00 A9 C2 20 20 03 20 07 88 A9 00 20 20 03 0..)B  . ..).  .
5B9C    A9 03 20 42 08 A9 C3 20 20 03 4C C6 5B 20 51 08 ). B.)C  .LF[ Q.
5BAC    20 1B 08 84 C2 CC CF C1 C4 A0 D4 C1 CC CB AC C1  ...BLOAD TALK,A
5BBC    A4 B8 B8 B0 B0 8D 00 20 00 88 A9 01 20 20 03 4C $8800.. ..).  .L
5BCC    84 63 A9 00 20 20 03 20 1B 08 84 C2 CC CF C1 C4 .c).  . ...BLOAD
5BDC    A0 D3 C8 D0 D3 AC C1 A4 B8 B8 B0 B0 8D 00 A9 88  SHPS,A$8800..).
5BEC    85 FF A5 0A 38 E9 01 0A 0A 0A 85 FE A0 07 B1 FE ..%.8i.....~ .1~
5BFC    C5 07 F0 27 88 10 F7 A5 07 C9 18 D0 0A A5 0A C9 E.p'..w%.I.P.%.I
5C0C    0D D0 04 A0 08 D0 14 A5 07 C9 19 D0 0B A5 0A C9 .P. .P.%.I.P.%.I
5C1C    01 D0 05 A0 09 4C 27 5C 4C 33 5B 98 18 69 B0 8D .P. .L'\L3[..i0.
5C2C    3B 5C 20 1B 08 84 C2 CC CF C1 C4 A0 D3 C8 D0 C0 ;\ ...BLOAD SHP@
5C3C    AC C1 A4 B8 B8 B0 B0 8D 00 A9 04 20 20 03 20 00 ,A$8800..).  . .
5C4C    88 A9 01 20 20 03 4C 84 63

;; 'U' handler (Use)

5C55    20 21 08

5C58    D5 F3 E5 AE                                     Use.
5C5C    AE AE 8D 00 A9 00 20 20 03 20 1B 08 84 C2 CC CF ....).  . ...BLO
5C6C    C1 C4 A0 D5 D3 C5 AC C1 A4 B8 B8 B0 B0 8D 00 20 AD USE,A$8800..
5C7C    00 88 A5 0B 30 08 A9 01 20 20 03 4C 84 63 A9 02 ..%.0.).  .L.c).
5C8C    20 20 03 4C 84 63

;; 'V' handler (Volume)

5C92    20 21 08

5C95    D6 EF EC F5 ED E5 A0                            Volume
5C9C    00 A5 CD D0 0E A9 FF 85 CD 20 21 08 CF CE 8D 00 .%MP.)..M !.ON..
5CAC    4C 84 63 A9 00 85 CD 20 21 08 CF C6 C6 8D 00 4C L.c)..M !.OFF..L
5CBC    84 63

;; 'W' handler (Wear armour)

5CBE    20 21 08

5CC1    D7 E5 E1 F2 A0 E1 F2 ED EF F5 F2                Wear armour
5CCC    8D E6 EF F2 A0 F0 EC E1 F9 E5 F2 AD 00 20 5D 08 .for player-. ].
5CDC    F0 06 C5 0F 90 05 F0 03 4C 63 41 20 8C 60 20 21 p.E...p.LcA .` !
5CEC    08 C1 F2 ED EF F5 F2 BA 00 20 9A 84 48 20 69 08 .Armour:. ..H i.
5CFC    20 45 08 A5 0B 10 03 20 01 85 68 38 E9 C1 C9 08  E.%... ..h8iAI.
5D0C    90 03 4C D6 41 85 EA C9 00 F0 0C 18 69 18 A8 B9 ..LVA.jI.p..i.(9
5D1C    00 ED D0 03 4C F6 41 20 2D 08 A0 17 B1 FE A6 EA .mP.LvA -. .1~&j
5D2C    3D 65 5D D0 0A A5 EA 18 69 10 85 EA 4C 7F 5A A0 =e]P.%j.i..jL.Z
5D3C    1F B1 FE F0 07 18 69 18 A8 20 50 85 A0 1F A5 EA .1~p..i.( P. .%j
5D4C    91 FE F0 07 18 69 18 A8 20 5E 85 A5 EA 18 69 35 .~p..i.( ^.%j.i5
5D5C    20 7E 08 20 05 87 4C E6 41 FF FF 7F 2C 2C 24 04  ~. ..LfA...,,$.
5D6C    FF

;; 'X' handler (X-it)

5D6D    20 21 08

5D70    D8 AD E9 F4 A0 00 A5 0E C9 10 90 0C             X-it .%.I...
5D7C    C9 14 90 0B C9 16 90 14 C9 18 F0 14 4C 51 41 A5 I...I...I.p.LQA%
5D8C    00 8D 20 48 A5 01 8D 21 48 4C 9C 5D A9 00 85 14 .. H%..!HL.])...
5D9C    A2 1F BD 60 EE F0 0D CA E0 08 B0 F6 20 4E 08 29 ".=`np.J`.0v N.)
5DAC    0F 09 10 AA A5 0E 9D 60 EE 9D 00 EE A5 00 9D 20 ...*%..`n..n%..
5DBC    EE A5 01 9D 40 EE A9 00 9D C0 EE 9D E0 EE A9 1F n%..@n)..@n.`n).
5DCC    85 0E 20 05 87 4C 84 63


;; 'Y' handler (Yell)

5DD4    20 21 08    JSR $0821       ; print

5DD7    D9 E5 EC EC A0 00                                                       Yell ^@

5DDD    A5 0E       LDA $0E         ; .
5DDF    C9 14       CMP #$14        ; if _0E == 0x14
5DE1    F0 07       BEQ $5DEA       ;   go to $5DEA
5DE3    C9 15       CMP #$15        ; if _0E == 0x15
5DE5    F0 03       BEQ $5DEA       ;   go to $5DEA

5DE7    4C 51 41    JMP $4151       ; otherwise, jump to 'WHAT?'

5DEA    A5 14       LDA $14         ; invert _14
5DEC    49 FF       EOR #$FF        ; .
5DEE    85 14       STA $14         ; .
5DF0    30 0D       BMI #5DFF       ; if high bit, 'giddyup!' otherwise 'whoa!'
5DF2    20 21 08    JSR $0821       ; print

5DF5    F7 E8 EF E1 A1 8D 00                                                    whoa!^M^@

5DFC    4C 84 63    JMP $6384       ; return to main loop

5DFF    20 21 08    JSR $0821       ;

5E02    E7 E9 E4 E4 F9 F5 F0 A1 8D 00                                           giddyup!^M^@

5E0C    4C 84 63    JMP $6384       ; return to main loop


;; 'Z' handler (Zstats)

5E0F    20 21 08

5E12    DA F4 E1 F4 F3 A0 E6 EF F2 AD 00                  Ztats for-.

5E1D    20 5D 08    JSR $085D       ; get player number as input
5E20    D0 03       BNE $5E25       ; if != 0 skip

5E22    4C 96 5F    JMP $5F96       ; but if zero, jump to $5F96

5E25    C5 0F       CMP $0F         ; check again number of players
5E27    90 05       BCC $5E2E       ; if < max number of players, skip
5E29    F0 03       BEQ $5E2E       ; if == max number od players, sip
5E2B    4C 63 41    JMP $4163       ; but if > max, jump to $4163 (print 'NOT A PLAYER!')

;; display player information

5E2E    20 69 08    JSR $0869       ; ???
5E31    20 6C 63    JSR $636C       ; preserve $CE/$CE in $6382/$6383
5E34    A2 1C       LDX #$1C        ; X = 0x1C
5E36    A0 00       LDY #$00        ; Y = 0x00
5E38    20 1E 08    JSR $081E       ; set text col, row to X, Y and print

5E3B    1E D0 CC D2 AD 00                   .PLR-^@

5E41    A5 D4       LDA $D4         ; get current player
5E43    20 66 08    JSR $0866       ; display single digit
5E46    A9 1C       LDA #$1C        ;
5E48    20 24 08    JSR $0824       ; display character
5E4B    A2 18       LDX #$18        ; .
5E4D    86 CE       STX $CE         ; $CE = 0x18
5E4F    A0 01       LDY #$01        ; .
5E51    84 CF       STY $CF         ; $CF = 0x01
5E53    20 6F 08    JSR $086F       ;
5E56    A2 18       LDX #$18        ; .
5E58    86 CE       STX $CE         ; $CE = 0x18
5E5A    A0 02       LDY #$02        ; .
5E5C    84 CF       STY $CF         ; $CF = 0x02
5E5E    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5E61    A0 10       LDY #$10        ;
5E63    B1 FE       LDA ($FE),Y     ; load player attribute 0x10
5E65    20 24 08    JSR $0824       ; display character
5E68    A9 A0       LDA #$A0        ; ' '
5E6A    20 24 08    JSR $0824       ; display character
5E6D    A2 18       LDX #$18        ; .
5E6F    86 CE       STX $CE         ; _CE = 0x18
5E71    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5E74    A0 11       LDY #$11        ;
5E76    B1 FE       LDA ($FE),Y     ; load player attribute 0x11
5E78    18          CLC             ;
5E79    69 4D       ADC #$4D        ;
5E7B    20 7B 08    JSR $087B       ;
5E7E    A2 26       LDX #$26        ; .
5E80    86 CE       STX $CE         ; $CE = 0x26
5E82    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5E85    A0 12       LDY #$12        ; load player attribute 0x12 (player status G, P, D, etc)
5E87    B1 FE       LDA ($FE),Y     ; .
5E89    20 24 08    JSR $0824       ; display character

;; display MP

5E8C    A2 19       LDX #$19        ; X = 0x19
5E8E    A0 03       LDY #$03        ; Y = 0x03
5E90    20 1E 08    JSR $081E       ; print at X, Y

5E93    CD D0 BA 00                           MP:^@

5E97    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5E9A    A0 16       LDY #$16        ; load player attribute 0x16
5E9C    B1 FE       LDA ($FE),Y     ; .
5E9E    20 33 08    JSR $0833       ; output a byte as two digits

;; display level

5EA1    A2 20       LDX #$20        ; X = 0x20
5EA3    A0 03       LDY #$03        ; Y = 0x03
5EA5    20 1E 08    JSR $081E       ; print at X, Y

5EA8    CC D6 BA 00                           LV:^@

5EAC    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5EAF    A0 1A       LDY #$1A        ; load player attribute 0x1A
5EB1    B1 FE       LDA ($FE),Y     ; .
5EB3    20 66 08    JSR $0866       ; display single digit

;; display strength

5EB6    A2 18       LDX #$18        ; X = 0x18
5EB8    A0 04       LDY #$04        ; Y = 0x04
5EBA    20 1E 08    JSR $081E       ; print at X, y

5EBD    D3 D4 D2 BA 00                        STR:^@

5EC2    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5EC5    A0 13       LDY #$13        ; load player attribute 0x13
5EC7    B1 FE       LDA ($FE),Y     ; .
5EC9    20 33 08    JSR $0833       ; output a byte as two digits

;; display dexterity

5ECC    A2 18       LDX #$18        ; X = 0x18
5ECE    A0 05       LDY #$05        ; Y = 0x05
5ED0    20 1E 08    JSR $081E       ; print at X, Y

5ED3    C4 C5 D8 BA 00                        DEX:^@

5ED8    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5EDB    A0 14       LDY #$14        ; load player attribute 0x14
5EDD    B1 FE       LDA ($FE),Y     ; .
5EDF    20 33 08    JSR $0833       ; output a byte as two digits

;; display intelligence

5EE2    A2 18       LDX #$18        ; X = 0x18
5EE4    A0 06       LDY #$06        ; Y = 0x06
5EE6    20 1E 08    JSR $081E       ; print at X, Y

5EE9    C9 CE D4 BA 00                        INT:^@

5EEE    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5EF1    A0 15       LDY #$15        ; load player attribute 0x15
5EF3    B1 FE       LDA ($FE),Y     ; .
5EF5    20 33 08    JSR $0833       ; output a byte as two digits

;; display hit points

5EF8    A2 20       LDX #$20        ; X = 0x20
5EFA    A0 04       LDY #$04        ; Y = 0x04
5EFC    20 1E 08    JSR $081E       ; print at X, Y

5EFF    C8 D0 BA 00                           HP:^@

5F03    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5F06    A0 18       LDY #$18        ; load player attribute 0x18
5F08    B1 FE       LDA ($FE),Y     ; .
5F0A    20 33 08    JSR $0833       ; output a byte as two digits
5F0D    A0 19       LDY #$19        ; load player attribute 0x19
5F0F    B1 FE       LDA ($FE),Y     ; .
5F11    20 33 08    JSR $0833       ; output a byte as two digits

;; display hit max

5F14    A2 20       LDX #$20        ; X = 0x20
5F16    A0 05       LDY #$05        ; Y = 0x05
5F18    20 1E 08    JSR $081E       ; print at X, Y

5F1B    C8 CD BA 00                           HM:^@

5F1F    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5F22    A0 1A       LDY #$1A        ; load player attribute 0x1A
5F24    B1 FE       LDA ($FE),Y     ; .
5F26    20 33 08    JSR $0833       ; output a byte as two digits
5F29    A0 1B       LDY #$1B        ; load player attribute 0x1B
5F2B    B1 FE       LDA ($FE),Y     ; .
5F2D    20 33 08    JSR $0833       ; output a byte as two digits

;; display EX

5F30    A2 20       LDX #$20        ; X = 0x20
5F32    A0 06       LDY #$06        ; Y = 0x06
5F34    20 1E 08    JSR $081E       ; print at X, Y

5F37    C5 D8 BA 00                           EX:^@

5F3B    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5F3E    A0 1C       LDY #$1C        ; load player attribute 0x1C
5F40    B1 FE       LDA ($FE),Y     ; .
5F42    20 33 08    JSR $0833       ; output a byte as two digits
5F45    A0 1D       LDY #$1D        ; load player attribute 0x1D
5F47    B1 FE       LDA ($FE),Y     ; .
5F49    20 33 08    JSR $0833       ; output a byte as two digits

;; display W

5F4C    A2 18       LDX #$18        ; X = 0x18
5F4E    A0 07       LDY #$07        ; Y = 0x07
5F50    20 1E 08    JSR $081E       ; print at X, Y

5F53    D7 BA 00                              W:^@

5F56    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5F59    A0 1E       LDY #$1E        ; load player attribute 0x1E
5F5B    B1 FE       LDA ($FE),Y     ; .
5F5D    18          CLC             ;
5F5E    69 25       ADC #$25        ;
5F60    20 7E 08    JSR $087E       ; print from string table

;; display A

5F63    A2 18       LDX #$18        ; X = 0x18
5F65    A0 08       LDY #$08        ; Y = 0x08
5F67    20 1E 08    JSR $081E       ; print at X, Y

5F6A    C1 BA 00                              A:^@

5F6D    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
5F70    A0 1F       LDY #$1F        ; load player attribute 0x1F
5F72    B1 FE       LDA ($FE),Y     ; .
5F74    18          CLC             ;
5F75    69 35       ADC #$35        ;
5F77    20 7E 08    JSR $087E       ; print from string table
5F7A    20 77 63    JSR $6377       ; restore $CE/CF from $6382/$6383
5F7D    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5F80    F0 5E       BEQ $5FE0       ; not arrow, go to $5FE0
5F82    30 0B       BMI $5F8F       ; if negative (left arrow), go to $5F8F

; next player

5F84    E6 D4       INC $D4         ; increment player number
5F86    A5 0F       LDA $0F         ; get party size
5F88    C5 D4       CMP $D4         ; if party size < player number
5F8A    90 0A       BCC $5F96       ;     branch to $5F96
5F8C    4C 2E 5E    JMP $5E2E       ; otherwise, display player stats

; previous player

5F8F    C6 D4       DEC $D4         ; decrement player number
5F91    F0 3C       BEQ $5FCF       ; if zero, branch to $5FCF
5F93    4C 2E 5E    JMP $5E2E       ; otherwise, display player stats

; went past last player

5F96    20 0A 60    JSR $600A       ;
5F99    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5F9C    F0 42       BEQ $5FE0       ; not arrow, go to $5FE0
5F9E    10 07       BPL $5FA7       ; if right arrow, go to $5FA7

5FA0    A5 0F       LDA $0F         ; get party size
5FA2    85 D4       STA $D4         ; set player number to last player
5FA4    4C 2E 5E    JMP $5E2E       ; display player stats

5FA7    20 8C 60    JSR $608C       ;
5FAA    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5FAD    F0 31       BEQ ---         ;
5FAF    30 E5       BMI ---         ;
5FB1    20 0A 61    JSR $610A       ;
5FB4    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5FB7    F0 27       BEQ ---         ;
5FB9    30 EC       BMI ---         ;
5FBB    20 80 61    JSR $6180       ;
5FBE    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5FC1    F0 1D       BEQ ---         ;
5FC3    30 EC       BMI ---         ;
5FC5    20 AA 62    JSR $62AA       ;
5FC8    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5FCB    F0 13       BEQ ---         ;
5FCD    30 EC       BMI ---         ;

; went past first player

5FCF    20 0B 63    JSR $630B       ;
5FD2    20 F4 5F    JSR $5FF4       ; get arrow key (left = -1, right = +1, none = 0)
5FD5    F0 09       BEQ ---         ;
5FD7    30 EC       BMI ---         ;
5FD9    A9 01       LDA #$01        ;
5FDB    85 D4       STA $D4         ;
5FDD    4C 2E 5E    JMP $5E2E       ;

5FE0    20 69 08    JSR $0869       ;
5FE3    20 45 08    JSR $0845       ;
5FE6    A5 0B       LDA $0B         ;
5FE8    10 07       BPL ---         ;
5FEA    A5 C5       LDA $C5         ;
5FEC    85 D4       STA $D4         ;
5FEE    20 01 85    JSR $8501       ;
5FF1    4C 84 63    JMP $6384       ; return to main loop

;; get a key. If left arrow, return -1, if right arrow, return +1, otherwise return 0

5FF4    20 00 08    JSR $0800       ; get a key
5FF7    10 FB       BPL $5FF4       ;
5FF9    C9 88       CMP #$88        ; if left arrow
5FFB    F0 07       BEQ $6004       ;   go to $6004 and return -1
5FFD    C9 95       CMP #$95        ; if right arrow
5FFF    F0 06       BEQ $6007       ;   go to $6007 and return +1
6001    A9 00       LDA #$00        ; else
6003    60          RTS             ;   return 0

6004    A9 FF       LDA #$FF        ; return -1
6006    60          RTS             ;

6007    A9 01       LDA #$01        ; return +1
6009    60          RTS             ;

;;

600A    20 69 08    JSR $0869       ;
600D    20 6C 63    JSR $636C       ; preserve $CE/$CE in $6382/$6383
6010    A2 1B       LDX #$1B        ; X = 0x1B
6012    A0 00       LDY #$00        ; Y = 0x00
6014    84 D8       STY $D8         ; also store X in $D8
6016    84 D9       STY $D9         ; and Y in $D9
6018    20 1E 08    JSR $081E       ; print at X, Y

601B    1F D7 C5 C1 D0 CF CE D3 1D 00          >WEAPONS<^@

6025    A5 D9       LDA $D9         ;
6027    29 08       AND #$08        ;
6029    18          CLC             ;
602A    69 18       ADC #$18        ;
602C    85 CE       STA $CE         ; store in _CE
602E    A5 D9       LDA $D9         ;
6030    29 07       AND #$07        ;
6032    85 CF       STA $CF         ; store in _CF
6034    E6 CF       INC $CF         ; _CF++
6036    A5 D8       LDA $D8         ;
6038    F0 42       BEQ ---         ;
603A    18          CLC             ;
603B    69 20       ADC #$20        ;
603D    A8          TAY             ;
603E    B9 00 ED    LDA $ED00,Y     ; some offset into party info
6041    F0 2D       BEQ ---         ;
6043    48          PHA             ;
6044    A5 D8       LDA $D8         ;
6046    18          CLC             ;
6047    69 C1       ADC #$C1        ;
6049    20 24 08    JSR $0824       ;
604C    68          PLA             ;
604D    C9 10       CMP #$10        ;
604F    B0 0D       BCS ---         ;
6051    48          PHA             ;
6052    A9 AD       LDA #$AD        ;
6054    20 24 08    JSR $0824       ;
6057    68          PLA             ;
6058    20 66 08    JSR $0866       ;
605B    4C 61 60    JMP $6061       ;
605E    20 33 08    JSR $0833       ;
6061    A9 AD       LDA #$AD        ;
6063    20 24 08    JSR $0824       ;
6066    A5 D8       LDA $D8         ;
6068    18          CLC             ;
6069    69 3D       ADC #$3D        ;
606B    20 7E 08    JSR $087E       ;
606E    E6 D9       INC $D9         ;
6070    E6 D8       INC $D8         ;
6072    A5 D8       LDA $D8         ;
6074    C9 10       CMP #$10        ;
6076    90 AD       BCC ---         ;
6078    20 77 63    JSR $6377       ;
607B    60          RTS             ;

607C    20 21 08    JSR $0821       ;

C1 AD C8 C1 CE C4 D3 00     A-HANDS

E6 D9 4C 70 60
608C    20 69 08 20 6C 63 A2 1B A0 00 84 D8 84 D9 20 1E
609C    08
1F C1 D2 CD CF D5 D2 1C 00  .ARMOUR..
A9 18 85 CE A5 D9
60AC    85 CF E6 CF A5 D8 F0 42 18 69 18 A8 B9 00 ED F0
60BC    2D 48 A5 D8 18 69 C1 20 24 08 68 C9 10 B0 0D 48
60CC    A9 AD 20 24 08 68 20 66 08 4C DB 60 20 33 08 A9
60DC    AD 20 24 08 A5 D8 18 69 35 20 7E 08 E6 D9 E6 D8
60EC    A5 D8 C9 08 90 B4 20 77 63 60 20 21 08
C1 AD CE    A-N
60FC    CF A0 C1 D2 CD CF D5 D2 00  O ARMOUR
E6 D9 4C EA 60 20 69
610C    08 20 6C 63 A2 1A A0 00 20 1E 08
1E C5 D1 D5 C9      .EQUI
611C    D0 CD C5 CE D4 1C 00 PMENT..
20 93 62 A0 08 B9 00 ED 20
612C    33 08 20 21 08
AD D4 CF D2 C3 C8 C5 D3 00    -TORCHES.
20 93
613C    62 A0 09 B9 00 ED 20 33 08 20 21 08
AD C7 C5 CD D3 00   -GEMS.
614E    20 93 62 A0 0A B9 00 ED 20 33 08 20 21 08

615C    AD CB C5 D9 D3 00  -KEYS.
20 93 62 A0 0B B9 00 ED F0 10
616C    20 33 08 20 21 08
AD D3 C5 D8 D4 C1 CE D4 D3 00   -SEXTANTS.
617C    20 77 63 60 20 69 08 20 6C 63 A2 1C A0 00 20 1E
618C    08
1E C9 D4 C5 CD D3 1C 00 A9 00 85 CF AD 0C ED .ITEMS..
619C    F0 23 85 D8 20 93 62 20 21 08
D3 D4 CF CE C5 D3 BA 00   STONES:.
61AE    A0 07 26 D8 90 0A 84 D9 B9 9A 62 20 24 08
61BC    A4 D9 88 10 EF AD 0D ED F0 22 85 D8 20 93 62 20
61CC    21 08
D2 D5 CE C5 D3 BA 00 RUNES:.
A0 07 26 D8 90 0A 84
61DC    D9 B9 A2 62 20 24 08 A4 D9 88 10 EF AD 0E ED F0
61EC    33 20 93 62 AD 0E ED 29 04 F0 09 20 21 08
C2 C5   BE
61FC    CC CC A0 00   LL .
AD 0E ED 29 02 F0 09 20 21 08
C2 CF   BO
620C    CF CB A0 00   OK .
AD 0E ED 29 01 F0 2C 20 21 08
C3 C1   CA
621C    CE C4 CC 00     NDL.
AD 0F ED F0 36 20 93 62 20 21 08
B3    3
622C    A0 D0 C1 D2 D4 A0 CB C5 D9 BA 00     PART KEY:.
AD 0F ED 29 04
623C    F0 05 A9 D4 20 24 08 AD 0F ED 29 02 F0 05 A9 CC
624C    20 24 08 AD 0F ED 29 01 F0 05 A9 C3 20 24 08 AD
625C    15 ED F0 0B 20 93 62 20 21 08
C8 CF D2 CE 00    HORN.
AD
626C    16 ED F0 0C 20 93 62 20 21 08
D7 C8 C5 C5 CC 00   WHEEL.
627C    AD 17 ED F0 0E 30 0C 20 93 62 20 21 08
D3 CB D5    SKU
628C    CC CC 00    LL.
20 77 63 60 E6 CF A2 18 86 CE 60 C2 D7
629C    D0 CF C7 D2 D9 C2 C8 D3 C8 D3 CA D6 C3 C8 20 69
62AC    08 20 6C 63 A2 1A A0 00 84 F0 20 1E 08
1E D2 C5    .RE
62BC    C1 C7 C5 CE D4 D3 1D 00 AGENTS..
18 A5 F0 69 38 A8 B9 00
62CC    ED F0 30 85 EA 20 93 62 18 A5 F0 69 C1 20 24 08
62DC    A5 EA C9 10 B0 0D A9 AD 20 24 08 A5 EA 20 66 08
62EC    4C F2 62 20 33 08 A9 AD 20 24 08 18 A5 F0 69 5D
62FC    20 7E 08 E6 F0 A5 F0 C9 08 90 BD 20 77 63 60 20
630C    69 08 20 6C 63 A2 1A A0 00 84 D8 84 D9 20 1E 08

631C    1E CD C9 D8 D4 D5 D2 C5 D3 1D 00 .MIXTURES..

6327    A5 D9       LDA $D9         ;
6329    4A          LSR A           ;
632A    4A          LSR A           ;
632B    4A          LSR A           ;
632C    A2 05       LDX #$05        ;
632E    20 2A 08    JSR $082A       ;
6331    18          CLC             ;
6332    69 18       ADC #$18        ;
6334    85 CE       STA $CE         ;
6336    C9 24       CMP #$24        ;
6338    B0 26       BCS ---         ;
633A    A5 D9       LDA $D9         ;
633C    29 07       AND #$07        ;
633E    85 CF       STA $CF         ;
6340    E6 CF       INC $CF         ;
6342    A4 D8       LDY $D8         ;
6344    B9 40 ED    LDA $ED40,Y     ;
6347    F0 17       BEQ ---         ;
6349    A5 D8       LDA $D8         ;
634B    18          CLC             ;
634C    69 C1       ADC #$C1        ;
634E    20 24 08    JSR $0824       ;
6351    A9 AD       LDA #$AD        ;
6353    20 24 08    JSR $0824       ;
6356    A4 D8       LDY $D8         ;
6358    B9 40 ED    LDA $ED40,Y     ;
635B    20 33 08    JSR $0833       ;
635E    E6 D9       INC $D9         ;
6360    E6 D8       INC $D8         ;
6362    A5 D8       LDA $D8         ;
6364    C9 1A       CMP #$1A        ;
6366    90 BF       BCC ---         ;
6368    20 77 63    JSR $6377       ; restore $CE/CF from $6382/$6383
636B    60          RTS             ;

;; store $CE/$CF in $6382/$6383

636C    A5 CE       LDA $CE         ;
636E    8D 82 63    STA $6382       ;
6371    A5 CF       LDA $CF         ;
6373    8D 83 63    STA $6383       ;
6376    60          RTS             ;

;; restore $CE/CF from $6382/$6383

6377    AD 82 63    LDA $6382       ;
637A    85 CE       STA $CE         ;
637C    AD 83 63    LDA $6383       ;
637F    85 CF       STA $CF         ;
6381    60          RTS             ;

;;

6382    00
6383    00

;; ??? some sort of main loop that everything returns to

6384    20 AA 84    JSR $84AA       ; maybe checks to see if anyone is awake in the party ???
6387    A5 0B       LDA $0B         ;
6389    10 03       BPL $638E       ;
638B    4C 2D 72    JMP $722D       ;

638E    A5 1B       LDA $1B         ;
6390    C9 50       CMP #$50        ;
6392    B0 10       BCS $63A4       ;
6394    20 4E 08    JSR $084E       ;
6397    29 03       AND #$03        ;
6399    D0 09       BNE $63A4       ;
639B    A5 1B       LDA $1B         ;
639D    F8          SED             ;
639E    18          CLC             ;
639F    69 01       ADC #$01        ;
63A1    D8          CLD             ;
63A2    85 1B       STA $1B         ;
63A4    A5 0F       LDA $0F         ;
63A6    85 D4       STA $D4         ;
63A8    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
63AB    A0 12       LDY #$12        ;
63AD    B1 FE       LDA ($FE),Y     ;
63AF    C9 D3       CMP #$D3        ;
63B1    D0 0E       BNE $63C1       ;
63B3    20 4E 08    JSR $084E       ;
63B6    29 07       AND #$07        ;
63B8    D0 1B       BNE ---         ;
63BA    A9 C7       LDA #$C7        ;
63BC    91 FE       STA ($FE),Y     ;
63BE    4C D5 63    JMP $63D5       ;

63C1    C9 D0       CMP #$D0        ;
63C3    D0 10       BNE $63D5       ;
63C5    A9 02       LDA #$02        ;
63C7    20 BE 85    JSR $85BE       ;
63CA    20 01 85    JSR $8501       ;
63CD    A9 06       LDA #$06        ;
63CF    20 54 08    JSR $0854       ;
63D2    20 01 85    JSR $8501       ;

63D5    C6 D4       DEC $D4         ;
63D7    D0 CF       BNE ---         ;
63D9    A5 0F       LDA $0F         ;
63DB    20 0D 66    JSR $660D       ;
63DE    B0 03       BCS $63E3       ;

63E0    20 84 65    JSR $6584       ;

63E3    20 B3 65    JSR $65B3       ;
63E6    A5 F4       LDA $F4         ;
63E8    30 0C       BMI ---         ;
63EA    20 C4 6B    JSR $6BC4       ;
63ED    20 3D 66    JSR $663D       ;
63F0    20 89 67    JSR $6789       ;
63F3    20 C4 6B    JSR $6BC4       ;
63F6    A5 0B       LDA $0B         ;
63F8    C9 03       CMP #$03        ;
63FA    D0 11       BNE ---         ;
63FC    20 72 08    JSR $0872       ; in dungeon, display direction and level
63FF    20 FF 6D    JSR $6DFF       ;
6402    A5 C8       LDA $C8         ;
6404    29 F0       AND #$F0        ;
6406    C9 D0       CMP #$D0        ;
6408    D0 03       BNE ---         ;
640A    4C 8C 6E    JMP $6E8C       ;

640D    A5 11       LDA $11         ;
640F    F0 02       BEQ ---         ;
6411    C6 11       DEC $11         ;
6413    4C 93 6D    JMP $6D93       ;

;;

6416    A5 0B       LDA $0B         ; .
6418    C9 01       CMP #$01        ; if _0B != 0x01,
641A    D0 34       BNE $6450       ;   skip bridge troll check
641C    A5 C8       LDA $C8         ; .
641E    C9 17       CMP #$17        ; if _C8 != 0x17 (BRIDGE)
6420    D0 2E       BNE $6450       ;   skip bridge troll check
6422    20 4E 08    JSR $084E       ; generate a random number
6425    29 07       AND #$07        ; if any of low three bits are set
6427    D0 27       BNE $6450       ;    skip (so 12.5% chance of a troll)
6429    20 21 08    JSR $0821       ; print

642C    8D C2 D2 C9 C4 C7 C5 A0 D4 D2 CF CC CC D3 A1 8D 00                      ^MBRIDGE TROLLS!^M^@

643D    A9 A4       LDA #$A4        ; .
643F    85 C0       STA $C0         ; _C0 = 0xA4
6441    A5 00       LDA $00         ; .
6443    85 C1       STA $C1         ; _C1 = _00
6445    A5 01       LDA $01         ; .
6447    85 C2       STA $C2         ; _C2 = _01
6449    A9 17       LDA #$17        ; .
644B    85 C3       STA $C3         ; _C3 = 0x17
644D    4C B0 6F    JMP $6FB0       ; enter combat

;;

6450    A5 F4       LDA $F4         ; if _F4
6452    10 01       BPL $6455       ; has highbit set,
6454    60          RTS             ;     return

6455    A5 0B       LDA $0B         ;
6457    30 09       BMI ---         ;
6459    C9 03       CMP #$03        ;
645B    F0 62       BEQ ---         ;
645D    A5 C8       LDA $C8         ; load current tile
645F    4C 1F 65    JMP $651F       ;

6462    A6 D4       LDX $D4         ;
6464    BD 9F EF    LDA $EF9F,X     ;
6467    F0 53       BEQ ---         ;
6469    BD 7F EF    LDA $EF7F,X     ;
646C    85 06       STA $06         ;
646E    BD 8F EF    LDA $EF8F,X     ;
6471    85 07       STA $07         ;
6473    20 8D 08    JSR $088D       ;
6476    C9 03       CMP #$03        ;
6478    F0 16       BEQ ---         ;
647A    C9 44       CMP #$44        ;
647C    F0 12       BEQ ---         ;
647E    C9 46       CMP #$46        ;
6480    F0 08       BEQ ---         ;
6482    C9 47       CMP #$47        ;
6484    F0 20       BEQ ---         ;
6486    C9 4C       CMP #$4C        ;
6488    D0 32       BNE ---         ;
648A    20 79 86    JSR $8679       ;
648D    4C BC 64    JMP $64BC       ;

6490    20 A9 7E    JSR $7EA9       ;
6493    B1 FE       LDA ($FE),Y     ;
6495    C9 C7       CMP #$C7        ;
6497    D0 23       BNE ---         ;
6499    A9 D0       LDA #$D0        ;
649B    91 FE       STA ($FE),Y     ;
649D    20 F9 86    JSR $86F9       ;
64A0    20 45 08    JSR $0845       ;
64A3    4C BC 64    JMP $64BC       ;
64A6    20 A9 7E    JSR $7EA9       ;
64A9    30 11       BMI ---         ;
64AB    A9 D3       LDA #$D3        ;
64AD    91 FE       STA ($FE),Y     ;
64AF    A6 D4       LDX $D4         ;
64B1    A9 38       LDA #$38        ;
64B3    9D 9F EF    STA $EF9F,X     ;
64B6    20 F9 86    JSR $86F9       ;
64B9    20 45 08    JSR $0845       ;
64BC    4C 36 65    JMP $6536       ;

64BF    A5 C8       LDA $C8         ; load current tile
64C1    29 F0       AND #$F0        ;
64C3    C9 A0       CMP #$A0        ;
64C5    D0 11       BNE ---         ;
64C7    A5 C8       LDA $C8         ; load current tile
64C9    29 03       AND #$03        ;
64CB    F0 6C       BEQ ---         ;
64CD    C9 02       CMP #$02        ;
64CF    F0 62       BEQ ---         ;
64D1    C9 03       CMP #$03        ;
64D3    D0 61       BNE ---         ;
64D5    4C 60 65    JMP $6560       ;

64D8    C9 80       CMP #$80        ;
64DA    D0 5A       BNE ---         ;
64DC    A5 C8       LDA $C8         ; load current tile
64DE    29 0F       AND #$0F        ;
64E0    F0 06       BEQ ---         ;
64E2    C9 08       CMP #$08        ;
64E4    90 15       BCC ---         ;
64E6    B0 2A       BCS ---         ;
64E8    20 21 08    JSR $0821       ;

64EB    8D D7 C9 CE C4 D3 A1 8D 00                                              ^MWINDS!^M^@

64F4    A9 00       LDA #$00        ;
64F6    85 11       STA $11         ;
64F8    4C 36 65    JMP $6536       ;

64FB    20 21 08    JSR $0821       ;

64FE    8D C6 C1 CC CC C9 CE C7 A0 D2 CF C3 CB D3                               ^MFALLING ROCKS!^M^@

650F    4C 33 65    JMP $6533       ;

6512    20 21 08    JSR $0821       ;

6515    8D D0 C9 D4 A1 8D 00                                                    ^MPIT!^M^@

651C    4C 33 65    JMP $6533       ;

651F    C9 03       CMP #$03        ;
6521    F0 16       BEQ ---         ;
6523    C9 44       CMP #$44        ;
6525    F0 12       BEQ ---         ;
6527    C9 46       CMP #$46        ;
6529    F0 08       BEQ ---         ;
652B    C9 47       CMP #$47        ;
652D    F0 31       BEQ ---         ;
652F    C9 4C       CMP #$4C        ;
6531    D0 03       BNE ---         ;
6533    20 88 86    JSR $8688       ;
6536    4C 83 65    JMP $6583       ;

6539    A5 0F       LDA $0F         ;
653B    85 D4       STA $D4         ;
653D    20 A9 7E    JSR $7EA9       ;
6540    B1 FE       LDA ($FE),Y     ;
6542    C9 C7       CMP #$C7        ;
6544    D0 3D       BNE ---         ;
6546    20 4E 08    JSR $084E       ;
6549    29 07       AND #$07        ;
654B    D0 0C       BNE ---         ;
654D    A9 D0       LDA #$D0        ;
654F    A0 12       LDY #$12        ;
6551    91 FE       STA ($FE),Y     ;
6553    20 F9 86    JSR $86F9       ;
6556    20 45 08    JSR $0845       ;
6559    C6 D4       DEC $D4         ;
655B    D0 E0       BNE ---         ;
655D    4C 83 65    JMP $6583       ;

6560    A5 0F       LDA $0F         ;
6562    85 D4       STA $D4         ;
6564    20 A9 7E    JSR $7EA9       ;
6567    30 13       BMI ---         ;
6569    20 4E 08    JSR $084E       ;
656C    29 03       AND #$03        ;
656E    D0 0C       BNE ---         ;
6570    A9 D3       LDA #$D3        ;
6572    A0 12       LDY #$12        ;
6574    91 FE       STA ($FE),Y     ;
6576    20 F9 86    JSR $86F9       ;
6579    20 45 08    JSR $0845       ;
657C    C6 D4       DEC $D4         ;
657E    D0 E4       BNE ---         ;
6580    4C 83 65    JMP $6583       ;

6583    60          RTS             ;

6584    20 21 08    JSR $0821       ;

6587    8D D3 D4 C1 D2 D6 C9 CE C7 A1 A1 A1 8D 00   ^MSTARVING!!!^M^@

6595    A5 0F 85 D4 20 BB 7E
659C    30 05 A9 02 20 BE 85 C6 D4 D0 F2 20 F5 84 A9 06
65AC    20 54 08 20 F5 84 60 A5 0F 85 D4 20 BB 7E 30 4C
65BC    A0 11 B1 FE 85 D8 A0 15 B1 FE 20 40 85 0A A6 D8
65CC    F0 26 CA F0 19 CA F0 0C CA F0 17 CA F0 0B CA F0
65DC    0D CA F0 0A A9 00 4C F4 65 4A 4A 4C F4 65 4A 4C
65EC    F4 65 4A 85 D8 4A 65 D8 20 28 85 85 D8 A0 16 B1
65FC    FE C5 D8 B0 07 F8 18 69 01 D8 91 FE C6 D4 10 AB
660C    60 85 D8 F8 38 AD 12 ED E5 D8 8D 12 ED B0 20 AD
661C    11 ED E9 00 8D 11 ED B0 16 AD 10 ED E9 00 8D 10
662C    ED B0 0C A9 00 8D 12 ED 8D 11 ED 8D 10 ED 18 D8
663C    60 A5 0B C9 01 D0 03 4C 4E 66 C9 03 D0 03 4C 21
664C    67 60 20 4E 08 29 0F F0 03 4C 20 67 A5 04 0A 0A
665C    0A 0A 85 F6 A5 05 0A 0A 0A 0A 85 F7 38 A5 02 E9
666C    05 85 F8 38 A5 03 E9 05 85 F9 A2 03 BD 60 EE D0
667C    6F 20 4E 08 29 1F 85 06 38 E5 F8 C9 0B 90 61 20
668C    4E 08 29 1F 85 07 38 E5 F9 C9 0B 90 53 20 84 08
669C    C9 02 90 4F C9 04 90 48 C9 08 B0 44 18 A5 06 65
66AC    F6 9D 20 EE 18 A5 07 65 F7 9D 40 EE A5 1C D0 16
66BC    A5 1D C9 01 90 0B F0 04 C9 03 B0 0A A9 07 4C D4
66CC    66 A9 03 4C D4 66 A9 0F 85 D8 20 4E 08 25 D8 85
66DC    D8 20 4E 08 25 D8 0A 0A 69 C0 9D 60 EE 9D 00 EE
66EC    4C 1A 67 20 4E 08 29 07 D0 24 18 A5 06 65 F6 9D
66FC    20 EE 18 A5 07 65 F7 9D 40 EE 20 4E 08 29 07 0A
670C    69 80 C9 82 D0 02 A9 80 9D 60 EE 9D 00 EE CA 30
671C    03 4C 78 66 60 A5 0C 0A 0A 69 01 AA BD 60 EE D0
672C    51 86 D8 20 4E 08 29 07 85 06 C5 00 F0 44 20 4E
673C    08 29 07 85 07 C5 01 F0 39 20 96 08 D0 34 20 4E
674C    08 29 03 18 65 0C 0A 0A 69 90 C9 AC F0 24 A6 D8
675C    9D 60 EE A5 06 9D 20 EE 9D 80 EE A5 07 9D 40 EE
676C    9D A0 EE A5 0C 9D C0 EE BD 60 EE 20 90 6B 11 FE
677C    91 FE CA 30 07 8A 4A 4A C5 0C B0 A0 60 A5 0B C9
678C    01 D0 03 4C A1 67 C9 02 D0 03 4C F7 69 C9 03 D0
679C    03 4C BF 6A 60 A5 04 0A 0A 0A 0A 85 F6 A5 05 0A
67AC    0A 0A 0A 85 F7 A2 07 BD 60 EE F0 26 38 BD 20 EE
67BC    E5 F6 C9 20 B0 14 38 BD 40 EE E5 F7 C9 20 B0 0A
67CC    86 D8 20 E2 67 A6 D8 4C DE 67 A9 00 9D 60 EE 9D
67DC    00 EE CA 10 D2 60 BD 60 EE C9 8C F0 33 C9 8E F0
67EC    2F 20 3B 6D C9 02 B0 28 20 4B 08 20 21 08

8D C1 D4 D4 C1 C3 CB C5 C4 A0 C2 D9 8D 00 ^MATTACKED BY^M^@

20 59 84 A6
680C    D8 BD 20 EE 85 FA BD 40 EE 85 FB 68 68 4C 79 47
681C    C9 04 B0 27 BD 60 EE C9 80 F0 03 4C 32 69 BD 00
682C    EE C9 80 F0 0F C9 81 F0 04 C9 82 F0 07 A5 F9 D0
683C    0A 4C 8D 6C A5 F8 D0 03 4C 8D 6C BD 60 EE C9 80
684C    F0 03 4C 32 69 BD 00 EE C9 80 D0 06 A5 F8 30 59
685C    10 1E C9 81 D0 06 A5 F9 30 71 10 14 C9 82 D0 08
686C    A5 F8 F0 0C 10 43 30 08 A5 F9 F0 04 10 5D 30 00
687C    20 3B 6D C9 06 B0 77 20 4E 08 29 03 F0 70 BD 00
688C    EE C9 80 D0 07 A9 FF 85 F8 4C B5 68 C9 81 D0 07
689C    A9 FF 85 F9 4C D7 68 C9 82 D0 07 A9 01 85 F8 4C
68AC    B5 68 A9 01 85 F9 4C D7 68 18 BD 20 EE 65 F8 85
68BC    FA BD 40 EE 85 FB 20 81 08 20 17 7F D0 56 BD 00
68CC    EE 29 03 20 A3 6B D0 25 4C E0 69 18 BD 20 EE 85
68DC    FA BD 40 EE 65 F9 85 FB 20 81 08 20 17 7F D0 34
68EC    BD 00 EE 29 03 20 A3 6B D0 03 4C E0 69 60 A5 FA
68FC    C5 FB B0 10 A5 FB 10 06 A9 81 9D 00 EE 60 A9 83
690C    9D 00 EE 60 A5 FA 10 06 A9 80 9D 00 EE 60 A9 82
691C    9D 00 EE 60 20 4E 08 20 64 83 18 7D 00 EE 29 03
692C    09 80 9D 00 EE 60 BD 60 EE C9 88 F0 08 C9 E8 F0
693C    04 C9 F4 90 1F 20 3B 6D A5 FA 20 70 83 C9 05 B0
694C    13 A5 FB 20 70 83 C9 05 B0 0A 20 4E 08 30 05 20
695C    AD 6C A6 D8 A9 02 85 F0 BD 60 EE C9 8C F0 60 C9
696C    8E F0 5C 20 DB 87 30 07 20 4E 08 29 03 D0 50 20
697C    4E 08 30 19 A5 F8 F0 15 18 BD 20 EE 65 F8 85 FA
698C    BD 40 EE 85 FB 20 81 08 20 17 7F F0 47 A5 F9 F0
699C    15 BD 20 EE 85 FA 18 BD 40 EE 65 F9 85 FB 20 81
69AC    08 20 17 7F F0 2E A5 F8 F0 15 18 BD 20 EE 65 F8
69BC    85 FA BD 40 EE 85 FB 20 81 08 20 17 7F F0 15 20
69CC    4E 08 20 64 83 85 F8 20 4E 08 20 64 83 85 F9 C6
69DC    F0 D0 9C 60 BD 20 EE 9D 80 EE BD 40 EE 9D A0 EE
69EC    A5 FA 9D 20 EE A5 FB 9D 40 EE 60 C9 10 90 01 60
69FC    A2 1F A9 02 85 F0 BD 60 EE F0 32 BD C0 EE F0 2D
6A0C    30 18 20 4E 08 30 26 20 4E 08 20 64 83 85 F8 20
6A1C    4E 08 20 64 83 85 F9 4C 3C 6A 20 67 6D C9 02 B0
6A2C    0F BD C0 EE C9 FF D0 08 86 D8 20 F4 67 4C B8 6A
6A3C    20 4E 08 30 19 A5 F8 F0 15 18 BD 20 EE 65 F8 85
6A4C    FA BD 40 EE 85 FB 20 93 08 20 17 7F F0 3C A5 F9
6A5C    F0 15 18 BD 40 EE 65 F9 85 FB BD 20 EE 85 FA 20
6A6C    93 08 20 17 7F F0 23 18 BD 20 EE 65 F8 85 FA BD
6A7C    40 EE 85 FB 20 93 08 20 17 7F F0 0E BD C0 EE C9
6A8C    80 F0 29 C6 F0 30 25 4C 13 6A A5 FA C9 20 B0 1C
6A9C    A5 FB C9 20 B0 16 BD 20 EE 9D 80 EE BD 40 EE 9D
6AAC    A0 EE A5 FA 9D 20 EE A5 FB 9D 40 EE CA 30 03 4C
6ABC    FE 69 60 A2 1F BD 60 EE F0 19 BD C0 EE C5 0C D0
6ACC    12 BD 60 EE C9 AC F0 0B C9 B0 F0 07 A9 07 85 F0
6ADC    4C E2 6A 4C 86 6B BD 20 EE C5 00 D0 07 BD 40 EE
6AEC    C5 01 F0 EF 20 4E 08 20 64 83 85 F8 20 4E 08 20
6AFC    64 83 85 F9 20 4E 08 30 19 A5 F8 F0 15 18 7D 20
6B0C    EE 29 07 85 06 BD 40 EE 85 07 20 96 08 20 AA 80
6B1C    F0 37 A5 F9 F0 15 18 7D 40 EE 29 07 85 07 BD 20
6B2C    EE 85 06 20 96 08 20 AA 80 F0 1E 18 BD 20 EE 65
6B3C    F8 29 07 85 06 BD 40 EE 85 07 20 96 08 20 AA 80
6B4C    F0 07 C6 F0 10 9E 4C 86 6B BD 60 EE 20 90 6B 11
6B5C    FE 91 FE BD 20 EE 9D 80 EE BD 40 EE 9D A0 EE A5
6B6C    06 9D 20 EE A5 07 9D 40 EE BD 80 EE 85 06 BD A0
6B7C    EE 85 07 20 96 08 29 F0 91 FE CA 30 03 4C C1 6A
6B8C    20 00 8C 60 38 E9 90 4A 4A 18 69 01 A0 00 60 FF
6B9C    00 01 00 00 FF 00 01 C5 F5 F0 11 18 69 02 29 03
6BAC    C5 F5 D0 11 A5 E9 29 03 D0 0B F0 06 A5 E9 29 03
6BBC    F0 03 A9 FF 60 A9 00 60 A5 0B C9 01 F0 01 60 A2
6BCC    03 BD 60 EE C9 8C F0 08 C9 8E F0 54 CA 10 F2 60
6BDC    A9 0B 85 DB BD 20 EE C5 00 D0 0A BD 40 EE C5 01
6BEC    D0 03 20 44 6C A0 1F 86 D8 C4 D8 F0 2D B9 60 EE
6BFC    F0 28 BD 20 EE D9 20 EE D0 20 BD 40 EE D9 40 EE
6C0C    D0 18 A9 00 99 60 EE 99 00 EE 86 D8 84 D9 20 4B
6C1C    08 A5 DB 20 54 08 A6 D8 A4 D9 88 10 CA 4C D8 6B
6C2C    A9 0C 85 DB BD 20 EE C5 00 D0 BA BD 40 EE C5 01
6C3C    D0 B3 20 6A 6C 4C F1 6B 86 D8 20 4B 08 A9 8C 85
6C4C    0E 20 4B 08 A9 7F 85 00 A9 4E 85 01 A9 0B 20 54
6C5C    08 20 88 86 A6 D8 A9 10 85 0E 20 03 08 60 86 D8
6C6C    A5 0E 48 A9 8E 85 0E 20 4B 08 A9 0C 20 54 08 20
6C7C    88 86 20 88 86 20 88 86 20 88 86 68 85 0E A6 D8
6C8C    60 A9 4D 8D FD EF 20 CD 6C 10 01 60 20 87 08 48
6C9C    A9 4F 91 FE 20 63 08 20 88 86 20 87 08 68 91 FE
6CAC    60 A9 4F 8D FD EF 20 CD 6C 10 01 60 20 87 08 48
6CBC    A9 4F 91 FE 20 63 08 20 88 86 20 87 08 68 91 FE
6CCC    60 A9 03 85 EA BD 20 EE 85 FA BD 40 EE 85 FB 20
6CDC    4B 08 A9 03 20 54 08 18 A5 FA 65 F8 85 FA 18 A5
6CEC    FB 65 F9 85 FB C5 01 D0 09 A5 FA C5 00 D0 03 A9
6CFC    00 60 20 3C 84 10 19 20 87 08 48 AD FD EF 91 FE
6D0C    20 63 08 20 87 08 68 91 FE C6 EA D0 CA A9 FF 60
6D1C    86 D9 A9 06 20 54 08 A6 D9 E0 08 B0 07 20 4E 08
6D2C    29 03 D0 08 A9 00 9D 60 EE 9D 00 EE A9 FF 60 38
6D3C    A5 00 FD 20 EE 85 FA 20 64 83 85 F8 38 A5 01 FD
6D4C    40 EE 85 FB 20 64 83 85 F9 A5 FA 20 70 83 85 D9
6D5C    A5 FB 20 70 83 18 65 D9 85 D9 60 38 A5 00 FD 20
6D6C    EE 85 FA 20 64 83 85 F8 38 A5 01 FD 40 EE 85 FB
6D7C    20 64 83 85 F9 A5 FA 20 70 83 85 D9 A5 FB 20 70
6D8C    83 18 65 D9 85 D9 60 E6 E9 F8 18 A5 1F 69 01 85
6D9C    1F A5 1E 69 00 85 1E A5 1D 69 00 85 1D A5 1C 69
6DAC    00 85 1C D8 A5 C7 F0 08 C6 C7 D0 04 A9 25 85 C6
6DBC    A5 0B C9 03 D0 13 A5 11 D0 0F 20 21 08

C9 D4 A7 D3 A0 C4 C1 D2 CB A1 8D 00   IT'S DARK!..

20 45 08 20 04 81 4C AC 40

;;

6DDE    A2 00       LDX #$00        ;
6DE0    A9 00       LDA #$00        ;

6DE2    9D 00 EF    STA $EF00,X     ;
6DE5    E8          INX             ;
6DE6    D0 FA       BNE $6DE2       ;

6DE8    A9 01       LDA #$01        ;
6DEA    85 D4       STA $D4         ;
6DEC    A5 0B       LDA $0B         ;
6DEE    85 E8       STA $E8         ;
6DF0    A9 82       LDA #$82        ;
6DF2    85 0B       STA $0B         ;
6DF4    A9 C8       LDA #$C8        ;
6DF6    85 C0       STA $C0         ;
6DF8    85 E6       STA $E6         ;
6DFA    A9 C1       LDA #$C1        ; 'A'

6DFC    4C 6D 70    JMP $706D       ; load the CON_ based on the value in accumulator

6DFF    A5 C8 29 0F F0 76 A5 C8 29 F0 C9 80 F0
6E0C    6E C9 90 F0 6A C9 A0 F0 66 C9 D0 F0 62 C9 F0 F0
6E1C    5E A5 C8 29 0F 0A 0A 69 8C 85 C0 85 E6 A5 00 85
6E2C    06 A5 01 85 07 20 96 08 29 F0 91 FE A2 1F BD C0
6E3C    EE C5 0C D0 1F BD 20 EE C5 00 D0 18 BD 40 EE C5
6E4C    01 D0 11 A9 00 9D C0 EE 9D 60 EE 9D 20 EE 9D 40
6E5C    EE 4C 63 6E CA 10 D7 A2 00 8A 9D 00 EF CA D0 FA
6E6C    68 68 A5 C8 4A 4A 4A 4A AA BD 7C 6E 4C 6D 70 60
6E7C    B0 B1 B2 B3 B4 B0 B0 B0 B0 B0 B0 B0 B5 B0 B6 B0
6E8C    A9 00 20 20 03 38 A5 0A E9 11 85 E0 C9 07 D0 08
6E9C    A5 0C 4A 18 65 E0 85 E0 A5 C8 29 0F 85 E1 A9 01
6EAC    85 E2 A9 02 85 E3 20 51 08 A5 0B 85 E8 A9 81 85
6EBC    0B A9 00 8D 7C 7A 8D 7D 7A A2 FF A5 E1 C9 0F D0
6ECC    4E A5 E0 C9 07 B0 48 20 21 08 8D D4 C8 C5 A0 C1 N%`I.0H !..THE A
6EDC    CC D4 C5 D2 A0 D2 CF CF CD 8D CF C6 A0 00 A5 00 LTER ROOM.OF .%.
6EEC    C9 03 F0 10 B0 1B 20 21 08 D4 D2 D5 D4 C8 8D 00 I.p.0. !.TRUTH..
6EFC    A2 00 F0 1B 20 21 08 CC CF D6 C5 8D 00 A2 01 D0 ".p. !.LOVE..".P
6F0C    0E 20 21 08 C3 CF D5 D2 C1 C7 C5 8D 00 A2 02 86 . !.COURAGE.."..
6F1C    18 A9 00 AA 9D 00 EF E8 D0 FA A2 0F BD 00 02 9D .).*..ohPz".=...
6F2C    10 03 CA 10 F7 A5 10 49 02 0A 0A 0A 0A 85 D8 A5 ..J.w%.I......X%
6F3C    0F 85 D4 20 DC 7E 30 20 48 18 A5 D4 65 D8 A8 88 ..T \~0 H.%TeX(.
6F4C    68 A6 D4 CA 9D A0 EF B9 40 02 9D 80 EF B9 48 02 h&TJ. o9@...o9H.
6F5C    9D 90 EF 20 BB 7E F0 07 A6 D4 A9 00 9D 9F EF C6 ..o ;~p.&T)...oF
6F6C    D4 D0 D0 A2 0F BD 10 02 F0 34 9D 50 EF 9D 60 EF TPP".=..p4.Po.`o
6F7C    BD 20 02 9D 00 EF BD 30 02 9D 10 EF BD 50 EF 20 = ...o=0...o=Po
6F8C    95 7E A8 B9 F0 7D 85 D8 20 FD 7E 46 D8 05 D8 9D .~(9p}.X }~FX.X.
6F9C    40 EF BD 50 EF C9 AC D0 05 A9 3C 9D 60 EF CA 10 @o=PoI,P.)<.`oJ.
6FAC    C4 4C 7D 71

;; enter combat

6FB0    20 21 08

6FB3    8D 8D AA AA AA AA A0 C3 CF CD C2 C1 D4 A0 AA AA AA AA 8D 8D 00          ^M^M**** COMBAT ****^M^M^@

6FC8    A5 C0       LDA $C0         ; _E6 = _C0
6FCA    85 E6       STA $E6         ; .

6FCC    A2 00       LDX #$00        ; zero out $EFXX page
6FCE    8A          TXA             ; .

6FCF    9D 00 EF    STA $EF00,X     ; .
6FD2    CA          DEX             ; .
6FD3    D0 FA       BNE $6FCF       ; .

;; summary of logic for which map to load
; based on values of _0E, _C0, _C3, and _C8

; if _0E >= 0x14 and (_C8 >= 0x14 or _C8 < 0x10):
;   if _C0 == 0x80:
;     _E6 = 'H'
;     MAP 'I'
;   else:
;     if _C3 < 0x03:
;       MAP 'O'
;     else:
;       _C8 == 0x03:   MAP 'S'          0x03 is swamp
;       _C8 == 0x05:   MAP 'B'          0x05 is light forest
;       _C8 == 0x06:   MAP 'F'          0x06 is heavier forest
;       _C8 == 0x07:   MAP 'H'          0x07 is hills
;       _C8 == 0x16:   MAP 'D'          0x16 is ???
;       _C8 == 0x3E:   MAP 'C'          0x3E is corridor
;       _C8 == 0x17:   MAP 'R'          0x17 is bridge
;       _C8 == 0x19:   MAP 'R'          0x19 is bridge
;       _C8 == 0x1A:   MAP 'R'          0x1A is bridge
;       _C8 == 0x3F:   MAP 'R'          0x3F is wooden platform
;       otherwise:     MAP 'G'
; else:
;   if _C0 == 0x80
;     _E6 = 'H'
;     MAP 'T'
;   else:
;     if _C3 < 0x03:
;       MAP 'P'
;     else:
;       MAP 'E'


6FD5    A5 0E       LDA $0E         ; get _0E
6FD7    C9 14       CMP #$14        ; if < 0x14
6FD9    90 0A       BCC $6FE5       ;     go to $6FE5

6FDB    A5 C8       LDA $C8         ; get _C8
6FDD    C9 14       CMP #$14        ; if >= 0x14
6FDF    B0 23       BCS $7004       ;     go to $7004

6FE1    C9 10       CMP #$10        ; if < 0x10
6FE3    90 1F       BCC $7004       ;     go to $7004

6FE5    A5 C0       LDA $C0         ; get _C0
6FE7    C9 80       CMP #$80        ; if != 0x80
6FE9    D0 09       BNE $6FF4       ;     go to $6FF4

6FEB    A9 C8       LDA #$C8        ; .
6FED    85 E6       STA $E6         ; _E6 = 0xC8 'H'
6FEF    A9 D4       LDA #$D4        ; 'T'
6FF1    4C 6D 70    JMP $706D       ; load combat map

6FF4    A5 C3       LDA $C3         ; get _C3
6FF6    C9 03       CMP #$03        ; if < 0x03
6FF8    90 05       BCC $6FFF       ;     go to $6FFF

6FFA    A9 C5       LDA #$C5        ; 'E'
6FFC    4C 6D 70    JMP $706D       ; load combat map

6FFF    A9 D0       LDA #$D0        ; 'P'
7001    4C 6D 70    JMP $706D       ; load combat map

7004    A5 C0       LDA $C0         ; get _C0
7006    C9 80       CMP #$80        ; if != 80
7008    D0 09       BNE $7013       ;     go to $7013

700A    A9 C8       LDA #$C8        ; .
700C    85 E6       STA $E6         ; _E6 = 0xC8 'H'
700E    A9 C9       LDA #$C9        ; 'I'
7010    4C 6D 70    JMP $706D       ; load combat map

7013    A5 C3       LDA $C3         ; get _C3
7015    C9 03       CMP #$03        ; if >= 0x03
7017    B0 05       BCS $701E       ;     go to $701E

7019    A9 CF       LDA #$CF        ; 'O'
701B    4C 6D 70    JMP $706D       ; load combat map

701E    A5 C8       LDA $C8         ; get _C8
7020    C9 03       CMP #$03        ; if != 0x03
7022    D0 05       BNE $7029       ;     go to $7029

7024    A9 D3       LDA #$D3        ; 'S'
7026    4C 6D 70    JMP $706D       ; load combat map

7029    C9 05       CMP #$05        ; if != 0x05
702B    D0 05       BNE $7032       ;     go to $7032

702D    A9 C2       LDA #$C2        ; 'B'
702F    4C 6D 70    JMP $706D       ; load combat map

7032    C9 06       CMP #$06        ; if != 0x06
7034    D0 05       BNE $703B       ;     go to $703B
7036    A9 C6       LDA #$C6        ; 'F'
7038    4C 6D 70    JMP $706D       ; load combat map

703B    C9 07       CMP #$07        ; if != 0x07
703D    D0 05       BNE $7044       ;     go to $7044
703F    A9 C8       LDA #$C8        ; 'H'
7041    4C 6D 70    JMP $706D       ; load combat map

7044    C9 16       CMP #$16        ; if != 0x16
7046    D0 05       BNE $704D       ;     go to $704D
7048    A9 C4       LDA #$C4        ; 'D'
704A    4C 6D 70    JMP $706D       ; load combat map

704D    C9 3E       CMP #$3E        ; if != 0x3E
704F    D0 05       BNE $7056       ;     go to $7056
7051    A9 C3       LDA #$C3        ; 'C'
7053    4C 6D 70    JMP $706D       ; load combat map

7056    C9 17       CMP #$17        ; if == 0x17
7058    F0 0C       BEQ $7066       ;     go to $7066
705A    C9 19       CMP #$19        ; if == 0x19
705C    F0 08       BEQ $7066       ;     go to $7066
705E    C9 1A       CMP #$1A        ; if == 0x1A
7060    F0 04       BEQ $7066       ;     go to $7066
7062    C9 3F       CMP #$3F        ; if != 0x3F
7064    D0 05       BNE $706B       ;     go to #706B

7066    A9 D2       LDA #$D2        ; 'R'
7068    4C 6D 70    JMP $706D       ; load combat map

706B    A9 C7       LDA #$C7        ; 'G'

;; load combat map

706D    8D 82 70    STA $7082       ; set the right file name
7070    A9 00       LDA #$00        ; .
7072    20 20 03    JSR $0320       ; call $0320 with A = 00
7075    20 1B 08    JSR $081B       ; print

; $7082 is the @
7078    84 C2 CC CF C1 C4 A0 C3 CF CE C0 AC C1 A4 B2 B4 B0 8D 00     ^DBLOAD CON@,A$240^M^@

708B    A5 0B       LDA $0B         ; .
708D    C9 82       CMP #$82        ; if _0B == 0x82
708F    F0 06       BEQ $7097       ;     go to $7097

7091    85 E8       STA $E8         ; _E8 = _0B
7093    A9 80       LDA #$80        ; _0B = 0x80
7095    85 0B       STA $0B         ; .

7097    A5 E8       LDA $E8         ; .
7099    C9 02       CMP #$02        ; if _E8 != 0x02
709B    D0 1A       BNE ---         ;
709D    A5 0B       LDA $0B         ;
709F    C9 82       CMP #$82        ;
70A1    F0 14       BEQ ---         ;
70A3    A5 C0       LDA $C0         ;
70A5    C9 50       CMP #$50        ;
70A7    F0 05       BEQ ---         ;
70A9    A9 00       LDA #$00        ;
70AB    4C D1 70    JMP $70D1       ;

70AE    A5 0F       LDA $0F         ;
70B0    0A          ASL A           ;
70B1    38          SEC             ;
70B2    E9 01       SBC #$01        ;
70B4    4C D1 70    JMP $70D1       ;

70B7    A5 E6       LDA $E6         ;
70B9    30 07       BMI ---         ;
70BB    20 4E 08    JSR $084E       ;
70BE    29 07       AND #$07        ;
70C0    D0 0F       BNE ---         ;
70C2    20 95 7E    JSR $7E95       ;
70C5    AA          TAX             ;
70C6    BD 48 7E    LDA $7E48,X     ;
70C9    20 FD 7E    JSR $7EFD       ;
70CC    18          CLC             ;
70CD    7D 48 7E    ADC $7E48,X     ;
70D0    4A          LSR A           ;

;;

70D1    85 EA       STA $EA         ;
70D3    8D 6C 7E    STA $7E6C       ;
70D6    4A          LSR A           ;
70D7    C5 0F       CMP $0F         ;
70D9    90 09       BCC ---         ;
70DB    A5 0F       LDA $0F         ;
70DD    0A          ASL A           ;
70DE    20 FD 7E    JSR $7EFD       ;
70E1    4C D1 70    JMP $70D1       ;

70E4    20 4E 08    JSR $084E       ; random number
70E7    29 0F       AND #$0F        ;    0-15
70E9    AA          TAX             ;
70EA    BD 00 EF    LDA $EF00,X     ; load _EF00[X]
70ED    D0 F5       BNE $70E4       ; loop until hit a zero

70EF    BD 40 02    LDA $0240,X     ; load $024X
70F2    9D 00 EF    STA $EF00,X     ; and store in $EF0X
70F5    BD 50 02    LDA $0250,X     ; load $0250
70F8    9D 10 EF    STA $EF10,X     ; and store in $EF1X
70FB    A5 E6       LDA $E6         ; get _E6
70FD    10 30       BPL ---         ;
70FF    A5 EA       LDA $EA         ;
7101    F0 0B       BEQ ---         ;
7103    20 4E 08    JSR $084E       ;
7106    29 1F       AND #$1F        ;
7108    F0 15       BEQ ---         ;
710A    29 07       AND #$07        ;
710C    F0 05       BEQ ---         ;
710E    A5 E6       LDA $E6         ;
7110    4C 2F 71    JMP $712F       ;

7113    A5 E6       LDA $E6         ;
7115    20 95 7E    JSR $7E95       ;
7118    A8          TAY             ;
7119    B9 24 7E    LDA $7E24,Y     ;
711C    4C 2F 71    JMP $712F       ;
711F    A5 E6       LDA $E6         ;
7121    20 95 7E    JSR $7E95       ;
7124    A8          TAY             ;
7125    B9 24 7E    LDA $7E24,Y     ;
7128    20 95 7E    JSR $7E95       ;
712B    A8          TAY             ;
712C    B9 24 7E    LDA $7E24,Y     ;
712F    9D 50 EF    STA $EF50,X     ;
7132    9D 60 EF    STA $EF60,X     ;
7135    20 95 7E    JSR $7E95       ;
7138    A8          TAY             ;
7139    B9 F0 7D    LDA $7DF0,Y     ;
713C    85 D8       STA $D8         ;
713E    20 FD 7E    JSR $7EFD       ;
7141    46 D8       LSR $D8         ;
7143    05 D8       ORA $D8         ;
7145    9D 40 EF    STA $EF40,X     ;
7148    C6 EA       DEC $EA         ;
714A    10 98       BPL ---         ;
714C    A5 0B       LDA $0B         ;
714E    C9 82       CMP #$82        ;
7150    F0 04       BEQ ---         ;
7152    A5 0F       LDA $0F         ;
7154    85 D4       STA $D4         ;
7156    20 DC 7E    JSR $7EDC       ;
7159    30 17       BMI ---         ;
715B    A6 D4       LDX $D4         ;
715D    CA          DEX             ;
715E    9D A0 EF    STA $EFA0,X     ;
7161    BD 60 02    LDA $0260,X     ;
7164    9D 80 EF    STA $EF80,X     ;
7167    BD 68 02    LDA $0268,X     ;
716A    9D 90 EF    STA $EF90,X     ;
716D    20 BB 7E    JSR $7EBB       ; A = 00 if the given player is alive (GPS) otherwise FF
7170    F0 07       BEQ ---         ;
7172    A6 D4       LDX $D4         ;
7174    A9 00       LDA #$00        ;
7176    9D 9F EF    STA $EF9F,X     ;
7179    C6 D4       DEC $D4         ;
717B    D0          BNE ---         ;


717C    D9 A9 02 20 20 03 2C 10 C0 A9 00 85 B8 A9 01 85
718C    D4 85 C5 20 BB 7E F0 0F A6 D4 BD 9F EF F0 05 A9
719C    00 9D 9F EF 4C 48 72 A6 D4 BD 9F EF F0 F6 C9 38
71AC    F0 F2 20 4B 08 20 01 85 20 05 87 20 30 08 20 21
71BC    08 8D D7 BA 00 20 2D 08 A0 1E B1 FE 8D 6E 7D 18
71CC    69 25 20 7E 08 20 21 08 8D 1E 00 20 00 08 D0 03
71DC    4C 45 41 C9 A0 D0 03 4C 45 41 C9 8D D0 03 4C C8
71EC    78 C9 8B D0 03 4C C8 78 C9 8A D0 03 4C E3 78 C9
71FC    AF D0 03 4C E3 78 C9 88 D0 03 4C 18 79 C9 95 D0
720C    03 4C FE 78 C9 C1 90 16 C9 DB B0 12 38 E9 C1 0A
721C    A8 B9 94 78 85 FE B9 95 78 85 FF 6C FE 00 4C 51
722C    41 A5 C5 85 D4 20 01 85 20 16 64 20 45 08 A5 C6
723C    C9 D1 D0 08 20 4E 08 30 03 4C 8F 71 20 2D 08 A0
724C    12 B1 FE C9 D3 D0 13 20 4E 08 29 07 D0 0C A9 C7
725C    91 FE 20 DC 7E A6 D4 9D 9F EF 20 0E 76 E6 D4 E6
726C    C5 A5 D4 C5 0F 90 05 F0 03 4C 7B 72 4C 8F 71 A9
727C    00 85 C5 20 4B 08 A9 0F 85 EA A6 EA BD 50 EF D0
728C    03 4C 1C 75 BD 70 EF F0 0F 20 4E 08 29 07 F0 03
729C    4C BE 74 A9 00 9D 70 EF BD 50 EF C9 EC D0 0B A9
72AC    CE 85 C6 A9 02 85 C7 4C E6 72 C9 DC D0 2C 20 4E
72BC    08 29 07 D0 25 A9 0B 20 FD 7E 85 06 A9 0B 20 FD
72CC    7E 85 07 20 8D 08 20 E4 7F 30 EA A6 EA A5 06 9D
72DC    00 EF A5 07 9D 10 EF 4C BE 74 BD 00 EF 85 FA BD
72EC    10 EF 85 FB A5 0F 85 D4 A9 FF 85 DB A9 00 85 D5
72FC    20 BB 7E D0 29 A6 D4 CA BD A0 EF F0 21 38 BD 80
730C    EF E5 FA 20 70 83 85 DA 38 BD 90 EF E5 FB 20 70
731C    83 18 65 DA C5 DB B0 06 85 DB A5 D4 85 D5 C6 D4
732C    D0 CE A5 D5 D0 03 4C BE 74 85 D4 AA CA 38 BD 80
733C    EF E5 FA 20 64 83 85 F8 38 BD 90 EF E5 FB 20 64
734C    83 85 F9 A6 EA BD 50 EF C9 AC D0 10 A9 3C 9D 60
735C    EF A5 DB C9 05 B0 27 A9 AC 9D 60 EF 20 4E 08 29
736C    03 D0 1B BD 50 EF 20 30 81 F0 13 C9 4E D0 06 A4
737C    C6 C0 CE F0 09 20 AB 81 20 4B 08 4C BE 74 20 4E
738C    08 29 03 D0 50 A6 EA BD 50 EF C9 FC F0 04 C9 B0
739C    D0 43 A5 C6 C9 CE F0 3D 20 4B 08 20 78 08 A2 80
73AC    A9 09 20 54 08 20 78 08 A9 08 85 D4 A6 D4 BD 9F
73BC    EF F0 1B 20 2D 08 A0 12 B1 FE C9 C7 D0 10 20 4E
73CC    08 30 0B A9 D3 91 FE A9 38 A6 D4 9D 9F EF C6 D4
73DC    D0 DA 4C BE 74 A6 EA BD 40 EF C9 18 B0 11 A5 F8
73EC    20 7B 83 85 F8 A5 F9 20 7B 83 85 F9 4C 1A 74 A5
73FC    DB C9 02 B0 19 A6 EA BD 50 EF C9 A8 D0 06 20 C9
740C    75 4C 17 74 C9 C8 D0 03 20 D7 75 4C F7 77 A6 EA
741C    BD 50 EF C9 AC D0 03 4C BE 74 C9 B0 D0 03 4C BE
742C    74 A9 02 85 F0 20 4E 08 30 18 A5 F8 F0 14 18 65
743C    FA 85 06 A5 FB 85 07 20 66 75 20 8A 08 20 E4 7F
744C    F0 4D A5 F9 F0 14 18 65 FB 85 07 A5 FA 85 06 20
745C    66 75 20 8A 08 20 E4 7F F0 35 A5 F8 F0 14 18 65
746C    FA 85 06 A5 FB 85 07 20 66 75 20 8A 08 20 E4 7F
747C    F0 1D 20 4E 08 20 64 83 85 F8 20 4E 08 20 64 83
748C    85 F9 C6 F0 D0 9F A5 DB C9 02 B0 26 4C F7 77 A6
749C    EA BD 00 EF 9D 20 EF BD 10 EF 9D 30 EF A5 06 C9
74AC    0B B0 77 9D 00 EF A5 07 C9 0B B0 6E 9D 10 EF 20
74BC    57 08 A9 00 85 D4 A6 EA BD 00 EF 85 06 BD 10 EF
74CC    85 07 20 8D 08 A6 EA C9 44 F0 36 C9 46 F0 27 C9
74DC    47 F0 06 C9 4C F0 1F D0 37 BD 50 EF 20 15 4F F0
74EC    2F 20 4E 08 DD 40 EF 90 27 A9 01 9D 70 EF A9 06
74FC    20 54 08 4C 1C 75 BD 50 EF C9 E8 F0 13 C9 F0 B0
750C    0F 20 4E 08 29 7F 85 DC A9 06 20 54 08 20 03 7C
751C    C6 EA 30 03 4C 86 72 4C AC 75 20 05 87 A6 EA BD
752C    50 EF 20 5E 84 20 21 08 C6 CC C5 C5 D3 A1 8D 00 Po ^. !.FLEES!..
753C    A6 EA BD 50 EF 20 DB 87 30 0E A0 01 A9 01 20 6C
754C    85 A0 03 A9 01 20 6C 85 A9 00 A6 EA 9D 50 EF 9D
755C    40 EF A9 08 20 54 08 4C BE 74 A5 C6 C9 CA F0 01
756C    60 A5 EA 48 20 FC 82 30 33 A9 4F 8D FD EF A5 06
757C    8D FE EF A5 07 8D FF EF 20 57 08 A9 06 20 54 08
758C    A9 00 8D FD EF 85 D4 20 4E 08 29 3F 85 DC 20 03
759C    7C 20 57 08 68 85 EA 68 68 4C BE 74 68 85 EA 60
75AC    20 B3 65 20 57 08 E6 E9 20 0E 76 A5 C7 F0 08 C6
75BC    C7 D0 04 A9 25 85 C6 20 45 08 4C 89 71 A9 25 20
75CC    0D 66 20 45 08 A9 08 20 54 08 60 20 4E 08 29 03
75DC    D0 2F 20 4E 08 29 3F 20 28 85 85 D8 F8 38 AD 14
75EC    ED E5 D8 8D 14 ED AD 13 ED E9 00 8D 13 ED B0 08
75FC    A9 00 8D 14 ED 8D 13 ED D8 A9 08 20 54 08 20 45
760C    08 60 A5 0B C9 81 F0 0F A2 0F BD 50 EF D0 08 CA
761C    10 F8 68 68 4C F9 76 A2 07 BD A0 EF D0 08 CA 10
762C    F8 68 68 4C 33 76 60 A5 0B C9 81 F0 37 A5 C0 20
763C    DB 87 10 1F 20 21 08 8D C2 C1 D4 D4 CC C5 A0 C9 [... !..BATTLE I
764C    D3 A0 CC CF D3 D4 A1 8D 00 A0 02 A9 02 20 80 85 S LOST!.. .). ..
765C    4C D0 77 A0 01 A9 02 20 6C 85 A0 03 A9 02 20 6C
766C    85 4C D0 77 20 21 08 8D CC C5 C1 D6 C5 A0 D2 CF .LPw !..LEAVE RO
767C    CF CD A1 8D 00 AD 7C 7A F0 0C 10 05 A9 03 4C 9E OM!..-|zp...).L.
768C    76 A9 01 4C 9E 76 AD 7D 7A 10 05 A9 00 4C 9E 76
769C    A9 02 85 10 A5 E8 85 0B A5 18 30 33 0A 0A 18 65
76AC    10 A8 B9 ED 76 85 0A A8 88 B9 12 52 85 08 B9 32
76BC    52 85 09 A9 00 20 20 03 20 05 51 20 21 08 C9 CE R..).  . .Q !.IN
76CC    D4 CF A0 C4 D5 CE C7 C5 CF CE 8D 00 20 52 52 20 TO DUNGEON.. RR
76DC    05 87 A9 01 20 20 03 A5 10 AA A9 00 85 B8 4C 44
76EC    43 11 16 17 14 12 14 17 15 13 15 17 16 20 21 08
76FC    8D D6 C9 C3 D4 CF D2 D9 A1 8D 00 A5 C0 20 DB 87 .VICTORY!..%@ [.
770C    10 0A A0 02 20 4E 08 29 01 20 6C 85 A5 0B C9 82
771C    D0 03 4C D0 77 A5 E8 C9 03 F0 69 A5 C0 C9 90 B0
772C    16 C9 80 F0 47 C9 20 90 58 C9 60 B0 54 C9 30 90
773C    06 C9 50 B0 02 90 4A C9 94 F0 46 C9 A0 F0 42 C9
774C    B8 F0 3E C9 B4 F0 3A C9 DC F0 36 A5 C3 20 48 08
775C    30 2F 90 2D 20 B2 77 A5 C1 9D 20 EE A5 C2 9D 40
776C    EE A9 3C 9D 60 EE 9D 00 EE 4C D0 77 20 B2 77 A5
777C    C1 9D 20 EE A5 C2 9D 40 EE A9 10 9D 60 EE 9D 00
778C    EE 4C D0 77 A5 C0 C9 94 F0 F7 C9 B4 F0 F3 C9 A0
779C    F0 EF A5 00 85 06 A5 01 85 07 20 96 08 D0 E2 A9
77AC    40 91 FE 4C D0 77 A2 1F BD 60 EE F0 16 CA 30 0A
77BC    A5 E8 C9 01 D0 F2 E0 08 B0 EE A9 18 20 FD 7E 18
77CC    69 08 AA 60 A5 0B C9 82 F0 17 A5 E8 85 0B C9 03
77DC    D0 03 20 06 8C A9 01 20 20 03 A9 00 85 B8 4C 84
77EC    63 A5 E8 85 0B A9 01 20 20 03 60 A6 D4 BD 7F EF
77FC    8D FE EF BD 8F EF 8D FF EF A9 4D 8D FD EF 20 57
780C    08 A9 05 20 54 08 A9 00 8D FD EF A5 C6 C9 D0 D0
781C    05 20 4E 08 30 14 20 2D 08 A9 1F B1 FE AA BD 7D
782C    7E 85 DA 20 4E 08 C5 DA B0 06 20 57 08 4C BE 74
783C    20 42 78 4C BE 74 A9 4F 8D FD EF 20 01 85 20 57
784C    08 A9 07 20 54 08 A9 00 8D FD EF 20 57 08 A6 EA
785C    BD 50 EF 20 95 7E AA BD F0 7D 4A 4A 20 FD 7E 20
786C    28 85 20 BE 85 D0 1A 20 30 08 20 21 08 8D C9 D3 (. >.P. 0. !..IS
787C    A0 CB C9 CC CC C5 C4 A1 8D 00 A0 04 A9 01 20 6C  KILLED!.. .). l
788C    85 20 01 85 20 45 08 60 7E 7A 51 41 71 7D 51 41
789C    51 41 51 41 7E 7D 51 41 51 41 51 41 51 41 51 41
78AC    51 41 51 41 51 41 51 41 51 41 C5 7D 51 41 51 41
78BC    55 5C 92 5C 51 41 51 41 51 41 DA 7D 20 21 08 CE U\.\QAQAQAZ} !.N
78CC    EF F2 F4 E8 8D 00 A6 D4 CA BD 80 EF 85 06 BC 90 orth..&TJ=.o..<.
78DC    EF 88 84 07 4C 32 79 20 21 08 D3 EF F5 F4 E8 8D o...L2y !.South.
78EC    00 A6 D4 CA BD 80 EF 85 06 BC 90 EF C8 84 07 4C .&TJ=.o..<.oH..L
78FC    32 79 20 21 08 C5 E1 F3 F4 8D 00 A6 D4 CA BD 90 2y !.East..&TJ=.
790C    EF 85 07 BC 80 EF C8 84 06 4C 32 79 20 21 08 D7 o..<.oH..L2y !.W
791C    E5 F3 F4 8D 00 A6 D4 CA BD 90 EF 85 07 BC 80 EF est..&TJ=.o..<.o
792C    88 84 06 4C 32 79 A9 00 20 54 08 A5 06 C9 0B 90
793C    03 4C E2 79 A5 07 C9 0B 90 03 4C E2 79 20 8A 08
794C    20 48 08 10 03 4C 1D 42 20 8A 08 20 BC 46 F0 06
795C    20 A3 41 4C 2D 72 A6 D4 CA A5 06 9D 80 EF A5 07
796C    9D 90 EF A9 00 20 54 08 A5 0B C9 81 D0 47 A2 00
797C    86 D8 BD 10 03 F0 32 BD 11 03 F0 2D 4A 4A 4A 4A
798C    C5 06 D0 25 BD 11 03 29 0F C5 07 D0 1C BD 12 03
799C    F0 09 20 C4 79 BD 10 03 99 80 02 BD 13 03 F0 09
79AC    20 C4 79 BD 10 03 99 80 02 A5 D8 18 69 04 85 D8
79BC    AA C9 10 90 BD 4C 2D 72 48 4A 4A 4A 4A 85 D9 68
79CC    29 0F A8 B9 D7 79 18 65 D9 A8 60 00 0B 16 21 2C
79DC    37 42 4D 58 63 6E A5 0B C9 81 D0 54 AD 7C 7A D0
79EC    19 AD 7D 7A D0 1A A5 06 C9 0B 90 06 8D 7C 7A 4C
79FC    3C 7A A5 07 8D 7D 7A 4C 3C 7A C5 06 F0 32 D0 06
7A0C    C5 07 F0 2C D0 00 68 68 20 21 08 C1 CC CC A0 CD E.p,P.hh !.ALL M
7A1C    D5 D3 D4 A0 D5 D3 C5 8D D4 C8 C5 A0 D3 C1 CD C5 UST USE.THE SAME
7A2C    A0 C5 D8 C9 D4 A1 8D 00 A9 01 20 54 08 4C 2D 72  EXIT!..). T.L-r
7A3C    A6 D4 CA A9 80 9D 80 EF 9D 90 EF A9 00 9D A0 EF
7A4C    A9 08 20 54 08 A5 0B C9 80 D0 22 A5 C0 20 DB 87
7A5C    10 1B 20 2D 08 A0 18 B1 FE A0 1A D1 FE D0 0E A0
7A6C    02 A9 02 20 80 85 A0 04 A9 02 20 80 85 4C 2D 72
7A7C    00 00 A9 00 8D 70 7D 20 21 08 C1 F4 F4 E1 E3 EB ..)..p} !.Attack
7A8C    AD 00 20 81 83 D0 03 4C 45 41 A5 06 8D FE EF A5
7A9C    07 8D FF EF AE 6E 7D BD 85 7E D0 4A E0 0A D0 12
7AAC    18 AD FE EF 65 F8 8D FE EF 18 AD FF EF 65 F9 8D
7ABC    FF EF 20 1A 83 10 03 4C F4 7C A9 4D 8D FD EF 20
7ACC    57 08 A9 00 8D FD EF A9 04 20 54 08 20 FC 82 10
7ADC    0F A9 01 8D 70 7D AD 6E 7D C9 02 F0 09 4C F4 7C
7AEC    EE 70 7D 4C 86 7B A9 0B 8D 6F 7D AD 6E 7D C9 09
7AFC    D0 1A 20 21 08 D2 C1 CE C7 C5 AD 00 20 9A 84 38 P. !.RANGE-. ..8
7B0C    E9 B0 C9 0A 90 03 4C F4 7C 8D 6F 7D AD 6E 7D C9
7B1C    02 F0 04 C9 09 D0 28 18 69 20 A8 B9 00 ED F0 06
7B2C    20 5E 85 4C 4B 7B A9 00 20 2D 08 A0 1E A9 00 91
7B3C    FE 20 21 08 CC C1 D3 D4 A0 CF CE C5 A1 8D 00 20 ~ !.LAST ONE!..
7B4C    1A 83 10 03 4C F4 7C A9 4D 8D FD EF AD 6E 7D C9
7B5C    0E D0 05 A9 4E 8D FD EF 20 57 08 A9 00 8D FD EF
7B6C    AD 70 7D D0 05 A9 04 20 54 08 EE 70 7D 20 FC 82
7B7C    10 08 CE 6F 7D D0 C8 4C F4 7C A5 0A C9 18 D0 0A
7B8C    AD 6E 7D C9 0B B0 03 4C F4 7C AD 6E 7D C9 09 D0
7B9C    0C 20 8D 08 C9 03 90 05 A9 46 99 80 02 A9 4F 8D
7BAC    FD EF AD 6E 7D C9 0E D0 05 A9 4E 8D FD EF 20 2D
7BBC    08 A0 14 B1 FE 20 40 85 0A 18 69 80 B0 0C 85 DA
7BCC    20 4E 08 C5 DA 90 03 4C F4 7C 20 2D 08 A0 13 B1
7BDC    FE 20 40 85 AE 6E 7D 18 7D 6D 7E 90 02 A9 FF 20
7BEC    FD 7E 85 DC 20 57 08 A9 06 20 54 08 A9 00 8D FD
7BFC    EF 20 03 7C 4C 37 7D 20 05 87 A6 EA BD 50 EF 85
7C0C    E6 20 5E 84 A6 EA BD 50 EF C9 5E D0 03 4C 71 7C
7C1C    BD 40 EF 38 E5 DC 9D 40 EF B0 4A A9 00 9D 40 EF
7C2C    9D 50 EF 20 21 08 CB C9 CC CC C5 C4 A1 8D 00 A5 .Po !.KILLED!..%
7C3C    D4 F0 2E A5 E6 20 95 7E A8 B9 F0 7D 4A 4A 4A 4A
7C4C    18 69 01 20 28 85 20 58 86 20 21 08 C5 D8 D0 AE .i. (. X. !.EXP.
7C5C    AB 00 A5 D8 C9 10 B0 06 20 66 08 4C 6D 7C 20 33 +.%XI.0. f.Lm| 3
7C6C    08 20 05 87 60 A6 EA BD 50 EF 20 95 7E AA BD F0
7C7C    7D 4A 85 D9 85 DA 46 DA 18 65 DA 85 D8 A6 EA BD
7C8C    40 EF C9 18 B0 10 20 21 08 C6 CC C5 C5 C9 CE C7 @oI.0. !.FLEEING
7C9C    A1 8D 00 4C F3 7C C5 DA B0 11 20 21 08 C3 D2 C9 !..Ls|EZ0. !.CRI
7CAC    D4 C9 C3 C1 CC A1 8D 00 4C F3 7C C5 D9 B0 0E 20 TICAL!..Ls|EY0.
7CBC    21 08 C8 C5 C1 D6 C9 CC D9 00 4C E5 7C C5 D8 B0 !.HEAVILY.Le|EX0
7CCC    0E 20 21 08 CC C9 C7 C8 D4 CC D9 00 4C E5 7C 20 . !.LIGHTLY.Le|
7CDC    21 08 C2 C1 D2 C5 CC D9 00 20 21 08 A0 D7 CF D5 !.BARELY. !. WOU
7CEC    CE C4 C5 C4 A1 8D 00 60 AD 70 7D D0 05 A9 04 20 NDED!..`-p}P.).
7CFC    54 08 A9 00 8D FD EF 20 21 08 CD C9 D3 D3 C5 C4 T.)..}o !.MISSED
7D0C    A1 8D 00 AD 6E 7D C9 09 D0 1E AD FE EF C9 0B B0 !..-n}I.P.-~oI.0
7D1C    17 85 06 AD FF EF C9 0B B0 0E 85 07 20 8D 08 C9
7D2C    03 90 05 A9 46 99 80 02 4C 37 7D AD 6E 7D C9 0B
7D3C    D0 2D AD 70 7D C9 02 90 26 CE 70 7D A5 F8 20 7B
7D4C    83 85 F8 A5 F9 20 7B 83 85 F9 A9 4D 8D FD EF 20
7D5C    1A 83 20 57 08 CE 70 7D D0 F5 A9 00 8D FD EF 4C
7D6C    2D 72 00 00 00 20 21 08 C3 E1 F3 F4 AC A0 00 4C -r... !.Cast, .L
7D7C    B1 48 20 21 08 C7 E5 F4 A0 E3 E8 E5 F3 F4 A1 8D 1H !.Get chest!.
7D8C    00 A6 D4 CA BD 80 EF 85 06 BD 90 EF 85 07 20 8D .&TJ=.o..=.o.. .
7D9C    08 C9 3C F0 03 4C B7 41 A9 16 99 80 02 A5 E8 C9
7DAC    03 D0 13 A5 00 85 06 A5 01 85 07 20 96 08 C9 40
7DBC    D0 04 A9 00 91 FE 4C 42 54 20 21 08 D2 E5 E1 E4 P.)..~LBT !.Read
7DCC    F9 A0 F7 E5 E1 F0 EF EE BA 8D 00 4C F8 59 20 21 y weapon:..LxY !
7DDC    08 DA F4 E1 F4 F3 8D 00 20 2E 5E 20 45 08 20 01 .Ztats.. .^ E. .
7DEC    85 4C 2D 72 FF FF 40 60 80 60 FF FF 30 30 40 50
7DFC    30 60 30 C0 FF 30 F0 80 50 30 50 30 70 40 80 40
7E0C    B0 C0 60 F0 70 D0 E0 FF 70 30 60 40 60 80 90 30
7E1C    80 30 30 30 20 20 80 FF

;; DATA

7E24    C8 C8 8A 88 86 84 8C 8E
7E2C    C4 E4 90 E4 A0 D0 A8 AC B0 90 BC 9C A4 E0 C8 90
7E3C    F0 B8 EC BC F0 F0 F4 B8 FC F8 FC FC

;; DATA

7E48    01 01 0C 04
7E4C    04 08 01 01 0C 0C 06 04 0F 06 0F 01 01 0F 04 08
7E5C    0A 0C 0A 0C 06 08 06 0C 06 04 08 04 06 04 04 01

;; DATA

7E6C    00 08 10 18 20 28 30 40 28 38 40 60 60 80 50 A0
7E7C    FF 60 80 90 A0 B0 C0 D0 F8 00 00 00 FF 00 00 00
7E8C    FF FF FF 00 FF 00 FF FF 00

;;

7E95    10 0C       BPL ---         ;
7E97    29 7F       AND #$7F        ;
7E99    4A          LSR A           ;
7E9A    C9 08       CMP #$08        ;
7E9C    90 04       BCC ---         ;
7E9E    4A          LSR A           ;
7E9F    18          CLC             ;
7EA0    69 04       ADC #$04        ;
7EA2    60          RTS             ;

7EA3    29 1F       AND #$1F        ;
7EA5    18          CLC             ;
7EA6    69 24       ADC #$24        ;
7EA8    60          RTS             ;

;;
; return 00 if the given player is awake (GP) otherwise FF

7EA9    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
7EAC    A0 12       LDY #$12        ; get player attribute 0x12 (player status)
7EAE    B1 FE       LDA ($FE),Y     ; .
7EB0    C9 C7       CMP #$C7        ; if 'G'
7EB2    F0 1D       BEQ $7ED1       ;   go to $7ED1
7EB4    C9 D0       CMP #$D0        ; if 'P'
7EB6    F0 19       BEQ $7ED1       ;   go to $7ED1
7EB8    A9 FF       LDA #$FF        ; .
7EBA    60          RTS             ; return with A = 0xFF

;;
; returns 00 if the given player is alive (GPS) otherwise FF

7EBB    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
7EBE    A0 12       LDY #$12        ; get player attribute 0x12 (player status)
7EC0    B1 FE       LDA ($FE),Y     ; .
7EC2    C9 C7       CMP #$C7        ; if 'G'
7EC4    F0 0B       BEQ $7ED1       ;    go to $7ED1

7EC6    C9 D0       CMP #$D0        ; if 'P'
7EC8    F0 07       BEQ $7ED1       ;    go to $7ED1
7ECA    C9 D3       CMP #$D3        ; if 'S'
7ECC    F0 03       BEQ $7ED1       ;   go to $7ED1
7ECE    A9 FF       LDA #$FF        ; .
7ED0    60          RTS             ; return with A = 0xFF

7ED1    A5 D4       LDA $D4         ; get current player
7ED3    C5 0F       CMP $0F         ; compare to party size
7ED5    F0 02       BEQ $7ED9       ; if last player, skip (return 0x00)

7ED7    B0 F5       BCS $7ECE       ; current player >= party size (return 0xFF)

7ED9    A9 00       LDA #$00        ; .
7EDB    60          RTS             ; return with A = 0x00

7EDC    20 2D 08    JSR $082D       ; ; set $FE/$FF vector based on $D4
7EDF    A0 12       LDY #$12        ; get player status
7EE1    B1 FE       LDA ($FE),Y     ; .
7EE3    C9 C7       CMP #$C7        ; if 'G'
7EE5    F0 0B       BEQ $7EF2       ;   go to $7EF2
7EE7    C9 D0       CMP #$D0        ; if 'P'
7EE9    F0 07       BEQ ---         ;   go to $7EF2
7EEB    C9 C4       CMP #$C4        ; if 'D'
7EED    F0 0B       BEQ ---         ;   go to $7EFA
7EEF    A9 38       LDA #$38        ; else return with A = 0x38
7EF1    60          RTS             ;

7EF2    A0 11       LDY #$11        ; get player attribute 0x11
7EF4    B1 FE       LDA ($FE),Y     ;
7EF6    0A          ASL A           ; double it
7EF7    69 20       ADC #$20        ; and add 20
7EF9    60          RTS             ; return it

7EFA    A9 FF       LDA #$FF        ; return 0xFF

7EFC    60 C9 00 F0 14 8D 16 7F 20 4E 08 CD 16 7F 90 07
7F0C    38 ED 16 7F 4C 07 7F C9 00 60 00 85 DA 86 D9 20
7F1C    BC 46 10 03 4C DF 7F BD 60 EE C9 8C F0 3C C9 8E
7F2C    F0 38 A0 1F B9 60 EE F0 18 B9 20 EE C5 FA D0 11
7F3C    B9 40 EE C5 FB D0 0A B9 60 EE C9 3C F0 03 4C DF
7F4C    7F 88 10 E0 A5 FA C5 00 D0 09 A5 FB C5 01 D0 03
7F5C    4C DF 7F BD 60 EE C9 80 F0 15 A5 FA DD 80 EE D0
7F6C    0E A5 FB DD A0 EE D0 07 20 4E 08 29 03 D0 64 BD
7F7C    60 EE 10 30 C9 8E 90 35 F0 47 C9 94 F0 43 C9 9C
7F8C    F0 16 C9 B4 F0 3B C9 EC F0 0E C9 F0 F0 33 C9 F8
7F9C    F0 2F C9 FC F0 2B D0 0C A5 DA C9 03 90 35 C9 45
7FAC    D0 2C F0 2F A5 DA 20 48 08 10 23 30 26 C9 80 D0
7FBC    08 A5 DA C9 02 90 17 B0 1A A5 DA C9 03 90 0F B0
7FCC    12 A5 DA C9 03 90 07 20 48 08 10 02 30 05 A6 D9
7FDC    A9 00 60 A6 D9 A9 FF 60 85 DA 20 BC 46 30 15 A5
7FEC    06 C9 0B B0 06 A5 07 C9 0B 90 0F A6 EA BD 40 EF
7FFC    C9 18 90 03 4C A7 80 4C A4 80 A6 EA BD 20 EF C5
800C    06 D0 0E BD 30 EF C5 07 D0 07 20 4E 08 29 03 D0
801C    E3 A2 0F BD 50 EF F0 0E BD 00 EF C5 06 D0 07 BD
802C    10 EF C5 07 F0 75 E0 08 B0 13 BD A0 EF F0 0E BD
803C    80 EF C5 06 D0 07 BD 90 EF C5 07 F0 5E CA 10 D3
804C    A6 EA BD 50 EF 10 30 C9 8E 90 35 F0 3B C9 94 F0
805C    37 C9 9C F0 16 C9 B4 F0 2F C9 EC F0 0E C9 F0 F0
806C    27 C9 F8 F0 23 C9 FC F0 1F D0 0C A5 DA C9 03 90
807C    2A C9 45 D0 23 F0 24 A5 DA 20 48 08 10 1A 30 1B
808C    A5 DA C9 03 90 12 B0 13 A5 DA C9 03 90 0A 20 48
809C    08 20 48 08 10 02 30 03 A9 00 60 A9 FF 60 48 29
80AC    0F D0 2F 68 29 F0 C9 80 F0 29 C9 90 F0 25 C9 A0
80BC    F0 21 C9 D0 F0 1D C9 F0 F0 19 20 4E 08 29 07 F0
80CC    0E A5 06 DD 80 EE D0 07 A5 07 DD A0 EE F0 04 A9
80DC    00 60 68 A9 FF 60 85 D8 29 F0 C9 C0 F0 14 C9 E0
80EC    F0 10 C9 D0 F0 0C A5 D8 C9 A1 F0 09 C9 C0 B0 05
80FC    90 00 A9 00 60 A9 FF 60 A5 0B C9 02 D0 1D AD 2F
810C    81 D0 01 60 CE 2F 81 F0 01 60 AD 2D 81 85 FA AD
811C    2E 81 85 FB 20 93 08 A9 3B 91 FC A9 00 8D 2F 81
812C    60 00 00 00 C9 20 F0 62 C9 5E F0 5E C9 84 F0 63
813C    C9 86 F0 4A C9 88 F0 55 C9 8A F0 4E C9 98 F0 3B
814C    C9 A4 F0 4F C9 AC F0 33 C9 B8 F0 38 C9 CC F0 2B
815C    C9 D0 F0 33 C9 D8 F0 2F C9 E0 F0 2E C9 E4 F0 2A
816C    C9 E8 F0 2C C9 F0 F0 22 C9 F4 B0 21 C9 F8 F0 1D
817C    C9 B0 F0 22 C9 FC F0 1E A9 00 60 A9 44 60 A9 45
818C    60 A9 46 60 A9 47 60 A9 37 60 A9 4E 60 A9 4F 60
819C    A9 4C 60 A9 4D 60 20 4E 08 29 03 18 69 44 60 8D
81AC    FD EF BD 00 EF 8D FE EF BD 10 EF 8D FF EF A9 03
81BC    20 54 08 20 1A 83 30 0B 20 D8 82 D0 26 20 57 08
81CC    4C BF 81 AD FD EF A2 00 8E FD EF C9 4C F0 01 60
81DC    AD FE EF 85 06 AD FF EF 85 07 20 8A 08 A9 4C 99
81EC    80 02 60 20 05 87 20 30 08 20 05 87 AD FD EF C9
81FC    44 D0 26 20 21 08 D0 CF C9 D3 CF CE C5 C4 A1 8D DP& !.POISONED!.
820C    00 20 BA 82 20 4E 08 10 51 20 2D 08 A0 12 B1 FE . :. N..Q -. .1~
821C    C9 C7 D0 46 A9 D0 91 FE 60 C9 45 D0 15 20 21 08
822C    C5 CC C5 C3 D4 D2 C9 C6 C9 C5 C4 A1 8D 00 20 42 ELECTRIFIED!.. B
823C    78 60 C9 46 D0 0D 20 21 08 C6 C9 C5 D2 D9 A0 00 x`IFP. !.FIERY .
824C    4C AD 82 C9 47 D0 37 20 21 08 D3 CC C5 D0 D4 A1 L-.IGP7 !.SLEPT!
825C    8D 00 20 BA 82 20 4E 08 30 0D 20 21 08 C6 C1 C9 .. :. N.0. !.FAI
826C    CC C5 C4 AE 8D 00 60 20 2D 08 A0 12 B1 FE C9 C7 LED...` -. .1~IG
827C    D0 E8 A9 D3 91 FE A9 38 A6 D4 9D 9F EF 60 C9 4C
828C    D0 0C 20 21 08 CC C1 D6 C1 A0 00 4C AD 82 C9 4E P. !.LAVA .L-.IN
829C    D0 0F 20 21 08 CD C1 C7 C9 C3 C1 CC A0 00 4C AD P. !.MAGICAL .L-
82AC    82 20 21 08 C8 C9 D4 A1 8D 00 20 42 78 60 20 01 . !.HIT!.. Bx` .
82BC    85 A5 06 8D FE EF A5 07 8D FF EF 20 57 08 A9 07
82CC    20 54 08 20 01 85 A9 00 8D FD EF 60 A5 0F 85 D4
82DC    20 BB 7E D0 16 A6 D4 CA AD FE EF DD 80 EF D0 0B
82EC    AD FF EF DD 90 EF D0 03 A5 D4 60 C6 D4 D0 E1 60
82FC    A0 0F B9 50 EF F0 0E B9 00 EF C5 06 D0 07 B9 10
830C    EF C5 07 F0 03 88 10 EA 98 85 EA C9 00 60 18 AD
831C    FE EF 65 F8 85 06 18 AD FF EF 65 F9 85 07 A5 06
832C    C9 0B B0 31 A5 07 C9 0B B0 2B 20 8D 08 30 19 C9
833C    03 90 15 C9 48 F0 11 C9 49 F0 1A C9 31 90 04 C9
834C    35 90 05 20 48 08 30 0D A5 06 8D FE EF A5 07 8D
835C    FF EF A9 00 60 A9 FF 60 C9 00 F0 07 30 03 A9 01
836C    60 A9 FF 60 C9 00 30 01 60 49 FF 18 69 01 60 49
837C    FF 18 69 01 60 20 96 83 F0 0F A6 D4 CA BD 80 EF
838C    85 06 BD 90 EF 85 07 A9 01 60

8396    20 00 08    JSR $0800       ;
8399    D0 06       BNE $83A1       ;
839B    20 05 87    JSR $8705       ; print newline
839E    A9 00       LDA #$00        ;
83A0    60          RTS             ;

;;

83A1    48          PHA             ;
83A2    A9 00       LDA #$00        ;
83A4    85 F8       STA $F8         ;
83A6    85 F9       STA $F9         ;
83A8    68          PLA             ;
83A9    C9 8D       CMP #$8D        ;
83AB    F0 1E       BEQ ---         ;
83AD    C9 8B       CMP #$8B        ;
83AF    F0 1A       BEQ ---         ;
83B1    C9 AF       CMP #$AF        ;
83B3    F0 20       BEQ ---         ;
83B5    C9 8A       CMP #$8A        ;
83B7    F0 1C       BEQ ---         ;
83B9    C9 88       CMP #$88        ;
83BB    F0 22       BEQ ---         ;
83BD    C9 95       CMP #$95        ;
83BF    F0 28       BEQ ---         ;
83C1    C9 A0       CMP #$A0        ;
83C3    F0 D6       BEQ ---         ;
83C5    20 24 08    JSR $0824       ;
83C8    4C 9B 83    JMP $839B       ;

83CB    20 05 84    JSR $8405       ; print 'North^M'
83CE    A9 FF       LDA #$FF        ;
83D0    85 F9       STA $F9         ;
83D2    4C F0 83    JMP $83F0       ;

83D5    20 10 84    JSR $8410       ; print 'South^M'
83D8    A9 01       LDA #$01        ;
83DA    85 F9       STA $F9         ;
83DC    4C F0 83    JMP $83F0       ;
83DF    20 25 84    JSR $8425       ; print 'West^M'
83E2    A9 FF       LDA #$FF        ;
83E4    85 F8       STA $F8         ;
83E6    4C F0 83    JMP $83F0       ;
83E9    20 1B 84    JSR $841B       ; print 'East^M'
83EC    A9 01       LDA #$01        ;
83EE    85 F8       STA $F8         ;

83F0    18          CLC             ;
83F1    A5 00       LDA $00         ;
83F3    65 F8       ADC $F8         ;
83F5    85 FA       STA $FA         ;
83F7    85 06       STA $06         ;
83F9    18          CLC             ;
83FA    A5 01       LDA $01         ;
83FC    65 F9       ADC $F9         ;
83FE    85 FB       STA $FB         ;
8400    85 07       STA $07         ;
8402    A9 01       LDA #$01        ;
8404    60          RTS             ;

;; print 'North^M'

8405    20 21 08    JSR $0821       ; print

8408    CE EF F2 F4 E8 8D 00                                                    North^M^@

840F    60          RTS             ;

;; print 'South^M'

8410    20 21 08    JSR $0821       ; print

8413    D3 EF F5 F4 E8 8D 00                                                    South^M^@

841A    60          RTS             ;

;; print 'East^M'

841B    20 21 08    JSR $0821       ;

841E    C5 E1 F3 F4 8D 00                                                       East^M^@

8424    60          RTS             ;

;; print 'West^M'

8425    20 21 08    JSR $0821       ;

8428    D7 E5 F3 F4 8D 00                                                       West^M^@

842E    60          RTS             ;

;;

842F    A5 0A       LDA $0A         ;
8431    D0 05       BNE $8438       ;
8433    A2 07       LDX #$07        ;
8435    4C 3E 84    JMP $843E       ;

8438    C9 11       CMP #$11        ;
843A    B0 1A       BCS ---         ;
843C    A2 1F       LDX #$1F        ;
843E    A5 FA       LDA $FA         ;
8440    DD 20 EE    CMP $EE20,X     ;
8443    D0 0E       BNE ---         ;
8445    A5 FB       LDA $FB         ;
8447    DD 40 EE    CMP $EE40,X     ;
844A    D0 07       BNE ---         ;
844C    BD 60 EE    LDA $EE60,X     ;
844F    F0 02       BEQ ---         ;
8451    8A          TXA             ;
8452    60          RTS             ;

8453    CA          DEX             ;
8454    10 E8       BPL ---         ;
8456    A9 FF       LDA #$FF        ;
8458    60          RTS             ;

8459    A6 D8       LDX $D8         ;
845B    BD 60 EE    LDA $EE60,X     ;
845E    10 1C       BPL ---         ;
8460    38          SEC             ;
8461    E9 80       SBC #$80        ;
8463    C9 10       CMP #$10        ;
8465    B0 0D       BCS ---         ;
8467    4A          LSR A           ;
8468    18          CLC             ;
8469    69 01       ADC #$01        ;
846B    20 7E 08    JSR $087E       ;
846E    A9 8D       LDA #$8D        ;
8470    20 24 08    JSR $0824       ;
8473    60          RTS             ;

8474    4A          LSR A           ;
8475    4A          LSR A           ;
8476    18          CLC             ;
8477    69 04       ADC #$04        ;
8479    4C 68 84    JMP $8468       ;

847C    C9 20       CMP #$20        ;
847E    90 15       BCC ---         ;
8480    C9 60       CMP #$60        ;
8482    B0 11       BCS ---         ;
8484    C9 30       CMP #$30        ;
8486    90 04       BCC ---         ;
8488    C9 50       CMP #$50        ;
848A    90 09       BCC ---         ;
848C    29 1F       AND #$1F        ;
848E    4A          LSR A           ;
848F    18          CLC             ;
8490    69 4C       ADC #$4C        ;
8492    4C 68 84    JMP $8468       ;

8495    A9 13       LDA #$13        ;
8497    4C 68 84    JMP $8468       ;

849A    20 00 08    JSR $0800       ;
849D    48          PHA             ;
849E    F0 03       BEQ ---         ;
84A0    20 24 08    JSR $0824       ;
84A3    A9 8D       LDA #$8D        ;
84A5    20 24 08    JSR $0824       ;
84A8    68          PLA             ;
84A9    60          RTS             ;

;; maybe checks to see if anyone is awake in the party

84AA    A5 0F       LDA $0F         ; get party size
84AC    85 D4       STA $D4         ; and set current party member to last in party
84AE    20 BB 7E    JSR $7EBB       ; A = 00 if the given player is alive (GPS) otherwise FF
84B1    F0 07       BEQ $84BA       ; if zero, skip
84B3    C6 D4       DEC $D4         ; go to earlier player
84B5    D0 F7       BNE $84AE       ; loop back
84B7    4C C9 84    JMP $84C9       ; jump to $84C9

84BA    20 A9 7E    JSR $7EA9       ; A = 00 if the given player is awake (GP) otherwise FF
84BD    F0 07       BEQ $84C6       ; if zero, skip
84BF    C6 D4       DEC $D4         ; go to earlier player
84C1    D0 F7       BNE $84BA       ; loop back
84C3    A9 FF       LDA #$FF        ; return 0xFF
84C5    60          RTS             ; .

84C6    A9 00       LDA #$00        ; return 0x00
84C8    60          RTS             ; .

;; death?

84C9    A9 00       LDA #$00        ; ???
84CB    20 20 03    JSR $0320       ; ???
84CE    A9 02       LDA #$02        ; request disk 2 (BRITANNIA)
84D0    20 42 08    JSR $0842       ; .
84D3    20 1B 08    JSR $081B       ;

84D6    84 C2 CC CF C1 C4 A0 D3 C1 D6 C5 AC C1 A4 B8 B8 B0 B0 8D 00             ^DBLOAD SAVE,A$8800^M^@

84EA    20 00 88    JSR $8800       ; call SAVE

84ED    A9 01       LDA #$01        ; ???
84EF    20 20 03    JSR $0320       ; ???
84F2    4C 84 63    JMP $6384       ; return to main loop

84F5    A5 0F       LDA $0F         ;
84F7    85 D4       STA $D4         ;
84F9    20 01 85    JSR $8501       ;
84FC    C6 D4       DEC $D4         ;
84FE    D0 F9       BNE ---         ;
8500    60          RTS             ;

;;

8501    A5 D4       LDA $D4         ;
8503    0A          ASL A           ;
8504    0A          ASL A           ;
8505    0A          ASL A           ;
8506    AA          TAX             ;
8507    A9 08       LDA #$08        ;
8509    85 F0       STA $F0         ;
850B    BD 00 E0    LDA $E000,X     ;
850E    85 FE       STA $FE         ;
8510    BD C0 E0    LDA $E0C0,X     ;
8513    85 FF       STA $FF         ;
8515    A0 26       LDY #$26        ;
8517    B1 FE       LDA ($FE),Y     ;
8519    49 7F       EOR #$7F        ;
851B    91 FE       STA ($FE),Y     ;
851D    88          DEY             ;
851E    C0 18       CPY #$18        ;
8520    B0 F5       BCS ---         ;
8522    E8          INX             ;
8523    C6 F0       DEC $F0         ;
8525    D0 E4       BNE ---         ;
8527    60          RTS             ;

8528    C9 00       CMP #$00        ;
852A    F0 12       BEQ ---         ;
852C    C9 63       CMP #$63        ;
852E    B0 0C       BCS ---         ;
8530    F8          SED             ;
8531    AA          TAX             ;
8532    A9 00       LDA #$00        ;
8534    18          CLC             ;
8535    69 01       ADC #$01        ;
8537    CA          DEX             ;
8538    D0 FA       BNE ---         ;
853A    F0 02       BEQ ---         ;
853C    A9 99       LDA #$99        ;
853E    D8          CLD             ;
853F    60          RTS             ;

8540    C9 00       CMP #$00        ;
8542    F0 0B       BEQ ---         ;
8544    A2 00       LDX #$00        ;
8546    F8          SED             ;
8547    E8          INX             ;
8548    38          SEC             ;
8549    E9 01       SBC #$01        ;
854B    D0 FA       BNE ---         ;
854D    8A          TXA             ;
854E    D8          CLD             ;
854F    60          RTS             ;

;; increment _ED00[Y] by 1 if not at max

8550    F8          SED             ;
8551    18          CLC             ;
8552    B9 00 ED    LDA $ED00,Y     ;
8555    69 01       ADC #$01        ;
8557    B0 03       BCS $855C       ;
8559    99 00 ED    STA $ED00,Y     ;
855C    D8          CLD             ;
855D    60          RTS             ;

;; decrement _ED00[Y] by 1, @@@

855E    F8          SED             ;
855F    38          SEC             ;
8560    B9 00 ED    LDA $ED00,Y     ;
8563    E9 01       SBC #$01        ;
8565    90 03       BCC $856A       ;
8567    99 00 ED    STA $ED00,Y     ;
856A    D8          CLD             ;
856B    60          RTS             ;

;; increase _ED00[Y] by A, maxed out at $99

856C    85 D9       STA $D9         ;
856E    F8          SED             ;
856F    18          CLC             ;
8570    B9 00 ED    LDA $ED00,Y     ; load _ED00[Y]
8573    F0 06       BEQ $857B       ; if zero, leave as-is
8575    65 D9       ADC $D9         ; otherwise add _D9
8577    90 02       BCC $857B       ;
8579    A9 99       LDA #$99        ; max out at 0x99
857B    99 00 ED    STA $ED00,Y     ;
857E    D8          CLD             ;
857F    60          RTS             ;

;; subtract _ED00[Y] by A ???
; going to make a change to $ED00,Y and possibly "lose an eighth"

8580    85 DA       STA $DA         ;
8582    84 D9       STY $D9         ;
8584    B9 00 ED    LDA $ED00,Y     ; load _ED00[Y]
8587    F0 0F       BEQ $8598       ; if zero, print 'THOU HAST LOST AN EIGHTH!'

8589    F8          SED             ; BCD mode
858A    38          SEC             ; set carry
858B    E5 DA       SBC $DA         ; A = A - _DA - 1
858D    F0 02       BEQ $8591       ; if == 0, store 0x01
858F    B0 02       BCS $8593       ; if carry set, store as is
8591    A9 01       LDA #$01        ; otherwise, store 0x01
8593    99 00 ED    STA $ED00,Y     ;
8596    D8          CLD             ;
8597    60          RTS             ; return

8598    20 21 08    JSR $0821       ; print

859B    8D D4 C8 CF D5 A0 C8 C1 D3 D4 A0 CC CF D3 D4 8D   THOU HAST LOST^M
85AB    C1 CE A0 C5 C9 C7 C8 D4 C8 A1 8D 00               AN EIGHTH!^M^@

85B7    A4 D9       LDY $D9         ;
85B9    A9 99       LDA #$99        ;
85BB    4C 89 85    JMP $8589       ; go back to where we were

;;

85BE    85 DA       STA $DA         ;
85C0    20 2D 08    JSR $082D       ;
85C3    A0 19       LDY #$19        ;
85C5    F8          SED             ;
85C6    38          SEC             ;
85C7    B1 FE       LDA ($FE),Y     ;
85C9    E5 DA       SBC $DA         ;
85CB    91 FE       STA ($FE),Y     ;
85CD    88          DEY             ;
85CE    B1 FE       LDA ($FE),Y     ;
85D0    E9 00       SBC #$00        ;
85D2    91 FE       STA ($FE),Y     ;
85D4    D8          CLD             ;
85D5    90 03       BCC ---         ;
85D7    A9 01       LDA #$01        ;
85D9    60          RTS             ;

85DA    A9 00       LDA #$00        ;
85DC    91 FE       STA ($FE),Y     ;
85DE    C8          INY             ;
85DF    91 FE       STA ($FE),Y     ;
85E1    20 2D 08    JSR $082D       ;
85E4    A0 12       LDY #$12        ;
85E6    A9 C4       LDA #$C4        ;
85E8    91 FE       STA ($FE),Y     ;
85EA    A5 0B       LDA $0B         ;
85EC    10 07       BPL ---         ;
85EE    A9 38       LDA #$38        ;
85F0    A6 D4       LDX $D4         ;
85F2    9D 9F EF    STA $EF9F,X     ;
85F5    A9 00       LDA #$00        ;
85F7    60          RTS             ;

85F8    85 DA       STA $DA         ;
85FA    20 2D 08    JSR $082D       ;
85FD    A0 19       LDY #$19        ;
85FF    F8          SED             ;
8600    18          CLC             ;
8601    B1 FE       LDA ($FE),Y     ;
8603    65 DA       ADC $DA         ;
8605    91 FE       STA ($FE),Y     ;
8607    88          DEY             ;
8608    B1 FE       LDA ($FE),Y     ;
860A    69 00       ADC #$00        ;
860C    91 FE       STA ($FE),Y     ;
860E    D8          CLD             ;
860F    A0 18       LDY #$18        ;
8611    B1 FE       LDA ($FE),Y     ;
8613    A0 1A       LDY #$1A        ;
8615    D1 FE       CMP ($FE),Y     ;
8617    90 1A       BCC ---         ;
8619    A0 19       LDY #$19        ;
861B    B1 FE       LDA ($FE),Y     ;
861D    A0 1B       LDY #$1B        ;
861F    D1 FE       CMP ($FE),Y     ;
8621    90 10       BCC ---         ;
8623    A0 1A       LDY #$1A        ;
8625    B1 FE       LDA ($FE),Y     ;
8627    A0 18       LDY #$18        ;
8629    91 FE       STA ($FE),Y     ;
862B    A0 1B       LDY #$1B        ;
862D    B1 FE       LDA ($FE),Y     ;
862F    A0 19       LDY #$19        ;
8631    91 FE       STA ($FE),Y     ;
8633    60          RTS             ;

8634    85 DA       STA $DA         ;
8636    A0 14       LDY #$14        ;
8638    F8          SED             ;
8639    18          CLC             ;
863A    B9 00 ED    LDA $ED00,Y     ;
863D    65 DA       ADC $DA         ;
863F    99 00 ED    STA $ED00,Y     ;
8642    88          DEY             ;
8643    B9 00 ED    LDA $ED00,Y     ;
8646    69 00       ADC #$00        ;
8648    99 00 ED    STA $ED00,Y     ;
864B    D8          CLD             ;
864C    90 09       BCC ---         ;
864E    A9 99       LDA #$99        ;
8650    99 00 ED    STA $ED00,Y     ;
8653    C8          INY             ;
8654    99 00 ED    STA $ED00,Y     ;
8657    60          RTS             ;

8658    85 D8       STA $D8         ;
865A    20 2D 08    JSR $082D       ;
865D    A0 1D       LDY #$1D        ;
865F    F8          SED             ;
8660    18          CLC             ;
8661    B1 FE       LDA ($FE),Y     ;
8663    65 D8       ADC $D8         ;
8665    91 FE       STA ($FE),Y     ;
8667    88          DEY             ;
8668    B1 FE       LDA ($FE),Y     ;
866A    69 00       ADC #$00        ;
866C    91 FE       STA ($FE),Y     ;
866E    D8          CLD             ;
866F    90 07       BCC ---         ;
8671    A9 99       LDA #$99        ;
8673    91 FE       STA ($FE),Y     ;
8675    C8          INY             ;
8676    91 FE       STA ($FE),Y     ;
8678    60          RTS             ;

8679    20 F9 86    JSR $86F9       ;
867C    A9 1E       LDA #$1E        ;
867E    20 FD 7E    JSR $7EFD       ;
8681    20 28 85    JSR $8528       ;
8684    20 BE 85    JSR $85BE       ;
8687    60          RTS             ;

8688    20 F5 84    JSR $84F5       ;
868B    A9 06       LDA #$06        ;
868D    20 54 08    JSR $0854       ;
8690    20 0B 87    JSR $870B       ;
8693    A5 0B       LDA $0B         ;
8695    30 33       BMI ---         ;
8697    A5 0E       LDA $0E         ;
8699    C9 14       CMP #$14        ;
869B    B0 2D       BCS ---         ;
869D    F8          SED             ;
869E    38          SEC             ;
869F    A5 1B       LDA $1B         ;
86A1    E9 0A       SBC #$0A        ;
86A3    D8          CLD             ;
86A4    B0 22       BCS ---         ;
86A6    A9 00       LDA #$00        ;
86A8    85 1B       STA $1B         ;
86AA    20 F5 84    JSR $84F5       ;
86AD    20 45 08    JSR $0845       ;
86B0    20 21 08    JSR $0821       ;

86B3    8D D4 C8 D9 A0 D3 C8 C9 D0 A0 D3 C9 CE CB D3 A1 8D 00                   ^MTHY SHIP SINKS!^M^@

86C5    4C C9 84    JMP $84C9       ;

86C8    85 1B       STA $1B         ;
86CA    A5 0F       LDA $0F         ;
86CC    85 D4       STA $D4         ;
86CE    20 4E 08    JSR $084E       ;
86D1    30 1B       BMI ---         ;
86D3    20 BB 7E    JSR $7EBB       ;
86D6    D0 16       BNE ---         ;
86D8    A5 0B       LDA $0B         ;
86DA    10 07       BPL ---         ;
86DC    A6 D4       LDX $D4         ;
86DE    BD 9F EF    LDA $EF9F,X     ;
86E1    F0 0B       BEQ ---         ;
86E3    A9 19       LDA #$19        ;
86E5    20 FD 7E    JSR $7EFD       ;
86E8    20 28 85    JSR $8528       ;
86EB    20 BE 85    JSR $85BE       ;
86EE    C6 D4       DEC $D4         ;
86F0    D0 DC       BNE ---         ;
86F2    20 F5 84    JSR $84F5       ;
86F5    20 45 08    JSR $0845       ;
86F8    60          RTS             ;

86F9    20 01 85    JSR $8501       ;
86FC    A9 07       LDA #$07        ;
86FE    20 54 08    JSR $0854       ;
8701    20 01 85    JSR $8501       ;
8704    60          RTS             ;

;; print newline

8705    A9 8D       LDA #$8D        ;
8707    20 24 08    JSR $0824       ;
870A    60          RTS             ;

;;

870B    20 53 87    JSR $8753       ;
870E    20 24 87    JSR $8724       ;
8711    20 53 87    JSR $8753       ;
8714    20 24 87    JSR $8724       ;
8717    20 53 87    JSR $8753       ;
871A    20 24 87    JSR $8724       ;
871D    20 53 87    JSR $8753       ;
8720    20 24 87    JSR $8724       ;
8723    60          RTS             ;

8724    A2 AE       LDX #$AE        ;
8726    BD 09 E0    LDA $E009,X     ;
8729    85 FE       STA $FE         ;
872B    BD C9 E0    LDA $E0C9,X     ;
872E    85 FF       STA $FF         ;
8730    BD 07 E0    LDA $E007,X     ;
8733    85 FC       STA $FC         ;
8735    BD C7 E0    LDA $E0C7,X     ;
8738    85 FD       STA $FD         ;
873A    A0 16       LDY #$16        ;
873C    B1 FC       LDA ($FC),Y     ;
873E    91 FE       STA ($FE),Y     ;
8740    88          DEY             ;
8741    D0 F9       BNE ---         ;
8743    24 CD       BIT $CD         ;
8745    10 08       BPL ---         ;
8747    20 4E 08    JSR $084E       ;
874A    30 03       BMI ---         ;
874C    2C 30 C0    BIT $C030       ;
874F    CA          DEX             ;
8750    D0 D4       BNE ---         ;
8752    60          RTS             ;

8753    A2 00       LDX #$00        ;
8755    BD 08 E0    LDA $E008,X     ;
8758    85 FE       STA $FE         ;
875A    BD C8 E0    LDA $E0C8,X     ;
875D    85 FF       STA $FF         ;
875F    BD 0A E0    LDA $E00A,X     ;
8762    85 FC       STA $FC         ;
8764    BD CA E0    LDA $E0CA,X     ;
8767    85 FD       STA $FD         ;
8769    A0 16       LDY #$16        ;
876B    B1 FC       LDA ($FC),Y     ;
876D    91 FE       STA ($FE),Y     ;
876F    88          DEY             ;
8770    D0 F9       BNE ---         ;
8772    24 CD       BIT $CD         ;
8774    10 08       BPL ---         ;
8776    20 4E 08    JSR $084E       ;
8779    30 03       BMI ---         ;
877B    2C 30 C0    BIT $C030       ;
877E    E8          INX             ;
877F    E0 AE       CPX #$AE        ;
8781    90 D2       BCC ---         ;
8783    60          RTS             ;

8784    C9 43       CMP #$43        ;
8786    F0 01       BEQ ---         ;
8788    60          RTS             ;

8789    A5 12       LDA $12         ;
878B    C9 04       CMP #$04        ;
878D    D0 1A       BNE ---         ;
878F    A5 13       LDA $13         ;
8791    C9 04       CMP #$04        ;
8793    D0 14       BNE ---         ;
8795    20 4B 08    JSR $084B       ;
8798    20 78 08    JSR $0878       ;
879B    A9 09       LDA #$09        ;
879D    A2 A0       LDX #$A0        ;
879F    20 54 08    JSR $0854       ;
87A2    A9 1F       LDA #$1F        ;
87A4    85 0A       STA $0A         ;
87A6    4C EB 51    JMP $51EB       ;

87A9    20 4B 08    JSR $084B       ;
87AC    A6 13       LDX $13         ;
87AE    BD 0F 4C    LDA $4C0F,X     ;
87B1    85 00       STA $00         ;
87B3    BD 17 4C    LDA $4C17,X     ;
87B6    85 01       STA $01         ;
87B8    20 78 08    JSR $0878       ;
87BB    A9 09       LDA #$09        ;
87BD    A2 A0       LDX #$A0        ;
87BF    20 54 08    JSR $0854       ;
87C2    20 78 08    JSR $0878       ;
87C5    20 03 08    JSR $0803       ;
87C8    20 4B 08    JSR $084B       ;
87CB    20 78 08    JSR $0878       ;
87CE    A9 09       LDA #$09        ;
87D0    A2 A0       LDX #$A0        ;
87D2    20 54 08    JSR $0854       ;
87D5    20 78 08    JSR $0878       ;
87D8    4C 84 63    JMP $6384       ; return to main loop

;;
; if A in [$00, $8A, $90, $94, $98, $B4, $CC]
;     return $00
;   else
;     return $FF

87DB    C9 00       CMP #$00        ;
87DD    10 1B       BPL $87FA       ;
87DF    C9 8A       CMP #$8A        ;
87E1    F0 17       BEQ $87FA       ;
87E3    C9 90       CMP #$90        ;
87E5    F0 13       BEQ $87FA       ;
87E7    C9 94       CMP #$94        ;
87E9    F0 0F       BEQ $87FA       ;
87EB    C9 98       CMP #$98        ;
87ED    F0 0B       BEQ $87FA       ;
87EF    C9 B4       CMP #$B4        ;
87F1    F0 07       BEQ $87FA       ;
87F3    C9 CC       CMP #$CC        ;
87F5    F0 03       BEQ $87FA       ;
87F7    A9 FF       LDA #$FF        ;
87F9    60          RTS             ;

87FA    A9 00       LDA #$00        ;
87FC    60          RTS             ;

87FD    FF 00 00

FF
```