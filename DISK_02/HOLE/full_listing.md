---
grand_parent: DISK 1
parent: HOLE
nav_order: 1
---

# HOLE full listing

Loaded into $8800 by ULT4 $557A in 'H' handler (Hole up and camp).

```
8800    20 1B 08    JSR $081B       ; print to load file

8803    84 C2 CC CF C1 C4 A0 C3 C1 CD D0 AC C1 A4 B2 B4 B0 8D 00                ^DBLOAD CAMP,A$240^M

8816    A9 00       LDA #$00        ; A = 0x00
8818    AA          TAX             ; X = 0x00

;; clear $EFXX to zero

8819    9D 00 EF    STA $EF00,X     ;
881C    CA          DEX             ;
881D    D0 FA       BNE $8819       ;

;; loop over each character in party

881F    A5 0F       LDA $0F         ; put party size
8821    85 D4       STA $D4         ;   in character number

8823    20 16 89    JSR $8916       ; for the character in $D4, set A := 00 if alive (GPS) or FF if dead
8826    30 14       BMI $883C       ; if dead, skip, so if alive...
8828    A9 38       LDA #$38        ; A := 0x38
882A    A6 D4       LDX $D4         ; X = character_num - 1
882C    CA          DEX             ; .
882D    9D A0 EF    STA $EFA0,X     ; $EFAx := 0x38
8830    BD 60 02    LDA $0260,X     ; .
8833    9D 80 EF    STA $EF80,X     ; $EF8x := $0260 + (character_num - 1)
8836    BD 68 02    LDA $0268,X     ; .
8839    9D 90 EF    STA $EF90,X     ; $EF9x := $0268 + (character_num - 1)

883C    C6 D4       DEC $D4         ; go to next character
883E    D0 E3       BNE $8823       ; and loop back

8840    20 21 08    JSR $0821       ; PRINT

8843    D2 C5 D3 D4 C9 CE C7 AE AE AE 8D 00                                     RESTING...^M

884F    A9 4B       LDA #$4B        ;
8851    85 EA       STA $EA         ;

8853    20 57 08    JSR $0857       ;
8856    C6 EA       DEC $EA         ;
8858    D0 F9       BNE $8853       ;

885A    20 4E 08    JSR $084E       ; random number generator?
885D    29 07       AND #$07        ; if any three lower bits 0 (87.5% chance)
885F    D0 55       BNE $88B6       ; skip...

;; ambush with 12.5% chance

8861    20 21 08    JSR $0821       ; PRINT

8864    C1 CD C2 D5 D3 C8 C5 C4 A1 8D 00                                        AMBUSHED!^M

886F    20 4E 08    JSR $084E       ; random number generator?
8872    29 07       AND #$07        ;
8874    A8          TAY             ;
8875    B9 0E 89    LDA $890E,Y     ;
8878    85 C0       STA $C0         ;
887A    85 E6       STA $E6         ;
887C    A5 00       LDA $00         ;
887E    85 C1       STA $C1         ;
8880    A5 01       LDA $01         ;
8882    85 C2       STA $C2         ;
8884    A5 0F       LDA $0F         ;
8886    85 D4       STA $D4         ;

8888    20 2D 08    JSR $082D       ; set $FE/$FF vector based on $D4 ???
888B    A0 12       LDY #$12        ;
888D    B1 FE       LDA ($FE),Y     ;
888F    C9 C7       CMP #$C7        ; 'G'
8891    D0 04       BNE $8897       ;
8893    A9 D3       LDA #$D3        ; 'S'
8895    91 FE       STA ($FE),Y     ;

8897    C6 D4       DEC $D4         ;
8899    D0 ED       BNE $8888       ;
889B    20 1B 08    JSR $081B       ; print to load file

889E    84 C2 CC CF C1 C4 A0 C3 C1 CD D0 AC C1 A4 B2 B4 B0 8D 00                ^DBLOAD CAMP,A$240^M

88B1    68          PLA             ;
88B2    68          PLA             ;
88B3    4C 06 40    JMP $4006       ; enter combat
```

```
;; no ambush

;; if $1E == $16 we print NO EFFECT and leave (not sure what $1E or $16 are yet)

88B6    A5 1E       LDA $1E         ;
88B8    C5 16       CMP $16         ;
88BA    D0 12       BNE $88CE       ;
88BC    20 21 08    JSR $0821       ; PRINT

88BF    CE CF A0 C5 C6 C6 C5 C3 D4 AE 8D 00                                     NO EFFECT.^M

88CB    4C 0D 89    JMP $890D       ; return
```

```

;; normal rest (no ambush and no "NO EFFECT" triggered)

88CE    85 16       STA $16         ; store value of $1E in $16

;; loop over characters in party

88D0    A5 0F       LDA $0F         ; set $D4 to last character
88D2    85 D4       STA $D4         ; .

88D4    20 16 89    JSR $8916       ; for the character in $D4, set A := 00 if alive (GPS) or FF if dead
88D7    30 19       BMI $88F2       ; if dead, jump to next character, otherwise...

88D9    A0 12       LDY #$12        ; load character status
88DB    B1 FE       LDA ($FE),Y     ; .
88DD    C9 D3       CMP #$D3        ; if 'S' (sleeping)
88DF    D0 04       BNE $88E5       ; .
88E1    A9 C7       LDA #$C7        ; set to 'G' (good)
88E3    91 FE       STA ($FE),Y     ; .

;; increase health by random amount

88E5    20 4E 08    JSR $084E       ; random number generator?
88E8    29 77       AND #$77        ;
88EA    20 2F 89    JSR $892F       ; restore health for character by up to amount in accumulator
88ED    A9 99       LDA #$99        ;
88EF    20 2F 89    JSR $892F       ; restore health for character by up to amount in accumulator

88F2    C6 D4       DEC $D4         ; loop back to next character
88F4    D0 DE       BNE $88D4       ;

88F6    20 6B 89    JSR $896B       ;
88F9    20 21 08    JSR $0821       ; PRINT

88FC    D0 CC C1 D9 C5 D2 D3 A0 C8 C5 C1 CC C5 C4 A1 8D 00  PLAYERS HEALED!^M

890D    60          RTS             ;
```

```
;; DATA?

890E    C0 C4 C8 CC B4 A0 A4 DC

;; for the character in $D4, return 00 if alive (GPS) or FF if dead

8916    20 2D 08    JSR $082D       ; set $FE/$FF vector to character data for character $D4
8919    A0 12       LDY #$12        ; load player status
891B    B1 FE       LDA ($FE),Y     ; .
891D    C9 C7       CMP #$C7        ; if 'G' (good)
891F    F0 0B       BEQ $892C       ;   return A = 00
8921    C9 D0       CMP #$D0        ; if 'P' (poisoned)
8923    F0 07       BEQ $892C       ;   return A = 00
8925    C9 D3       CMP #$D3        ; if 'S' (sleeping)
8927    F0 03       BEQ $892C       ;   return A = 00
8929    A9 FF       LDA #$FF        ; else:
892B    60          RTS             ;   return A = FF

892C    A9 00       LDA #$00        ;
892E    60          RTS             ;
```

```
;; restore health for character by up to amount in accumulator

892F    85 DA       STA $DA         ; store argument in $DA (fixed + random amount)
8931    20 2D 08    JSR $082D       ; set $FE/$FF vector to character data for character $D4

;; restore health by $DA

8934    A0 19       LDY #$19        ; lower two digital of character health
8936    F8          SED             ;
8937    18          CLC             ;
8938    B1 FE       LDA ($FE),Y     ;
893A    65 DA       ADC $DA         ;
893C    91 FE       STA ($FE),Y     ;
893E    88          DEY             ;
893F    B1 FE       LDA ($FE),Y     ;
8941    69 00       ADC #$00        ;
8943    91 FE       STA ($FE),Y     ;
8945    D8          CLD             ;

;; clip health by maximum

8946    A0 18       LDY #$18        ; compare most-significant digit of health
8948    B1 FE       LDA ($FE),Y     ;
894A    A0 1A       LDY #$1A        ; with most-significant digit of max health
894C    D1 FE       CMP ($FE),Y     ;
894E    90 1A       BCC $896A       ; and if less, return

8950    A0 19       LDY #$19        ; compare lower two digits of health
8952    B1 FE       LDA ($FE),Y     ;
8954    A0 1B       LDY #$1B        ; with lower two digits of max health
8956    D1 FE       CMP ($FE),Y     ;
8958    90 10       BCC $896A       ; and if less, return

895A    A0 1A       LDY #$1A        ; set the most-significant digit of health to max
895C    B1 FE       LDA ($FE),Y     ;
895E    A0 18       LDY #$18        ;
8960    91 FE       STA ($FE),Y     ;

8962    A0 1B       LDY #$1B        ; set the lower two digits of health to max
8964    B1 FE       LDA ($FE),Y     ;
8966    A0 19       LDY #$19        ;
8968    91 FE       STA ($FE),Y     ;

896A    60          RTS             ;
```

```
;;

896B    A5 0F       LDA $0F         ; start with last character
896D    85 D4       STA $D4         ;

896F    20 16 89    JSR $8916       ; for the character in $D4, set A := 00 if alive (GPS) or FF if dead
8972    30 3F       BMI $89B3       ; if dead, skip to next character, otherwise
8974    A0 11       LDY #$11        ; get character property $11 (not sure what that is)
8976    B1 FE       LDA ($FE),Y     ;
8978    85 D8       STA $D8         ; store in $D8
897A    A0 15       LDY #$15        ; get character property $15 (not sure what that is)
897C    B1 FE       LDA ($FE),Y     ;
897E    20 D0 89    JSR $89D0       ; some sort of conversion to BCD?
8981    0A          ASL A           ;
8982    A6 D8       LDX $D8         ;
8984    F0 26       BEQ $89AC       ;
8986    CA          DEX             ;
8987    F0 19       BEQ $89A2       ;
8989    CA          DEX             ;
898A    F0 0C       BEQ $8998       ;
898C    CA          DEX             ;
898D    F0 17       BEQ $89A6       ;
898F    CA          DEX             ;
8990    F0 0B       BEQ $899D       ;
8992    CA          DEX             ;
8993    F0 0D       BEQ $89A2       ;
8995    CA          DEX             ;
8996    F0 0A       BEQ $89A2       ;
8998    A9 00       LDA #$00        ;
899A    4C AC 89    JMP $89AC       ;

899D    4A          LSR A           ;
899E    4A          LSR A           ;
899F    4C AC 89    JMP $89AC       ;

89A2    4A          LSR A           ;
89A3    4C AC 89    JMP $89AC       ;

89A6    4A          LSR A           ;
89A7    85 D8       STA $D8         ;
89A9    4A          LSR A           ;
89AA    65 D8       ADC $D8         ;

89AC    20 B8 89    JSR $89B8       ;
89AF    A0 16       LDY #$16        ;
89B1    91 FE       STA ($FE),Y     ;

89B3    C6 D4       DEC $D4         ; go to next character
89B5    10 B8       BPL $896F       ; loop back

89B7    60          RTS             ;
```

```
89B8    C9 00       CMP #$00        ;
89BA    F0 12       BEQ $89CE       ;
89BC    C9 63       CMP #$63        ;
89BE    B0 0C       BCS $89CC       ;
89C0    F8          SED             ;
89C1    AA          TAX             ;
89C2    A9 00       LDA #$00        ;
89C4    18          CLC             ;
89C5    69 01       ADC #$01        ;
89C7    CA          DEX             ;
89C8    D0 FA       BNE $89C4       ;
89CA    F0 02       BEQ $89CE       ;
89CC    A9 99       LDA #$99        ;
89CE    D8          CLD             ;
89CF    60          RTS             ;
```

```
;; some sort of conversion to BCD?

89D0    C9 00       CMP #$00        ;
89D2    F0 0B       BEQ $89DF       ;
89D4    A2 00       LDX #$00        ;
89D6    F8          SED             ;

89D7    E8          INX             ;
89D8    38          SEC             ;
89D9    E9 01       SBC #$01        ;
89DB    D0 FA       BNE $89D7       ;

89DD    8A          TXA             ;
89DE    D8          CLD             ;

89DF    60          RTS             ;
```
