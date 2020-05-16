---
grand_parent: DISK 2
parent: LORD
nav_order: 1
---

# LORD full listing

```
ORG $6000 (actually loaded into $8800)
LEN $11D0


8800    68          PLA             ;
8801    68          PLA             ;
8802    68          PLA             ;
8803    68          PLA             ;
8804    4C 8F 89    JMP $898F       ; prompt for request after first time

;; TALK TO LORD BRITISH

8807    A5 15       LDA $15         ; if _15 != 0x00
8809    D0 05       BNE $8810       ;   been here before, go to $8810
880B    E6 15       INC $15         ; _15++ (ot 0x01)
880D    4C D1 96    JMP $96D1       ; first time Lord British greeting

8810    A9 01       LDA #$01        ; .
8812    85 D4       STA $D4         ; _D4 = 0x01
8814    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
8817    A0 45       LDY #$45        ; Y = 0x45
8819    B1 FE       LDA ($FE),Y     ; A = _ECXX[Y]
881B    C9 C4       CMP #$C4        ; if A != 'D' (dead)
881D    D0 3C       BNE $885B       ;   lord britsh says welcome...
881F    A9 C7       LDA #$C7        ; else: (if dead, resurrect)
8821    91 FE       STA ($FE),Y     ;   _ECXX[Y] = 'G'
8823    20 30 08    JSR $0830       ; @@@ print character name?
8826    20 21 08    JSR $0821       ;   print

8829    D4 C8 CF D5 A0 D3 C8 C1 CC D4 8D                                        THOU SHALT^M
8834    CC C9 D6 C5 A0 C1 C7 C1 C9 CE A1 8D 00                                  LIVE AGAIN!^M^@

8841    A2 14       LDX #$14        ; play sound 0x0A for duration 0x14 ?
8843    A9 0A       LDA #$0A        ; .
8845    20 54 08    JSR $0854       ; .
8848    20 78 08    JSR $0878       ; invert the hires screen ?
884B    A9 09       LDA #$09        ; play sound 0x09 for duration 0xC0 ?
884D    A2 C0       LDX #$C0        ; .
884F    20 54 08    JSR $0854       ; .
8852    20 78 08    JSR $0878       ; invert the hires screen ?
8855    20 96 99    JSR $9996       ; for all living players, set status to 'G' and set health to max
8858    20 45 08    JSR $0845       ; display player stats ?

;;

885B    20 21 08    JSR $0821       ;

885E    CC CF D2 C4 A0 C2 D2 C9 D4 C9 D3 C8 8D                                  LORD BRITISH^M
886B    D3 C1 D9 D3 BA A0 D7 C5 CC C3 CF CD C5 8D 00                            SAYS: WELCOME^M^@

887A    A9 01       LDA #$01        ; character_number = 1
887C    85 D4       STA $D4         ;
887E    20 30 08    JSR $0830       ; print character name?
8881    A5 0F       LDA $0F         ;
8883    C9 02       CMP #$02        ; if num_characters < 2
8885    90 04       BCC $888B       ;   go to $888B
8887    F0 15       BEQ $889E       ; if num_characters == 2, got to $889E
8889    B0 34       BCS $88BF       ; if num_characters >= 2, go to $88BF

;; < 2 characters

888B    20 21 08    JSR $0821       ; print

888E    8D CD D9 A0 C6 D2 C9 C5 CE C4 A1 8D 00                                  ^MMY FRIEND!^M^@

889B    4C E0 88    JMP $88E0       ;

;; 2 characters

889E    20 21 08    JSR $0821       ;

88A1    8D C1 CE C4 A0 D4 C8 C5 C5 A0 C1 CC D3 CF 8D 00                         ^MAND THEE ALSO^M^@

88B1    E6 D4       INC $D4         ; character_number += 1
88B3    20 30 08    JSR $0830       ; print character name
88B6    20 21 08    JSR $0821       ; print

88B9    A1 8D 00                                                                !^M^@

88BC    4C E0 88    JMP $88E0       ;

;; > 2 characters

88BF    20 21 08    JSR $0821       ; print

88C2    8D C1 CE C4 A0 D4 C8 D9 A0 D7 CF D2 D4 C8 D9 8D                         ^MAND THY WORTHY^M
88D2    C1 C4 D6 C5 CE D4 D5 D2 C5 D2 D3 A1 8D 00                               ADVENTURERS!^M^@

;; from here to $8969 is looping over characters in party

88E0    A5 0F       LDA $0F         ;
88E2    85 D4       STA $D4         ; character_number = num_characters

88E4    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
88E7    A0 1C       LDY #$1C        ;
88E9    B1 FE       LDA ($FE),Y     ; load #$1C for character
88EB    20 BE 99    JSR $99BE       ; ???

;; basically work out the log base two of A and put it in X

88EE    A2 01       LDX #$01        ; X = 1

88F0    C9 00       CMP #$00        ; if A = 0
88F2    F0 05       BEQ $88F9       ;   break
88F4    4A          LSR A           ; A = A / 2
88F5    E8          INX             ; X++
88F6    4C F0 88    JMP $88F0       ; loop back

88F9    8A          TXA             ; A = X

88FA    A0 1A       LDY #$1A        ;
88FC    D1 FE       CMP ($FE),Y     ; most-significant digit of max character health
88FE    F0 65       BEQ $8965       ; if zero, skip to next character
8900    91 FE       STA ($FE),Y     ;
8902    85 EA       STA $EA         ; character buffer pointer
8904    A0 18       LDY #$18        ;
8906    91 FE       STA ($FE),Y     ; most-significant digit of character health
8908    A9 00       LDA #$00        ;
890A    C8          INY             ; Y++
890B    91 FE       STA ($FE),Y     ; set lower two digits of character health to 00
890D    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
8910    A0 12       LDY #$12        ; set character status
8912    A9 C7       LDA #$C7        ; to 'G'
8914    91 FE       STA ($FE),Y     ; .

; seems to add a random number between 0 and 7 to character property $15 and $14, capped at max of $50  @@@

8916    A0 15       LDY #$15        ; not sure what 15 is

8918    20 4E 08    JSR $084E       ; random number?
891B    29 07       AND #$07        ; mask to get low three bits
891D    F8          SED             ;
891E    38          SEC             ;
891F    71 FE       ADC ($FE),Y     ; add to whatever is at byte Y of character description
8921    D8          CLD             ;
8922    C9 51       CMP #$51        ; if < 0x51
8924    90 02       BCC $8928       ;     skip

8926    A9 50       LDA #$50        ; otherwise make 0x50 (i.e. max out at 0x50)

8928    91 FE       STA ($FE),Y     ; and store back
892A    88          DEY             ; Y-- (15 to 14 to 13)
892B    C0 13       CPY #$13        ; if Y >= 13
892D    B0 E9       BCS $8918       ; loop back

892F    20 52 8A    JSR $8A52       ; print new line
8932    20 30 08    JSR $0830       ; print character name
8935    20 21 08    JSR $0821       ; print

8938    8D D4 C8 CF D5 A0 C1 D2 D4 A0 CE CF D7 8D                               ^MTHOU ART NOW^M
8946    CC C5 D6 C5 CC A0 00                                                    LEVEL ^@

894D    A5 EA       LDA $EA         ; ???
894F    20 66 08    JSR $0866       ; print single digit
8952    20 52 8A    JSR $8A52       ; print new line
8955    20 78 08    JSR $0878       ; invert the hires screen?
8958    A9 09       LDA #$09        ; play sound 0x09 for 0xC0
895A    A2 C0       LDX #$C0        ; .
895C    20 54 08    JSR $0854       ; .
895F    20 78 08    JSR $0878       ; invert the hires screen?
8962    20 45 08    JSR $0845       ; DISPLAY STATS

8965    C6 D4       DEC $D4         ; decrement character number
8967    F0 03       BEQ $896C       ; if zero, skip

8969    4C E4 88    JMP $88E4       ; otherwise loop back to next character

;; prompt for request

896C    20 21 08    JSR $0821       ; print

896F    8D D7 C8 C1 D4 A0 D7 CF D5 CC C4 A0 D4 C8 CF D5 8D                      ^MWHAT WOULD THOU^M
8980    C1 D3 CB A0 CF C6 A0 CD C5 BF 8D 00                                     ASK OF ME?^M^@

898C    4C A4 89    JMP $89A4

;; prompt for request after first time

898F    A9 01       LDA #$01        ; ???
8991    20 20 03    JSR $0320       ; SEL related?
8994    20 21 08    JSR $0821       ; print

8997    8D D7 C8 C1 D4 A0 C5 CC D3 C5 BF 8D 00                                  ^MWHAT ELSE?^M^@

;; get player request and act on it

89A4    20 1E 99    JSR $991E       ; get player input
89A7    20 F8 98    JSR $98F8       ; convert what was typed in to the request number (e.g. JOB == 0x04)
89AA    10 28       BPL $89D4       ; if valid, go to $89D4, otherwise
89AC    20 35 8A    JSR $8A35       ;    print "HE SAYS:"
89AF    20 21 08    JSR $0821       ;    print

89B2    C9 A0 C3 C1 CE CE CF D4 A0 C8 C5 CC D0 8D                               I CANNOT HELP^M
89C0    D4 C8 C5 C5 A0 D7 C9 D4 C8 A0 D4 C8 C1 D4 AE 8D 00                      THEE WITH THAT.^M^@

89D1    4C 8F 89    JMP $898F       ; go back to $898F (prompt for request after first time)

;; handle player request
; A is request number (e.g. JOB == 0x04)

89D4    0A          ASL A           ; Y = A * 2
89D5    A8          TAY             ; .
89D6    B9 E3 89    LDA $89E3,Y     ; load low byte of handler address
89D9    85 FE       STA $FE         ; and store in $FE
89DB    B9 E4 89    LDA $89E4,Y     ; load hight byte of handler address
89DE    85 FF       STA $FF         ; and store in $FF
89E0    6C FE 00    JMP (00FE)      ; and jump to request handler

;; vector

89E3    80 8A                       ; 0x00 => $8A80
89E5    80 8A                       ; 0x01 => $8A80 BYE
89E7    B8 8A                       ; 0x02 => $8AB8 NAME
89E9    F7 8A                       ; 0x03 => $8AF7 LOOK
89EB    28 8B                       ; 0x04 => $8B28 JOB
89ED    6B 8B                       ; 0x05 => $8B6B HEAL
89EF    F6 8B                       ; 0x06 => $8BF6 TRUT
89F1    5D 8C                       ; 0x07 => $8C5D LOVE
89F3    CC 8C                       ; 0x08 => $8CCC COUR
89F5    1D 8D                       ; 0x09 => $8D1D HONE
89F7    7A 8D                       ; 0x0A => $8D7A COMP
89F9    D3 8D                       ; 0x0B => $8DD3 VALO
89FB    1C 8E                       ; 0x0C => $8E1C JUST
89FD    61 8E                       ; 0x0D => $8E61 SACR
89FF    B8 8E                       ; 0x0E => $8EB8 HONO
8A01    1A 8F                       ; 0x0F => $8F1A SPIR
8A03    74 8F                       ; 0x10 => $8F74 HUMI
8A05    54 90                       ; 0x11 => $9054 PRID
8A07    46 91                       ; 0x12 => $9146 AVAT
8A09    01 92                       ; 0x13 => $9201 QUES
8A0B    F9 92                       ; 0x14 => $92F9 BRIT
8A0D    43 94                       ; 0x15 => $9443 ANKH
8A0F    C4 94                       ; 0x16 => $94C4 HELP
8A11    DF 94                       ; 0x17 => $94DF ABYS
8A13    FA 95                       ; 0x18 => $95FA MOND
8A15    15 96                       ; 0x19 => $9615 MINA
8A17    2E 96                       ; 0x1A => $962E EXOD
8A19    48 96                       ; 0x1B => $9648 VIRT

;; print "LORD BRITISH SAYS: "

8A1B    20 21 08    JSR $0821       ; print

8A1E    8D CC CF D2 C4 A0 C2 D2 C9 D4 C9 D3 C8 8D                               ^MLORD BRITISH^M
8A2C    D3 C1 D9 D3 BA A0 8D 00                                                 SAYS: ^M^@

8A34    60          RTS

;; print "HE SAYS:"

8A35    20 21 08    JSR $0821       ; print

8A38    8D C8 C5 A0 D3 C1 D9 D3 BA 8D 00                                        ^MHE SAYS:^M^@

8A43    60          RTS

;; print "HE ASKS:"

8A44    20 21 08    JSR $0821       ; print

8A47    8D C8 C5 A0 C1 D3 CB D3 BA 8D 00                                        ^MHE ASKS:^M^@

;; print newline

8A52    A9 8D       LDA #$8D        ; '^M'
8A54    20 24 08    JSR $0824       ; print character
8A57    60          RTS             ;

;; get player input and return 0x00 if 'Y' and 0x01 if 'N'

8A58    20 1E 99    JSR $991E       ; get player input
8A5B    AD 00 03    LDA $0300       ;
8A5E    C9 D9       CMP #$D9        ; if 'Y'
8A60    F0 18       BEQ $8A7A       ;    go to $8A7A

8A62    C9 CE       CMP #$CE        ; if 'N'
8A64    F0 17       BEQ $8A7D       ;    go to $8A7D

8A66    20 21 08    JSR $0821       ; print

8A69    8D D9 C5 D3 A0 CF D2 A0 CE CF BA 8D 00                                  ^MYES OR NO:^M^@

8A76    60          RTS             ;

8A77    4C 58 8A    JMP $8A58       ;

8A7A    A9 00       LDA #$00        ; return 0x00 (YES)
8A7C    60          RTS             ;

8A7D    A9 01       LDA #$01        ; return 0x01 (NO)
8A7F    60          RTS             ;

;; BYE

8A80    20 1B 8A    JSR $8A1B       ; print "LORD BRITISH SAYS: "
8A83    20 21 08    JSR $0821       ; print

8A86    C6 C1 D2 C5 A0 D4 C8 C5 C5 8D                                           FARE THEE^M
8A90    D7 C5 CC CC A0 CD D9 A0 C6 D2 C9 C5 CE C4 00                            WELL MY FRIEND^@

8A9F    A5 0F       LDA $0F         ; party size
8AA1    C9 01       CMP #$01        ; if != 1
8AA3    D0 09       BNE $8AAE       ;     go to $8AAE

; just print '!' as only one party member

8AA5    20 21 08    JSR $0821       ; print

8AA8    A1 8D 00                                                                !^M^@

8AAB    4C CE 99    JMP $99CE       ; global return

; print 'S!' as more than one party member

8AAE    20 21 08    JSR $0821       ; print

8AB1    D3 A1 8D 00                                                             S!^M^@

8AB5    4C CE 99    JMP $99CE       ; global return

;; NAME

8AB8    20 35 8A    JSR $8A35       ; print "HE SAYS:"
8ABB    20 21 08    JSR $0821       ; print

8ABE    CD D9 A0 CE C1 CD C5 A0 C9 D3 8D                                        MY NAME IS^M
8AC9    CC CF D2 C4 A0 C2 D2 C9 D4 C9 D3 C8 AC 8D                               LORD BRITISH,^M
8AD7    D3 CF D6 C5 D2 C5 C9 C7 CE A0 CF C6 8D                                  SOVEREIGN OF^M
8AE4    C1 CC CC A0 C2 D2 C9 D4 C1 CE CE C9 C1 A1 8D 00                         ALL BRITANNIA!^M^@

8AF4    4C 8F 89    JMP $898F       ; prompt for request after first time

;; LOOK

8AF7    20 21 08

8AFA    D4 C8 CF D5 A0 D3 C5 C5 A0 D4 C8 C5 8D                                  THOU SEE THE^M
8B07    CB C9 CE C7 A0 D7 C9 D4 C8 A0 D4 C8 C5 8D                               KING WITH THE^M
8B15    D2 CF D9 C1 CC A0 D3 C3 C5 D0 D4 D2 C5 AE 8D 00                         ROYAL SCEPTRE^M^@

8B25    4C 8F 89    JMP $898F       ; prompt for request after first time

;; JOB

8B28    20 35 8A 20 21 08

8B2E    C9 A0 D2 D5 CC C5 A0 C1 CC CC 8D                                        I RULE ALL^M
        C2 D2 C9 D4 C1 CE CE C9 C1 AC A0 C1 CE C4 8D                            BRITANNIA, AND^M
        D3 C8 C1 CC CC A0 C4 CF A0 CD D9 A0 C2 C5 D3 D4 8D                      SHALL DO MY BEST^M
        D4 CF A0 C8 C5 CC D0 A0 D4 C8 C5 C5 A1 8D 00                            TO HELP THEE!^M^@

4C 8F 89    JMP $898F       ; prompt for request after first time

;; HEAL

8B6B    20 35 8A    JSR $8A35       ; print "HE SAYS:"
8B6E    20 21 08    JSR $0821       ; print

8B71    C9 A0 C1 CD A0 D7 C5 CC CC AC 8D                                        I AM WELL,^M
8B7C    D4 C8 C1 CE CB A0 D9 C5 AE 8D 00                                        THANK YE^M^@

8B87    20 44 8A    JSR $8A44       ; print "HE ASKS:"
8B8A    20 21 08    JSR $0821       ; print

8B8D    C1 D2 D4 A0 D4 C8 CF D5 A0 D7 C5 CC CC BF 8D 00                         ART THOU WELL?^M^@

8B9D    20 58 8A    JSR $8A58       ; get player Y/N input
8BA0    D0 18       BNE $8BBA       ; if 'N' go to $8BBA, otherwise
8BA2    20 35 8A    JSR $8A35       ;     print "HE SAYS:"
8BA5    20 21 08    JSR $0821       ;     print

8BA8    D4 C8 C1 D4 A0 C9 D3 A0 C7 CF CF C4 AE 8D 00                            THAT IS GOOD.^M^@

8BB7    4C 8F 89    JMP $898F       ; prompt for request after first time

;; if the player says 'N' to 'ART THOU WELL?'

8BBA    20 35 8A    JSR $8A35       ; print "HE SAYS:"
8BBD    20 21 08    JSR $0821       ; print

8BC0    CC C5 D4 A0 CD C5 A0 C8 C5 C1 CC 8D                                     LET ME HEAL^M
8BCC    D4 C8 D9 A0 D7 CF D5 CE C4 D3 A1 8D 00                                  THY WOUNDS!^M^@

8BD9    A2 14       LDX #$14        ; play sound 0x0A for duration 0x14
8BDB    A9 0A       LDA #$0A        ; .
8BDD    20 54 08    JSR $0854       ; .
8BE0    20 78 08    JSR $0878       ; invert the hires screen
8BE3    A9 09       LDA #$09        ; play sound 0x09 for duration 0xC0
8BE5    A2 C0       LDX #$C0        ; .
8BE7    20 54 08    JSR $0854       ; .
8BEA    20 78 08    JSR $0878       ; invert the hires screen
8BED    20 96 99    JSR $9996       ; for all living players, set status to 'G' and set health to max
8BF0    20 45 08    JSR $0845       ; DISPLAY STATS
8BF3    4C 8F 89    JMP $898F       ; prompt for request after first time

;; TRUT

8BF6    20 35 8A    JSR $8A35       ; print "HE SAYS:"
8BF9    20 21 08    JSR $0821       ; print

8BFC    CD C1 CE D9 A0 D4 D2 D5 D4 C8 D3 A0 C3 C1 CE 8D MANY TRUTHS CAN.
8C0C    C2 C5 A0 CC C5 C1 D2 CE C5 C4 A0 C1 D4 8D D4 C8 BE LEARNED AT.TH
8C1C    C5 A0 CC D9 C3 C1 C5 D5 CD AE A0 C9 D4 8D CC C9 E LYCAEUM. IT.LI
8C2C    C5 D3 A0 CF CE A0 D4 C8 C5 8D CE CF D2 D4 C8 D7 ES ON THE.NORTHW
8C3C    C5 D3 D4 C5 D2 CE 8D D3 C8 CF D2 C5 A0 CF C6 A0 ESTERN.SHORE OF
8C4C    D6 C5 D2 C9 D4 D9 8D C9 D3 CC C5 A1 8D 00  VERITY.ISLE!..

8C5A    4C 8F 89    JMP $898F       ; prompt for request after first time

;; LOVE

8C5D    20 35 8A    JSR $8A35       ; print "HE SAYS:"
8C60    20 21 08    JSR $0821       ; print

8C63    CC CF CF CB A0 C6 CF D2 A0    LOOK FOR
8C6C    D4 C8 C5 8D CD C5 C1 CE C9 CE C7 A0 CF C6 A0 CC THE.MEANING OF L
8C7C    CF D6 C5 8D C1 D4 A0 C5 CD D0 C1 D4 C8 A0 C1 C2 OVE.AT EMPATH AB
8C8C    C2 C5 D9 AE 8D D4 C8 C5 A0 C1 C2 C2 C5 D9 A0 D3 BEY..THE ABBEY S
8C9C    C9 D4 D3 8D CF CE A0 D4 C8 C5 A0 D7 C5 D3 D4 C5 ITS.ON THE WESTE
8CAC    D2 CE 8D C5 C4 C7 C5 A0 CF C6 A0 D4 C8 C5 A0 C4 RN.EDGE OF THE D
8CBC    C5 C5 D0 8D C6 CF D2 C5 D3 D4 A1 8D 00 EEP.FOREST!..

8CC9    4C 8F 89    JMP $898F       ; prompt for request after first time

;; COUR

8CCC    20 35 8A 20 21 08
8CD2    D3 C5 D2 D0 C5 CE D4 A0 C3 C1 SERPENT CA
8CDC    D3 D4 CC C5 8D CF CE A0 D4 C8 C5 A0 C9 D3 CC C5 STLE.ON THE ISLE
8CEC    A0 CF C6 8D C4 C5 C5 C4 D3 A0 C9 D3 A0 D7 C8 C5  OF.DEEDS IS WHE
8CFC    D2 C5 8D C3 CF D5 D2 C1 C7 C5 A0 D3 C8 CF D5 CC RE.COURAGE SHOUL
8D0C    C4 8D C2 C5 A0 D3 CF D5 C7 C8 D4 A1 8D 00  D.BE SOUGHT!..

8D1A    4C 8F 89    JMP $898F       ; prompt for request after first time

;; HONE

8D1D    20 35 8A 20 21 08

8D23    D4 C8 C5 A0 C6 C1 C9 D2 A0    THE FAIR
8D2C    D4 CF D7 CE C5 8D CF C6 A0 CD CF CF CE C7 CC CF TOWNE.OF MOONGLO
8D3C    D7 A0 CF CE 8D C4 C1 C7 C7 C5 D2 A0 C9 D3 CC C5 W ON.DAGGER ISLE
8D4C    AC A0 C9 D3 8D D7 C8 C5 D2 C5 A0 D4 C8 C5 A0 D6 , IS.WHERE THE V
8D5C    C9 D2 D4 D5 C5 8D CF C6 A0 C8 CF CE C5 D3 D4 D9 IRTUE.OF HONESTY
8D6C    8D D4 C8 D2 C9 D6 C5 D3 A1 8D 00        .THRIVES!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; COMP

8D7A    20 35 8A 20 21 08

8D80    D4 C8 C5 A0 C2 C1 D2 C4 D3 A0 C9 CE THE BARDS IN
8D8C    A0 D4 C8 C5 8D D4 CF D7 CE C5 A0 CF C6 A0 C2 D2  THE.TOWNE OF BR
8D9C    C9 D4 C1 C9 CE 8D C1 D2 C5 A0 D7 C5 CC CC A0 D6 ITAIN.ARE WELL V
8DAC    C5 D2 D3 C5 C4 8D C9 CE A0 D4 C8 C5 A0 D6 C9 D2 ERSED.IN THE VIR
8DBC    D4 D5 C5 A0 CF C6 8D C3 CF CD D0 C1 D3 D3 C9 CF TUE OF.COMPASSIO
8DCC    CE A1 8D 00                                     N!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; VALO

8DD3    20 35 8A 20 21 08

8DD9    CD C1 CE MAN
8DDC    D9 A0 D6 C1 CC C9 C1 CE D4 8D C6 C9 C7 C8 D4 C5 Y VALIANT.FIGHTE
8DEC    D2 D3 A0 C3 CF CD C5 8D C6 D2 CF CD A0 CA C8 C5 RS COME.FROM JHE
8DFC    CC CF CD AC 8D C9 CE A0 D4 C8 C5 A0 D6 C1 CC C1 LOM,.IN THE VALA
8E0C    D2 C9 C1 CE 8D C9 D3 CC C5 D3 A1 8D 00  RIAN.ISLES!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; JUST

8E1C    20 35 8A 20 21 08 C9 CE A0 D4 C8 C5 A0 C3 C9 D4  5. !.IN THE CIT
8E2C    D9 A0 CF C6 8D D9 C5 D7 AC A0 C9 CE A0 D4 C8 C5 Y OF.YEW, IN THE
8E3C    A0 C4 C5 C5 D0 8D C6 CF D2 C5 D3 D4 AC A0 CA D5  DEEP.FOREST, JU
8E4C    D3 D4 C9 C3 C5 8D C9 D3 A0 D3 C5 D2 D6 C5 C4 A1 STICE.IS SERVED!
8E5C    8D 00

4C 8F 89    JMP $898F       ; prompt for request after first time

;; SACR

8E61    20 35 8A 20 21 08 CD C9 CE CF C3  5. !.MINOC
8E6C    AC A0 D4 CF D7 CE C5 A0 CF C6 8D D3 C5 CC C6 AD , TOWNE OF.SELF-
8E7C    D3 C1 C3 D2 C9 C6 C9 C3 C5 AC 8D CC C9 C5 D3 A0 SACRIFICE,.LIES
8E8C    CF CE A0 D4 C8 C5 8D C5 C1 D3 D4 C5 D2 CE A0 D3 ON THE.EASTERN S
8E9C    C8 CF D2 C5 D3 8D CF C6 A0 CC CF D3 D4 A0 C8 CF HORES.OF LOST HO
8EAC    D0 C5 8D C2 C1 D9 A1 8D 00             PE.BAY!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; HONO

8EB8    20 35 8A 20
8EBC    21 08 D4 C8 C5 A0 D0 C1 CC C1 C4 C9 CE D3 A0 D7 !.THE PALADINS W
8ECC    C8 CF 8D D3 D4 D2 C9 D6 C5 A0 C6 CF D2 A0 C8 CF HO.STRIVE FOR HO
8EDC    CE CF D2 8D C1 D2 C5 A0 CF C6 D4 A0 D3 C5 C5 CE NOR.ARE OFT SEEN
8EEC    A0 C9 CE 8D D4 D2 C9 CE D3 C9 C3 AC A0 CE CF D2  IN.TRINSIC, NOR
8EFC    D4 C8 8D CF C6 A0 D4 C8 C5 A0 C3 C1 D0 C5 A0 CF TH.OF THE CAPE O
8F0C    C6 8D C8 C5 D2 CF C5 D3 A1 8D 00       F.HEROES!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; SPIR

8F1A    20 35  5
8F1C    8A 20 21 08 C9 CE A0 D3 CB C1 D2 C1 A0 C2 D2 C1 . !.IN SKARA BRA
8F2C    C5 8D D4 C8 C5 A0 D3 D0 C9 D2 C9 D4 D5 C1 CC 8D E.THE SPIRITUAL.
8F3C    D0 C1 D4 C8 A0 C9 D3 A0 D4 C1 D5 C7 C8 D4 AC 8D PATH IS TAUGHT,.
8F4C    C6 C9 CE C4 A0 C9 D4 A0 CF CE A0 C1 CE 8D C9 D3 FIND IT ON AN.IS
8F5C    CC C5 A0 CE C5 C1 D2 8D D3 D0 C9 D2 C9 D4 D7 CF LE NEAR.SPIRITWO
8F6C    CF C4 A1 8D 00                         OD!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; HUMI

8F74    20 35 8A 20 21 08 C8 D5                          5. !.HU
8F7C    CD C9 CC C9 D4 D9 A0 C9 D3 A0 D4 C8 C5 8D C6 CF MILITY IS THE.FO
8F8C    D5 CE C4 C1 D4 C9 CF CE A0 CF C6 8D D6 C9 D2 D4 UNDATION OF.VIRT
8F9C    D5 C5 A1 A0 D4 C8 C5 8D D2 D5 C9 CE D3 A0 CF C6 UE! THE.RUINS OF
8FAC    A0 D0 D2 CF D5 C4 8D CD C1 C7 C9 CE C3 C9 C1 A0  PROUD.MAGINCIA
8FBC    C1 D2 C5 A0 C1 8D D4 C5 D3 D4 C9 CD CF CE D9 A0 ARE A.TESTIMONY
8FCC    D5 CE D4 CF 8D D4 C8 C5 A0 D6 C9 D2 D4 D5 C5 A0 UNTO.THE VIRTUE
8FDC    CF C6 8D C8 D5 CD C9 CC C9 D4 D9 A1 8D 00 20 00 OF.HUMILITY!.. .
8FEC    08 20 21 08 8D C6 C9 CE C4 A0 D4 C8 C5 A0 D2 D5 . !..FIND THE RU
8FFC    C9 CE D3 8D CF C6 A0 CD C1 C7 C9 CE C3 C9 C1 A0 INS.OF MAGINCIA
900C    C6 C1 D2 8D CF C6 C6 A0 D4 C8 C5 A0 D3 C8 CF D2 FAR.OFF THE SHOR
901C    C5 D3 8D CF C6 A0 C2 D2 C9 D4 C1 CE CE C9 C1 AC ES.OF BRITANNIA,
902C    8D CF CE A0 C1 A0 D3 CD C1 CC CC A0 C9 D3 CC C5 .ON A SMALL ISLE
903C    8D C9 CE A0 D4 C8 C5 A0 D6 C1 D3 D4 8D CF C3 C5 .IN THE VAST.OCE
904C    C1 CE A1 8D 00                         AN!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; PRID

9054    20 35 8A 20 21 08 CF C6                          5. !.OF
905C    A0 D4 C8 C5 A0 C5 C9 C7 C8 D4 8D C3 CF CD C2 C9  THE EIGHT.COMBI
906C    CE C1 D4 C9 CF CE D3 A0 CF C6 8D D4 D2 D5 D4 C8 NATIONS OF.TRUTH
907C    AC A0 CC CF D6 C5 A0 C1 CE C4 8D C3 CF D5 D2 C1 , LOVE AND.COURA
908C    C7 C5 AC A0 D4 C8 C1 D4 8D D7 C8 C9 C3 C8 A0 C3 GE, THAT.WHICH C
909C    CF CE D4 C1 C9 CE D3 8D CE C5 C9 D4 C8 C5 D2 A0 ONTAINS.NEITHER
90AC    D4 D2 D5 D4 C8 AC 8D CC CF D6 C5 A0 CE CF D2 A0 TRUTH,.LOVE NOR
90BC    C3 CF D5 D2 C1 C7 C5 8D C9 D3 A0 D0 D2 C9 C4 C5 COURAGE.IS PRIDE
90CC    AE 8D 00 20 00 08 20 21 08 8D D0 D2 C9 C4 C5 A0 ... .. !..PRIDE
90DC    C2 C5 C9 CE C7 A0 CE CF D4 8D C1 A0 D6 C9 D2 D4 BEING NOT.A VIRT
90EC    D5 C5 A0 CD D5 D3 D4 A0 C2 C5 8D D3 C8 D5 CE CE UE MUST BE.SHUNN
90FC    C5 C4 A0 C9 CE A0 C6 C1 D6 CF D2 8D CF C6 A0 C8 ED IN FAVOR.OF H
910C    D5 CD C9 CC C9 D4 D9 AC A0 D4 C8 C5 8D D6 C9 D2 UMILITY, THE.VIR
911C    D4 D5 C5 A0 D7 C8 C9 C3 C8 A0 C9 D3 8D D4 C8 C5 TUE WHICH IS.THE
912C    A0 C1 CE D4 C9 D4 C8 C5 D3 C9 D3 8D CF C6 A0 D0  ANTITHESIS.OF P
913C    D2 C9 C4 C5 A1 8D 00                   RIDE!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; AVAT

9146    20 1B 8A 20 21 08
914C    D4 CF A0 C2 C5 A0 C1 CE A0 C1 D6 C1 D4 C1 D2 8D TO BE AN AVATAR.
915C    C9 D3 A0 D4 CF A0 C2 C5 A0 D4 C8 C5 8D C5 CD C2 IS TO BE THE.EMB
916C    CF C4 C9 CD C5 CE D4 A0 CF C6 8D D4 C8 C5 A0 C5 ODIMENT OF.THE E
917C    C9 C7 C8 D4 8D D6 C9 D2 D4 D5 C5 D3 AE 8D 00 20 IGHT.VIRTUES...
918C    00 08 20 21 08 8D C9 D4 A0 C9 D3 A0 D4 CF A0 CC .. !..IT IS TO L
919C    C9 D6 C5 A0 C1 8D CC C9 C6 C5 A0 C3 CF CE D3 D4 IVE A.LIFE CONST
91AC    C1 CE D4 CC D9 8D C1 CE C4 A0 C6 CF D2 C5 D6 C5 ANTLY.AND FOREVE
91BC    D2 A0 C9 CE 8D D4 C8 C5 A0 D1 D5 C5 D3 D4 A0 D4 R IN.THE QUEST T
91CC    CF 8D C2 C5 D4 D4 C5 D2 A0 D4 C8 D9 D3 C5 CC C6 O.BETTER THYSELF
91DC    8D C1 CE C4 A0 D4 C8 C5 A0 D7 CF D2 CC C4 A0 C9 .AND THE WORLD I
91EC    CE 8D D7 C8 C9 C3 C8 A0 D7 C5 A0 CC C9 D6 C5 AE N.WHICH WE LIVE.
91FC    8D 00                                  ..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; QUES

9201    20 1B 8A 20 21 08 D4 C8 C5 A0 D1                      .. !.THE Q
920C    D5 C5 D3 D4 A0 CF C6 8D D4 C8 C5 A0 C1 D6 C1 D4 UEST OF.THE AVAT
921C    C1 D2 A0 C9 D3 8D C9 D3 A0 D4 CF A0 CB CE CF D7 AR IS.IS TO KNOW
922C    A0 C1 CE C4 8D C2 C5 C3 CF CD C5 A0 D4 C8 C5 8D  AND.BECOME THE.
923C    C5 CD C2 CF C4 C9 CD C5 CE D4 A0 CF C6 8D D4 C8 EMBODIMENT OF.TH
924C    C5 A0 C5 C9 C7 C8 D4 8D D6 C9 D2 D4 D5 C5 D3 A0 E EIGHT.VIRTUES
925C    CF C6 8D C7 CF CF C4 CE C5 D3 D3 A1 8D 00 20 00 OF.GOODNESS!.. .
926C    08 20 21 08 8D C9 D4 A0 C9 D3 A0 CB CE CF D7 CE . !..IT IS KNOWN
927C    A0 D4 C8 C1 D4 8D C1 CC CC A0 D7 C8 CF A0 D4 C1  THAT.ALL WHO TA
928C    CB C5 A0 CF CE 8D D4 C8 C9 D3 A0 D1 D5 C5 D3 D4 KE ON.THIS QUEST
929C    A0 CD D5 D3 D4 8D D0 D2 CF D6 C5 A0 D4 C8 C5 CD  MUST.PROVE THEM
92AC    D3 C5 CC D6 C5 D3 8D C2 D9 A0 C3 CF CE D1 D5 C5 SELVES.BY CONQUE
92BC    D2 C9 CE C7 8D D4 C8 C5 A0 C1 C2 D9 D3 D3 A0 C1 RING.THE ABYSS A
92CC    CE C4 8D D6 C9 C5 D7 C9 CE C7 A0 D4 C8 C5 8D C3 ND.VIEWING THE.C
92DC    CF C4 C5 D8 A0 CF C6 8D D5 CC D4 C9 CD C1 D4 C5 ODEX OF.ULTIMATE
92EC    A0 D7 C9 D3 C4 CF CD A1 8D 00           WISDOM!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; BRIT

92F9    20 35 8A
92FC    20 21 08 C5 D6 C5 CE A0 D4 C8 CF D5 C7 C8 A0 D4  !.EVEN THOUGH T
930C    C8 C5 8D C7 D2 C5 C1 D4 A0 C5 D6 C9 CC A0 CC CF HE.GREAT EVIL LO
931C    D2 C4 D3 8D C8 C1 D6 C5 A0 C2 C5 C5 CE A0 D2 CF RDS.HAVE BEEN RO
932C    D5 D4 C5 C4 8D C5 D6 C9 CC A0 D9 C5 D4 A0 D2 C5 UTED.EVIL YET RE
933C    CD C1 C9 CE D3 8D C9 CE A0 C2 D2 C9 D4 C1 CE CE MAINS.IN BRITANN
934C    C9 C1 AE 8D 00 20 00 08 20 21 08 8D C9 C6 A0 C2 IA... .. !..IF B
935C    D5 D4 A0 CF CE C5 A0 D3 CF D5 CC 8D C3 CF D5 CC UT ONE SOUL.COUL
936C    C4 A0 C3 CF CD D0 CC C5 D4 C5 8D D4 C8 C5 A0 D1 D COMPLETE.THE Q
937C    D5 C5 D3 D4 A0 CF C6 A0 D4 C8 C5 8D C1 D6 C1 D4 UEST OF THE.AVAT
938C    C1 D2 AC A0 CF D5 D2 8D D0 C5 CF D0 CC C5 A0 D7 AR, OUR.PEOPLE W
939C    CF D5 CC C4 8D C8 C1 D6 C5 A0 C1 A0 CE C5 D7 A0 OULD.HAVE A NEW
93AC    C8 CF D0 C5 AC 8D C1 A0 CE C5 D7 A0 C7 CF C1 CC HOPE,.A NEW GOAL
93BC    A0 C6 CF D2 8D CC C9 C6 C5 AE 8D 00 20 00 08 20  FOR.LIFE... ..
93CC    21 08 8D D4 C8 C5 D2 C5 A0 D7 CF D5 CC C4 A0 C2 !..THERE WOULD B
93DC    C5 A0 C1 8D D3 C8 C9 CE C9 CE C7 A0 C5 D8 C1 CD E A.SHINING EXAM
93EC    D0 CC C5 8D D4 C8 C1 D4 A0 D4 C8 C5 D2 C5 A0 C9 PLE.THAT THERE I
93FC    D3 8D CD CF D2 C5 A0 D4 CF A0 CC C9 C6 C5 8D D4 S.MORE TO LIFE.T
940C    C8 C1 CE A0 D4 C8 C5 A0 C5 CE C4 CC C5 D3 D3 8D HAN THE ENDLESS.
941C    D3 D4 D2 D5 C7 C7 CC C5 A0 C6 CF D2 8D D0 CF D3 STRUGGLE FOR.POS
942C    D3 C5 D3 D3 C9 CF CE D3 8D C1 CE C4 A0 C7 CF CC SESSIONS.AND GOL
943C    C4 A1 8D 00                            D!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; ANKH

9443    20 35 8A 20 21 08 D4 C8 C5                              5. !.THE
944C    A0 C1 CE CB C8 A0 C9 D3 A0 D4 C8 C5 8D D3 D9 CD  ANKH IS THE.SYM
945C    C2 CF CC A0 CF C6 A0 CF CE C5 8D D7 C8 CF A0 D3 BOL OF ONE.WHO S
946C    D4 D2 C9 D6 C5 D3 A0 C6 CF D2 8D D6 C9 D2 D4 D5 TRIVES FOR.VIRTU
947C    C5 AC A0 CB C5 C5 D0 A0 C9 D4 8D D7 C9 D4 C8 A0 E, KEEP IT.WITH
948C    D4 C8 C5 C5 A0 C1 D4 8D D4 C9 CD C5 D3 A0 C6 CF THEE AT.TIMES FO
949C    D2 A0 C2 D9 8D D4 C8 C9 D3 A0 CD C1 D2 CB A0 D4 R BY.THIS MARK T
94AC    C8 CF D5 8D D3 C8 C1 CC D4 A0 C2 C5 A0 CB CE CF HOU.SHALT BE KNO
94BC    D7 CE A1 8D 00                         WN!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; HELP

94C4    A9 00 20 20 03 20 1B 08 ).  . ..
94CC    84 C2 D2 D5 CE A0 C8 C5 CC D0 AC C1 A4 B8 B8 B0 .BRUN HELP,A$880
94DC    B0 8D 00                                        0..

;; ABYS

94DF    20 35 8A 20 21 08 D4 C8 C5 A0 C7 D2 C5  5. !.THE GRE
94EC    C1 D4 8D D3 D4 D9 C7 C9 C1 CE A0 C1 C2 D9 D3 D3 AT.STYGIAN ABYSS
94FC    8D C9 D3 A0 D4 C8 C5 A0 C4 C1 D2 CB C5 D3 D4 8D .IS THE DARKEST.
950C    D0 CF C3 CB C5 D4 A0 CF C6 A0 C5 D6 C9 CC 8D D2 POCKET OF EVIL.R
951C    C5 CD C1 C9 CE C9 CE C7 A0 C9 CE 8D C2 D2 C9 D4 EMAINING IN.BRIT
952C    C1 CE CE C9 C1 A1 8D 00 20 00 08 20 21 08 8D C9 ANNIA!.. .. !..I
953C    D4 A0 C9 D3 A0 D3 C1 C9 C4 A0 D4 C8 C1 D4 8D C9 T IS SAID THAT.I
954C    CE A0 D4 C8 C5 A0 C4 C5 C5 D0 C5 D3 D4 8D D2 C5 N THE DEEPEST.RE
955C    C3 C5 D3 D3 C5 D3 A0 CF C6 A0 D4 C8 C5 8D C1 C2 CESSES OF THE.AB
956C    D9 D3 D3 A0 C9 D3 A0 D4 C8 C5 8D C3 C8 C1 CD C2 YSS IS THE.CHAMB
957C    C5 D2 A0 CF C6 A0 D4 C8 C5 8D C3 CF C4 C5 D8 A1 ER OF THE.CODEX!
958C    8D 00 20 00 08 20 21 08 8D C9 D4 A0 C9 D3 A0 C1 .. .. !..IT IS A
959C    CC D3 CF A0 D3 C1 C9 C4 8D D4 C8 C1 D4 A0 CF CE LSO SAID.THAT ON
95AC    CC D9 A0 CF CE C5 A0 CF C6 8D C8 C9 C7 C8 C5 D3 LY ONE OF.HIGHES
95BC    D4 A0 D6 C9 D2 D4 D5 C5 8D CD C1 D9 A0 C5 CE D4 T VIRTUE.MAY ENT
95CC    C5 D2 A0 D4 C8 C9 D3 8D C3 C8 C1 CD C2 C5 D2 AC ER THIS.CHAMBER,
95DC    A0 CF CE C5 8D D3 D5 C3 C8 A0 C1 D3 A0 C1 CE 8D  ONE.SUCH AS AN.
95EC    C1 D6 C1 D4 C1 D2 A1 A1 A1 8D 00       AVATAR!!!..

4C 8F 89    JMP $898F       ; prompt for request after first time

;; MOND

95FA    20 35 8A 20 21 08

9600    CD CF CE C4 C1 C9 CE A0 C9 D3 A0 C4 C5 C1 C4 A1 8D 00                   MONDAIN IS DEAD!^M^@

9612    4C 8F 89    JMP $898F       ; prompt for request after first time

;; MINA

9615    20 35 8A 20 21 08

961B    CD C9 CE C1 D8 A0 C9 D3 A0 C4 C5 C1 C4 A1 8D 00                         MINAX IS DEAD!^M^@

962B    4C 8F 89    JMP $898F       ; prompt for request after first time

;; EXOD

962E    20 35 8A 20 21 08

9634    C5 D8 CF C4 D5 D3 A0 C9 D3 A0 C4 C5 C1 C4 A1 8D 00                      EXODUS IS DEAD!^M^@

4C 8F 89    JMP $898F       ; prompt for request after first time

;; VIRT

9648    20 35 8A 20 21 08
964E    D4 C8 C5 A0 C5 C9 C7 C8 D4 8D D6 C9 D2 D4 THE EIGHT.VIRT
965C    D5 C5 D3 A0 CF C6 A0 D4 C8 C5 8D C1 D6 C1 D4 C1 UES OF THE.AVATA
966C    D2 A0 C1 D2 C5 BA 8D 00 20 00 08 20 21 08 C8 CF R ARE:.. .. !.HO
967C    CE C5 D3 D4 D9 AC 8D C3 CF CD D0 C1 D3 D3 C9 CF NESTY,.COMPASSIO
968C    CE AC 8D D6 C1 CC CF D2 AC 8D CA D5 D3 D4 C9 C3 N,.VALOR,.JUSTIC
969C    C5 AC 8D D3 C1 C3 D2 C9 C6 C9 C3 C5 AC 8D C8 CF E,.SACRIFICE,.HO
96AC    CE CF D2 AC 8D D3 D0 C9 D2 C9 D4 D5 C1 CC C9 D4 NOR,.SPIRITUALIT
96BC    D9 AC 8D C1 CE C4 A0 C8 D5 CD C9 CC C9 D4 D9 A1 Y,.AND HUMILITY!
96CC    8D 00

4C 8F 89    JMP $898F       ; prompt for request after first time

;;

96D1    20 21 08    JSR $0821       ;

96D4    CC CF D2 C4 A0 C2 D2 C9 D4 C9 D3 C8 8D                                  LORD BRITISH^M
96E1    D2 C9 D3 C5 D3 A0 C1 CE C4 A0 D3 C1 D9 D3 8D                            RISES AND SAYS^M
96F0    C1 D4 A0 CC CF CE C7 A0 CC C1 D3 D4 A1 8D 00                            AT LONG LAST!^M^@

96FF    A9 01       LDA #$01        ; .
9701    85 D4       STA $D4         ; set character number to 1
9703    20 30 08    JSR $0830       ; print character name
9706    20 21 08    JSR $0821       ; print

9709    8D D4 C8 ^MTH
970C    CF D5 A0 C8 C1 D3 D4 A0 C3 CF CD C5 A1 8D D7 C5 OU HAST COME!.WE
971C    A0 C8 C1 D6 C5 A0 D7 C1 C9 D4 C5 C4 8D D3 D5 C3  HAVE WAITED.SUC
972C    C8 A0 C1 A0 CC CF CE C7 AC 8D CC CF CE C7 A0 D4 H A LONG,.LONG T
973C    C9 CD C5 AE AE AE 8D 00 IME.....
20 00 08 20 21 08
8D CC  .L
974C    CF D2 C4 A0 C2 D2 C9 D4 C9 D3 C8 8D D3 C9 D4 D3 ORD BRITISH.SITS
975C    A0 C1 CE C4 A0 D3 C1 D9 D3 BA 8D C1 A0 CE C5 D7  AND SAYS:.A NEW
976C    A0 C1 C7 C5 A0 C9 D3 8D D5 D0 CF CE A0 C2 D2 C9  AGE IS.UPON BRI
977C    D4 C1 CE CE C9 C1 AE 8D D4 C8 C5 A0 C7 D2 C5 C1 TANNIA..THE GREA
978C    D4 A0 C5 D6 C9 CC 8D CC CF D2 C4 D3 A0 C1 D2 C5 T EVIL.LORDS ARE
979C    A0 C7 CF CE C5 8D C2 D5 D4 A0 CF D5 D2 A0 D0 C5  GONE.BUT OUR PE
97AC    CF D0 CC C5 8D CC C1 C3 CB A0 C4 C9 D2 C5 C3 D4 OPLE.LACK DIRECT
97BC    C9 CF CE 8D C1 CE C4 A0 D0 D5 D2 D0 CF D3 C5 A0 ION.AND PURPOSE
97CC    C9 CE 8D D4 C8 C5 C9 D2 A0 CC C9 D6 C5 D3 AE AE IN.THEIR LIVES..
97DC    AE 00 ..
20 00 08 20 21 08
8D 8D C1 A0 C3 C8 C1 CD ..A CHAM
97EC    D0 C9 CF CE A0 CF C6 8D D6 C9 D2 D4 D5 C5 A0 C9 PION OF.VIRTUE I
97FC    D3 A0 C3 C1 CC CC C5 C4 8D C6 CF D2 AE A0 D4 C8 S CALLED.FOR. TH
980C    CF D5 A0 CD C1 D9 A0 C2 C5 8D D4 C8 C9 D3 A0 C3 OU MAY BE.THIS C
981C    C8 C1 CD D0 C9 CF CE AC 8D C2 D5 D4 A0 CF CE CC HAMPION,.BUT ONL
982C    D9 A0 D4 C9 CD C5 8D D3 C8 C1 CC CC A0 D4 C5 CC Y TIME.SHALL TEL
983C    CC AE A0 C9 8D D7 C9 CC CC A0 C1 C9 C4 A0 D4 C8 L. I.WILL AID TH
984C    C5 C5 8D C1 CE D9 A0 D7 C1 D9 A0 D4 C8 C1 D4 A0 EE.ANY WAY THAT
985C    C9 8D C3 C1 CE A1 8D 00 I.CAN!..
20 00 08 20 21 08
8D C8 .H
986C    CF D7 A0 CD C1 D9 A0 C9 8D C8 C5 CC D0 A0 D4 C8 OW MAY I.HELP TH
987C    C5 C5 BF 8D 00

4C A4 89

;; things to say to Lord British

9884    A0 A0 A0 A0               00
9888    C2 D9 C5 A0               01      BYE
988C    CE C1 CD C5               02      NAME
9890    CC CF CF CB               03      LOOK
9894    CA CF C2 A0               04      JOB
9898    C8 C5 C1 CC               05      HEAL
989C    D4 D2 D5 D4               06      TRUT
98A0    CC CF D6 C5               07      LOVE
98A4    C3 CF D5 D2               08      COUR
98A8    C8 CF CE C5               09      HONE
98AC    C3 CF CD D0               0A      COMP
98B0    D6 C1 CC CF               0B      VALO
98B4    CA D5 D3 D4               0C      JUST
98B8    D3 C1 C3 D2               0D      SACR
98BC    C8 CF CE CF               0E      HONO
98C0    D3 D0 C9 D2               0F      SPIR
98C4    C8 D5 CD C9               10      HUMI
98C8    D0 D2 C9 C4               11      PRID
98CC    C1 D6 C1 D4               12      AVAT
98D0    D1 D5 C5 D3               13      QUES
98D4    C2 D2 C9 D4               14      BRIT
98D8    C1 CE CB C8               15      ANKH
98DC    C8 C5 CC D0               16      HELP
98E0    C1 C2 D9 D3               17      ABYS
98E4    CD CF CE C4               18      MOND
98E8    CD C9 CE C1               19      MINA
98EC    C5 D8 CF C4               1A      EXOD
98F0    D6 C9 D2 D4               1B      VIRT
98F4    00 00 00 00

;; convert what was typed in to the command number above (e.g. JOB == 0x04)
; or $FF if unknown command

98F8    A9 00       LDA #$00        ; _EA = 0
98FA    85 EA       STA $EA         ;

98FC    A5 EA       LDA $EA         ; A = _EA
98FE    0A          ASL A           ;
98FF    0A          ASL A           ;
9900    A8          TAY             ; Y = A * 4
9901    A2 00       LDX #$00        ; X = 0x00

9903    B9 84 98    LDA $9884,Y     ; look at COMMANDS[_EA * 4]
9906    F0 13       BEQ $991B       ; if zero, we've hit end so go return 0xFF
9908    DD 00 03    CMP $0300,X     ; if character doesn't match between buffer and command table
990B    D0 09       BNE $9916       ;     go on to next command
990D    C8          INY             ; if it does much, increment Y (index into commands)
990E    E8          INX             ;     and increment X (index into buffer)
990F    E0 04       CPX #$04        ; if index is X < 4,
9911    90 F0       BCC $9903       ;     look back to next character
9913    A5 EA       LDA $EA         ; otherwise return
9915    60          RTS             ;     the command index

9916    E6 EA       INC $EA         ; increment $EA and
9918    4C FC 98    JMP $98FC       ;   loop back

991B    A9 FF       LDA #$FF        ;
991D    60          RTS             ;

;; get input

991E    A9 BF       LDA #$BF        ; '?'
9920    20 24 08    JSR $0824       ; print character
9923    A9 00       LDA #$00        ; _EA = 0x00
9925    85 EA       STA $EA         ; .

9927    20 00 08    JSR $0800       ; get input?
992A    C9 8D       CMP #$8D        ; if ^M
992C    F0 2C       BEQ $995A       ;   go to $995A

992E    C9 88       CMP #$88        ; if ^H
9930    F0 16       BEQ $9948       ;   go to $9948

9932    C9 A0       CMP #$A0        ; if space
9934    90 F1       BCC $9927       ;   go back to $9927

;; handle everything else

9936    A6 EA       LDX $EA         ; X = character buffer pointer
9938    9D 00 03    STA $0300,X     ; store A in character buffer
993B    20 24 08    JSR $0824       ; print character
993E    E6 EA       INC $EA         ; increment character buffer pointer
9940    A5 EA       LDA $EA         ;
9942    C9 0F       CMP #$0F        ; if pointer < 0x0F
9944    90 E1       BCC $9927       ;   go back to $9927
9946    B0 12       BCS $995A       ; otherwise (>= 0x0F), go to $995A

;; handle ^H

9948    A5 EA       LDA $EA         ; get character buffer pointer
994A    F0 DB       BEQ ---         ; if zero, back to $9927
994C    C6 EA       DEC $EA         ; decrement buffer pointer
994E    C6 CE       DEC $CE         ; decrement text column
9950    A9 A0       LDA #$A0        ; ' '
9952    20 24 08    JSR $0824       ; print character (to overwrite)
9955    C6 CE       DEC $CE         ; decrement text column again
9957    4C 27 99    JMP $9927       ; back to $9927

;; handle ^M or max string limit

; pad buffer from pointer to offset 0x05 with spaces ???

995A    A6 EA       LDX $EA         ; load character buffer pointer
995C    A9 A0       LDA #$A0        ; ' '

995E    9D 00 03    STA $0300,X     ; store space at end of character buffer
9961    E8          INX             ; X++
9962    E0 06       CPX #$06        ; if X < 0x06
9964    90 F8       BCC $995E       ;   loop back to $$995E

9966    A9 8D       LDA #$8D        ; '^M'
9968    20 24 08    JSR $0824       ; print character
996B    60          RTS             ;

;; ???

996C    68          PLA             ;
996D    85 FE       STA $FE         ;
996F    68          PLA             ;
9970    85 FF       STA $FF         ;
9972    A0 00       LDY #$00        ;
9974    84 F0       STY $F0         ;
9976    A2 FF       LDX #$FF        ;
9978    E8          INX             ;
9979    E6 FE       INC $FE         ;
997B    D0 02       BNE ---         ;
997D    E6 FF       INC $FF         ;
997F    B1 FE       LDA ($FE),Y     ;
9981    F0 0A       BEQ ---         ;
9983    DD 00 03    CMP $0300,X     ;
9986    F0 F0       BEQ ---         ;
9988    E6 F0       INC $F0         ;
998A    4C 78 99    JMP $9978       ;

998D    A5 FF       LDA $FF         ;
998F    48          PHA             ;
9990    A5 FE       LDA $FE         ;
9992    48          PHA             ;
9993    A5 F0       LDA $F0         ;

9995    60          RTS             ;

;; for all living players, set status to 'G' and set health to max

9996    A5 0F       LDA $0F         ; load the number of characters in to $D4
9998    85 D4       STA $D4         ; .

999A    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4
999D    A0 12       LDY #$12        ;
999F    B1 FE       LDA ($FE),Y     ; get player status (0x12 in player vector)
99A1    C9 C4       CMP #$C4        ; if == 'D'
99A3    F0 14       BEQ $99B9       ;     skip to next player

99A5    A9 C7       LDA #$C7        ; set player status to 'G'
99A7    91 FE       STA ($FE),Y     ; .
99A9    A0 1A       LDY #$1A        ; set health to max
99AB    B1 FE       LDA ($FE),Y     ; .
99AD    A0 18       LDY #$18        ; .
99AF    91 FE       STA ($FE),Y     ; .
99B1    A0 1B       LDY #$1B        ; .
99B3    B1 FE       LDA ($FE),Y     ; .
99B5    A0 19       LDY #$19        ; .
99B7    91 FE       STA ($FE),Y     ; .

99B9    C6 D4       DEC $D4         ; decrement to next player
99BB    D0 DD       BNE $999A       ; if more players, loop back

99BD    60          RTS             ;

;; ???

99BE    C9 00       CMP #$00        ; if A == 00
99C0    F0 0B       BEQ $99CD       ;     return
99C2    A2 00       LDX #$00        ; X = 00
99C4    F8          SED             ; use BCD
99C5    E8          INX             ; X++

99C6    38          SEC             ; set carry
99C7    E9 01       SBC #$01        ; subtract one from A
99C9    D0 FA       BNE $99C6       ; if not zero, loop back

99CB    8A          TXA             ; A = X
99CC    D8          CLD             ;
99CD    60          RTS             ;

;;

99CE    60          RTS             ; global return

99CF    02 FF
```