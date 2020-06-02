---
grand_parent: DISK 1
parent: NEWGAME
nav_order: 1
---

# NEWGAME full listing

```
6400    2C 8B C0    BIT $C08B       ;
6403    2C 8B C0    BIT $C08B       ;
6406    4C 30 64    JMP $6430       ;

;;

6409    A9 00       LDA #$00        ; .
640B    85 B8       STA $B8         ; $B8 = 0x00
640D    A9 00       LDA #$00        ; song 0x00 (stop?)
640F    20 20 03    JSR $0320       ; .
6412    20 1B 08    JSR $081B       ; print to OS to load file

6415    84 C2 D2 D5 CE A0 D5 81 CC D4 B4 AC C1 A4 B4 B0 B0 B0 8D 00             ^DBRUN U.LT4,A$4000^M

;; get a key press

6429    20 00 08    JSR $0800       ; get a key press
642C    10 FB       BPL $6429       ;

642E    60          RTS

;; DATA

; starts at 0x00
; changed to 0x1D after TREE.SPK displayed
; this is the text # to show next

642F    00

;;

6430    A9 00       LDA #$00        ; .
6432    8D F8 67    STA $67F8       ; $67F8 := 0x00 (??)
6435    A9 10       LDA #$10        ; set text column to 0x10
6437    85 CF       STA $CF         ; .

6439    20 1B 08    JSR $081B       ; print to OS to load file

643C    84 C2 CC CF C1 C4 A0 D4 81 D2 C5 C5 AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD T.REE.SPK,A$4000^M

6455    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
6458    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
645B    A9 1D       LDA #$1D        ; set $642F = 0x1D (the first of the texts at the tree)
645D    8D 2F 64    STA $642F       ; .
6460    4C 6C 64    JMP $646C       ;

;; all but the first tree text comes back here

6463    20 29 64    JSR $6429       ; get a key press
6466    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
6469    AD 2F 64    LDA $642F       ; A = $642F

;;

646C    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
646F    EE 2F 64    INC $642F       ; increment $642F (started at 0x1D)
6472    AD 2F 64    LDA $642F       ; A = $642F
6475    C9 21       CMP #$21        ; if 0x21 (we've just shown 0x20)
6477    D0 06       BNE $647F       ; .

6479    20 55 68    JSR $6855       ; portal appear?
647C    4C 63 64    JMP $6463       ; loop back to next screen
647F    C9 23       CMP #$23        ; otherwise, if 0x23
6481    D0 22       BNE $64A5       ; .
6483    20 9D 68    JSR $689D       ; portal disappear?
6486    20 1B 08    JSR $081B       ; print to OS to load file

6489    84 C2 CC CF C1 C4 A0 D0 81 D2 D4 CC AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD P.RTL.SPK,A$4000^M

64A2    4C 63 64    JMP $6463       ; loop back to next screen

;; not 0x23

64A5    C9 24       CMP #$24        ; if 0x24
64A7    D0 22       BNE $64CB       ; .

64A9    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
64AC    20 1B 08    JSR $081B       ; print to OS to load file

64AF    84 C2 CC CF C1 C4 A0 D4 81 D2 C5 C5 AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD T.REE.SPK,A$4000^M

64C8    4C 63 64    JMP $6463       ; loop back to next screen

;; not 0x24

64CB    C9 29       CMP #$29        ; if 0x29
64CD    D0 22       BNE $64F1       ; .

64CF    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
64D2    20 1B 08    JSR $081B       ; print to OS to load file

64D5    84 C2 CC CF C1 C4 A0 CC 81 CF CF CB AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD L.OOK.SPK,A$4000..

64EE    4C 63 64    JMP $6463       ; loop back to next screen

;; not 0x29

64F1    C9 2C       CMP #$2C        ; if 0x2C
64F3    D0 08       BNE $64FD       ; .

64F5    A9 D4       LDA #$D4        ; play song 0xD4
64F7    20 20 03    JSR $0320       ; .
64FA    4C 63 64    JMP $6463       ; loop back to next screen

;; not 0x2C

64FD    C9 2D       CMP #$2D        ; if 0x2D
64FF    D0 22       BNE $6523       ; .

6501    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
6504    20 1B 08    JSR $081B       ; print to OS to load file

6507    84 C2 CC CF C1 C4 A0 C6 81 C1 C9 D2 AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD F.AIR.SPK,A$4000..

6520    4C 63 64    JMP $6463       ;

;; not 0x2D

6523    C9 2F       CMP #$2F        ;
6525    D0 22       BNE $6549       ;

6527    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
652A    20 1B 08    JSR $081B       ; print to OS to load file

652D    84 C2 CC CF C1 C4 A0 D7 81 C1 C7 CE AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD W.AGN.SPK,A$4000..

6546    4C 63 64    JMP $6463       ;

;;

6549    C9 32       CMP #$32        ;
654B    D0 22       BNE $656F       ;

654D    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
6550    20 1B 08    JSR $081B       ; print to OS to load file

6553    84 C2 CC CF C1 C4 A0 C7 81 D9 D0 D3 AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD G.YPS.SPK,A$4000..

656C    4C 63 64    JMP $6463       ;

;;

656F    C9 33       CMP #$33        ;
6571    D0 22       BNE $6595       ;

6573    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
6576    20 1B 08    JSR $081B       ; print to OS to load file

6579    84 C2 CC CF C1 C4 A0 D4 81 C1 C2 CC AE D3 D0 CB AC C1 A4 B4 B0 B0 B0 8D 00      ^DBLOAD T.ABL.SPK,A$4000..

6592    4C 63 64    JMP $6463       ;

6595    C9 35       CMP #$35        ;
6597    D0 F9       BNE $6592       ;

6599    20 C1 6B    JSR $6BC1       ; unpack SPK image to hires screen
659C    20 1B 08    JSR $081B       ; print to OS to load file

659F    84 C2 CC CF C1 C4 A0 C3 81 D2 C4 D3 AC C1 A4 B4 B0 B0 B0 8D 00                  ^DBLOAD C.RDS,A$4000..

65B4    20 29 64    JSR $6429       ; get a key press
65B7    A9 00       LDA #$00        ; store 0x00 in $67F8 (number of card pairs shown?)
65B9    8D F8 67    STA $67F8       ; .

;; set all the $67E6[] to zero

65BC    A2 07       LDX #$07        ; .
65BE    A9 00       LDA #$00        ; .
65C0    9D E6 67    STA $67E6,X     ; .
65C3    CA          DEX             ; .
65C4    10 FA       BPL $65C0       ; .

;; looped back to if another card pair to ask

65C6    A2 0F       LDX #$0F        ;

65C8    BD 00 E0    LDA $E000,X     ;
65CB    85 FE       STA $FE         ;
65CD    BD C0 E0    LDA $E0C0,X     ;
65D0    85 FF       STA $FF         ;
65D2    A0 02       LDY #$02        ;

65D4    A9 2A       LDA #$2A        ;
65D6    91 FE       STA ($FE),Y     ;
65D8    C8          INY             ;
65D9    A9 55       LDA #$55        ;
65DB    91 FE       STA ($FE),Y     ;
65DD    C8          INY             ;
65DE    C0 0C       CPY #$0C        ;
65E0    D0 04       BNE $65E6       ;

65E2    A0 1C       LDY #$1C        ;
65E4    D0 EE       BNE $65D4       ;

65E6    C0 26       CPY #$26        ;
65E8    90 EA       BCC $65D4       ;

65EA    E8          INX             ;
65EB    10 DB       BPL $65C8       ;

65ED    AD F8 67    LDA $67F8       ;
65F0    C9 04       CMP #$04        ;
65F2    F0 04       BEQ $65F8       ;
65F4    C9 06       CMP #$06        ;
65F6    D0 03       BNE $65FB       ;

65F8    20 E1 68    JSR $68E1       ;
65FB    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12

; pick two random virtue cards not yet seen

65FE    20 2C 68    JSR $682C       ; get random virtue card that's still zero
6601    8E F6 67    STX $67F6       ; store it in $67F6

6604    20 2C 68    JSR $682C       ; get another random virtue card that's still zero
6607    EC F6 67    CPX $67F6       ; if it's the same as the first,
660A    F0 F8       BEQ $6604       ;    pick another one

660C    8E F7 67    STX $67F7       ; store second virtue card in $67F7
660F    EC F6 67    CPX $67F6       ; check order of cards and
6612    B0 09       BCS $661D       ; if necessary...

6614    AC F6 67    LDY $67F6       ; flip order of two cards
6617    8E F6 67    STX $67F6       ; .
661A    8C F7 67    STY $67F7       ; .

661D    AD F8 67    LDA $67F8       ; load ??
6620    D0 08       BNE $662A       ; if not zero, we've already had a question

6622    A9 35       LDA #$35        ; otherwise, display text #35: "the gypsy places the first two cards"
6624    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
6627    4C 3B 66    JMP $663B       ;

662A    C9 06       CMP #$06        ; if card pair == 0x06
662C    F0 08       BEQ $6636       ; skip

662E    A9 36       LDA #$36        ; otherwise, text #36: "the gypsy places two more of the cards"
6630    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
6633    4C 3B 66    JMP $663B       ; .

6636    A9 37       LDA #$37        ; so if card pair == 0x06, text: #37 "the gypsy places the last two cards"
6638    20 77 6A    JSR $6A77       ; display text (based on accumulator?)

663B    A9 38       LDA #$38        ; text: #38: "upon the table, they are the cards of"
663D    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
6640    AD F6 67    LDA $67F6       ; display name of virtue in $67F6 (first card)
6643    18          CLC             ; .
6644    69 39       ADC #$39        ; .
6646    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
6649    AD F6 67    LDA $67F6       ; A = card to show
664C    A2 00       LDX #$00        ; X = card position (0 = left)
664E    20 3E 6B    JSR $6B3E       ; display card image
6651    20 21 08    JSR $0821       ; print

6654    A0 E1 EE E4 A0 00       ' and '

665A    AD F7 67    LDA $67F7       ; display name of virtue in $67F7 (second card)
665D    18          CLC             ; .
665E    69 39       ADC #$39        ; .
6660    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
6663    20 21 08    JSR $0821       ; print

6666    AE A0 D3 E8 E5 A0 F3 E1 F9 F3 AC 00     . She says,

6672    A9 00       LDA #$00        ; print following at
6674    85 CE       STA $CE         ;   0x00, current text row
6676    E6 CF       INC $CF         ;   .
6678    20 21 08    JSR $0821       ;   .

667B    A7 C3 EF EE F3 E9 E4 E5 F2 A0 F4 E8 E9 F3 BA A7 00      'Consider this:'.

668C    AD F7 67    LDA $67F7       ; A = card to show
668F    A2 01       LDX #$01        ; X = card position (1 = right)
6691    20 3E 6B    JSR $6B3E       ; display card image
6694    20 29 64    JSR $6429       ; get a key press
6697    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12

; calculate the text and display the dilemma text to show given virtues $67F6 and $67F7

669A    AE F6 67    LDX $67F6       ; .
669D    BD EE 67    LDA $67EE,X     ; this gets the base text # of the left virtual
66A0    18          CLC             ; .
66A1    6D F7 67    ADC $67F7       ; add the second
66A4    20 77 6A    JSR $6A77       ; display the dilemma text

;; handle the response

66A7    20 29 64    JSR $6429       ; get a key press
66AA    C9 C1       CMP #$C1        ; is it 'A'
66AC    F0 10       BEQ $66BE       ;
66AE    C9 C2       CMP #$C2        ; is it 'B'
66B0    D0 F5       BNE $66A7       ; if not, go back and ask again

;; if the user selected 'B' swap the cards, so 'A' is always effectively what they picked

66B2    AE F7 67    LDX $67F7       ;
66B5    AC F6 67    LDY $67F6       ;
66B8    8E F6 67    STX $67F6       ;
66BB    8C F7 67    STY $67F7       ;

;; at this point $67F6 is what they picked and $67F7 is what they rejected

66BE    AE F6 67    LDX $67F6       ; X = virtue just picked
66C1    8E F5 67    STX $67F5       ; store virtue just picked in $67F5
66C4    A9 01       LDA #$01        ;
66C6    9D E6 67    STA $67E6,X     ; set the score for chosen virtue to 0x01

; add 0x05 to the virtue score of the virtue just chosen

66C9    18          CLC             ; .
66CA    F8          SED             ; .
66CB    BD 24 68    LDA $6824,X     ; .
66CE    69 05       ADC #$05        ; .
66D0    9D 24 68    STA $6824,X     ; .

; increment STR, DEX, INT based on chosen virtue

66D3    BD 0C 68    LDA $680C,X     ; STR += $680C[virtue]
66D6    6D 09 68    ADC $6809       ; .
66D9    8D 09 68    STA $6809       ; .
66DC    BD 14 68    LDA $6814,X     ; DEX += $6814[virtue]
66DF    6D 0A 68    ADC $680A       ; .
66E2    8D 0A 68    STA $680A       ; .
66E5    BD 1C 68    LDA $681C,X     ; INT += $681C[virtue]
66E8    6D 0B 68    ADC $680B       ; .
66EB    8D 0B 68    STA $680B       ; .
66EE    D8          CLD             ;

66EF    AC F8 67    LDY $67F8       ; Y = number of card pairs shown
66F2    A9 00       LDA #$00        ; A = 0x00
66F4    20 B6 6A    JSR $6AB6       ; draw bead? based on A = 00, X = chosen virtue, Y = number of card pairs shown

66F7    AE F7 67    LDX $67F7       ; X = virtual just rejected
66FA    A9 FF       LDA #$FF        ; .
66FC    9D E6 67    STA $67E6,X     ; set the score for rejected virtue to 0xFF

66FF    AC F8 67    LDY $67F8       ; Y = number of card pairs shown
6702    A9 01       LDA #$01        ; A = 0x01
6704    20 B6 6A    JSR $6AB6       ; draw bead? based on A = 1, X = rejected virtue, Y = number of card pairs shown

6707    EE F8 67    INC $67F8       ; increment number of card pairs shown
670A    AD F8 67    LDA $67F8       ; if 0x07
670D    C9 07       CMP #$07        ; .
670F    F0 03       BEQ $6714       ; skip ahead
6711    4C C6 65    JMP $65C6       ; otherwise jump to $65C6

;; ask all cards

6714    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
6717    A9 42       LDA #$42        ; text #42 "With the final choice" ...
6719    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
671C    20 29 64    JSR $6429       ; get a key press
671F    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
6722    A9 00       LDA #$00        ; song 0x00
6724    20 20 03    JSR $0320       ; .
6727    A9 43       LDA #$43        ; text #43 "There is a moment of intense wrenching" ...
6729    20 77 6A    JSR $6A77       ; display text (based on accumulator?)
672C    20 29 64    JSR $6429       ; get a key press
672F    A9 00       LDA #$00        ; song 0x00
6731    20 20 03    JSR $0320       ; .
6734    20 1B 08    JSR $081B       ; print to OS to load files

6737    84 C2 CC CF C1 C4 A0 CE 81 D0 D2 D4 AC C1 A4 B0 8D                      .BLOAD N.PRT,A$0.
6748    84 C2 CC CF C1 C4 A0 CE 81 CC D3 D4 AC C1 A4 C5 C5 B0 B0 8D             .BLOAD N.LST,A$EE00.
675C    84 C2 CC CF C1 C4 A0 CE 81 D2 D3 D4 AC C1 A4 C5 C3 B0 B0 8D 00          .BLOAD N.RST,A$EC00..

6771    A5 0B       LDA $0B         ; $EB = $0B
6773    85 E8       STA $E8         ; .
6775    A9 00       LDA #$00        ; $0B = 0x00
6777    85 0B       STA $0B         ; .
6779    AD F5 67    LDA $67F5       ; $D4 = $67F5 ($D4 == character number?)
677C    85 D4       STA $D4         ;
677E    E6 D4       INC $D4         ; $D4++
6780    20 2D 08    JSR $082D       ; set up $FE/$FF to point to the right character data vector based on $D4
6783    A5 FE       LDA $FE         ; $FC/FD = $FE/FF
6785    85 FC       STA $FC         ; .
6787    A5 FF       LDA $FF         ; .
6789    85 FD       STA $FD         ; .
678B    A9 01       LDA #$01        ; set $D4 to 0x01
678D    85 D4       STA $D4         ; .
678F    20 2D 08    JSR $082D       ; set up $FE/$FF to point to the right character data vector based on $D4

; loop over the attributes in the character data vector from 0x1F down to 0x00
; and swap the attribute of character 1 and the character set above

6792    A0 1F       LDY #$1F        ; .

6794    B1 FE       LDA ($FE),Y     ; load the 0x1F attribute of character 1
6796    48          PHA             ; and put it on stack
6797    B1 FC       LDA ($FC),Y     ; load the 0x1F attribute of character incremented to in $677E
6799    91 FE       STA ($FE),Y     ; store it as attribute of character 1
679B    68          PLA             ; get the old 0x1F attribute of character 1
679C    91 FC       STA ($FC),Y     ; and store it as the 0x1F attribute of the chractered incremented to in $677E
679E    88          DEY             ; decrease the character data vector offset
679F    10 F3       BPL $6794       ; if >= 0, loop back

;; copy new character's name into roster

; loop Y from 0x0F to 0x00 inclusive
; store $0300[Y] in character data attribute Y

67A1    A0 0F       LDY #$0F        ; .

67A3    B9 00 03    LDA $0300,Y     ; .
67A6    91 FE       STA ($FE),Y     ; .
67A8    88          DEY             ; .
67A9    10 F8       BPL $67A3       ; .

;; copy new character's gender into roster

67AB    A0 10       LDY #$10        ; .
67AD    A5 BC       LDA $BC         ; .
67AF    91 FE       STA ($FE),Y     ; .

;;

67B1    18          CLC             ; not sure why this is needed

67B2    A0 13       LDY #$13        ; store STR in character attribute 0x13
67B4    AD 09 68    LDA $6809       ; .
67B7    91 FE       STA ($FE),Y     ; .

67B9    A0 14       LDY #$14        ; store DEX in character attribute 0x14
67BB    AD 0A 68    LDA $680A       ; .
67BE    91 FE       STA ($FE),Y     ; .

67C0    A0 15       LDY #$15        ; store INT in character attribute 0x15
67C2    AD 0B 68    LDA $680B       ; .
67C5    91 FE       STA ($FE),Y     ; .

;; copy virtue scores

; loop X from 0x07 to 0x00 inclusive
; copy from $6824[X] to $ED00[X]

67C7    A2 07       LDX #$07        ; .

67C9    BD 24 68    LDA $6824,X     ; .
67CC    9D 00 ED    STA $ED00,X     ; .
67CF    CA          DEX             ; .
67D0    10 F7       BPL $67C9       ; .

;;

67D2    A9 01       LDA #$01        ; set party size to 1
67D4    85 0F       STA $0F         ; .
67D6    AE F5 67    LDX $67F5       ; load character class?
67D9    BD F9 67    LDA $67F9,X     ; could these possibly be the starting location depending on character class?
67DC    85 00       STA $00         ; .
67DE    BD 01 68    LDA $6801,X     ; .
67E1    85 01       STA $01         ; .
67E3    4C 09 64    JMP $6409       ; start game!

;;

; set in $65C0, $66C6, $66FC
; $682C randomly picks elements here until it gets a zero and returns offset of that zero
; $68E1 sets all these to zero if they are positive (have high-bit unset)

; virtue card scores?
; either 00, 01, or FF I think

67E6    00 00 00 00 00 00 00 00

;; this tells you the offset of the texts based on the first card
; the dilemma to show is this times the first card plus the second card

67EE    00 06 0B 0F 12 14 15

67F5    00                          ; might be character class; used as offset into $67F9[] and $6801[]

;; two virtues gypsy is to ask about

67F6    00
67F7    00

;; variable set various times (set to 0x00 initially)

; set to 0x00 in $6432 and $65B9
; incremented in $6707
; often compared to 0x07

; could be the number of card pairs shown

67F8    00

;; could these possibly be the starting location depending on character class?

; 00 honesty        E7  88      MAGE
; 01 compassion     53  69      BARD
; 02 valor          23  DD      FIGHTER
; 03 justice        3B  2C      DRUID
; 04 sacrifice      9E  15      TINKER
; 05 honor          69  B7      PALADIN
; 06 spirituality   17  81      RANGER
; 07 humility       BA  AB      SHEPHERD

67F9    E7 53 23 3B 9E 69 17 BA
6801    88 69 DD 2C 15 B7 81 AB

6809    0F      ; STR; incremented by $680C[virtue]; character attribute 0x13
680A    0F      ; DEX; incremented by $6814[virtue]; character attribute 0x14
680B    0F      ; INT; incremented by $681C[virtue]; character attribute 0x15

; how much to increment STR by per virtue

; valor increments by 3
; sacrifice, honor, and spirituality increment by 1

680C    00 00 03 00 01 01 01 00

; how much to increment DEX by per virtue

; compassion increments by 3
; justice, sacrifice, and spirtuality increment by 1

6814    00 03 00 01 01 00 01 00

; how much to increment INT by per virtue

; honesty increments by 3
; justice, honor, and spirituality increment by 1

681C    03 00 00 01 00 01 01 00

;; initial virtue scores

6824    50 50 50 50 50 50 50 50

;; randomly picks a virtue card score that is still zero

682C    20 4E 08    JSR $084E       ; generate random number
682F    29 07       AND #$07        ; between 0 and 7
6831    AA          TAX             ; .
6832    BD E6 67    LDA $67E6,X     ; loop until gets zero
6835    D0 F5       BNE $682C       ; .

6837    60          RTS             ;

;; make sound with frequency based on $FB

6838    A0 28       LDY #$28        ;
683A    2C 30 C0    BIT $C030       ; SPEAKER TOGGLE

; loop X from $FB to 0x00

683D    A6 FB       LDX $FB         ;

683F    CA          DEX             ;
6840    D0 FD       BNE $683F       ;

6842    2C 30 C0    BIT $C030       ; SPEAKER TOGGLE
6845    88          DEY             ;

6846    D0 F5       BNE $683D       ;

;; set $FE/$FF to hires base address of row in $FB

6848    A6 FB       LDX $FB         ;
684A    BD C0 E0    LDA $E0C0,X     ;
684D    85 FF       STA $FF         ;
684F    BD 00 E0    LDA $E000,X     ;
6852    85 FE       STA $FE         ;

6854    60          RTS             ;

;; portal appear?

6855    A9 00       LDA #$00        ; $FC/$FD = $6000
6857    85 FC       STA $FC         ; .
6859    A9 60       LDA #$60        ; .
685B    85 FD       STA $FD         ; .

; loop $FB from 0x2B to 0x46 (exclusive)

685D    A2 2B       LDX #$2B        ; $FB = 0x2B
685F    86 FB       STX $FB         ; .

6861    20 48 68    JSR $6848       ; set $FE/$FF to hires base address of row in $FB

; loop Y from 0x04 to 0x08

6864    A0 04       LDY #$04        ; Y = 0x04

6866    A2 00       LDX #$00        ; X = 0x00
6868    B1 FE       LDA ($FE),Y     ;
686A    81 FC       STA ($FC,X)     ;

686C    E6 FC       INC $FC         ; increment $FC/$FD with wrap
686E    D0 02       BNE $6872       ; .
6870    E6 FD       INC $FD         ; .

6872    C8          INY             ; increment Y too
6873    C0 08       CPY #$08        ;
6875    90 F1       BCC $6868       ;

;

6877    E6 FB       INC $FB         ; increment $FB
6879    A5 FB       LDA $FB         ; .
687B    C9 46       CMP #$46        ; .
687D    90 E2       BCC $6861       ; and loop back if < 0x46

687F    C6 FB       DEC $FB         ;

; loop $FB down to 0x2B

6881    20 38 68    JSR $6838       ; make sound based on $FB then set $FE/$FF to hires base address of row in $FB

6884    A0 04       LDY #$04        ;
6886    A9 D5       LDA #$D5        ; store 0xD5 in one hires byte 11010101
6888    91 FE       STA ($FE),Y     ;
688A    C8          INY             ;
688B    A9 AA       LDA #$AA        ; store 0xAA in the next byte  10101010
688D    91 FE       STA ($FE),Y     ;
688F    C8          INY             ;
6890    A9 D5       LDA #$D5        ; store 0xD5 in the next byte  11010101
6892    91 FE       STA ($FE),Y     ;
6894    C6 FB       DEC $FB         ;
6896    A5 FB       LDA $FB         ;
6898    C9 2B       CMP #$2B        ;
689A    B0 E5       BCS $6881       ;

689C    60          RTS             ;

;; portal disappear?

689D    A9 00       LDA #$00        ; $FC/$FD = $6000
689F    85 FC       STA $FC         ; .
68A1    A9 60       LDA #$60        ; .
68A3    85 FD       STA $FD         ; .

; loop $FB from 0x2B to 0x46 (exclusive)

68A5    A2 2B       LDX #$2B        ; $FB = 0x2B
68A7    86 FB       STX $FB         ; .

68A9    20 38 68    JSR $6838       ; make sound based on $FB then set $FE/$FF to hires base address of row in $FB


; loop Y from 0x04 to 0x08

68AC    A0 04       LDY #$04        ;
68AE    A2 00       LDX #$00        ;

68B0    A1 FC       LDA ($FC,X)     ; load $FC/FD
68B2    91 FE       STA ($FE),Y     ; store in $FE/FF + Y

68B4    E6 FC       INC $FC         ; increment $FC/FD with wrap
68B6    D0 02       BNE $68BA       ; .
68B8    E6 FD       INC $FD         ; .

68BA    C8          INY             ; Y++
68BB    C0 08       CPY #$08        ; .
68BD    90 F1       BCC $68B0       ; loop back if not 0x08 yet

68BF    E6 FB       INC $FB         ; $FB ++
68C1    A5 FB       LDA $FB         ; .
68C3    C9 46       CMP #$46        ; .
68C5    90 E2       BCC $68A9       ; loop back if not 0x46 yet

68C7    60          RTS             ;

;;

68C8    A2 4F       LDX #$4F        ;
68CA    BD 68 E0    LDA $E068,X     ;
68CD    85 FE       STA $FE         ;
68CF    BD 28 E1    LDA $E128,X     ;
68D2    85 FF       STA $FF         ;
68D4    A0 26       LDY #$26        ;
68D6    A9 00       LDA #$00        ;
68D8    91 FE       STA ($FE),Y     ;
68DA    88          DEY             ;
68DB    D0 FB       BNE $68D8       ;
68DD    CA          DEX             ;
68DE    10 EA       BPL $68CA       ;
68E0    60          RTS             ;

;; for all 8 elements in $67E6, set to zero if positive

; loop X from 0x07 to 0x00

68E1    A2 07       LDX #$07        ; read $67E6[7]

68E3    BD E6 67    LDA $67E6,X     ; .
68E6    30 05       BMI $68ED       ; if positive
68E8    A9 00       LDA #$00        ;   set to zero
68EA    9D E6 67    STA $67E6,X     ;   .
68ED    CA          DEX             ;
68EE    10 F3       BPL $68E3       ;

68F0    60          RTS             ;

;; clear rows 0x90 to 0xBF in hires and text row to 0x12

; loop x from 0xBF down to 0x90
;   put base memory address of hires row X in $FE/$FF
;   loop y from 0x27 to 0x00
;       set ($FE), Y to 0x00

68F1    A2 BF       LDX #$BF        ;

68F3    BD 00 E0    LDA $E000,X     ;
68F6    85 FE       STA $FE         ;
68F8    BD C0 E0    LDA $E0C0,X     ;
68FB    85 FF       STA $FF         ;
68FD    A9 00       LDA #$00        ;
68FF    A0 27       LDY #$27        ;

6901    91 FE       STA ($FE),Y     ;
6903    88          DEY             ;
6904    10 FB       BPL $6901       ;

6906    CA          DEX             ;
6907    E0 90       CPX #$90        ;
6909    B0 E8       BCS $68F3       ;

690B    A9 00       LDA #$00        ; set text column to 0x00
690D    85 CE       STA $CE         ; .
690F    A9 12       LDA #$12        ; set text row to 0x12
6911    85 CF       STA $CF         ; .

6913    60          RTS             ;

;; get the user to change the disk number to what's in A

6914    85 DE       STA $DE         ; store A in $DE (the desired disk)

6916    A5 DE       LDA $DE         ;
6918    C9 02       CMP #$02        ; if it's disk 2,
691A    F0 72       BEQ $698E       ;   or
691C    C9 04       CMP #$04        ; if it's disk 4,
691E    F0 6E       BEQ $698E       ;   jump to 698E

6920    A9 01       LDA #$01        ; set drive number ($DF)
6922    85 DF       STA $DF         ;   to 1
6924    A5 DE       LDA $DE         ; if desired disk
6926    C5 D2       CMP $D2         ;   is in drive 1
6928    F0 61       BEQ $698B       ;       we're done
692A    AD F8 67    LDA $67F8       ; if $67F8 (???)
692D    C9 07       CMP #$07        ;
692F    D0 06       BNE $6937       ;
6931    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
6934    4C 3A 69    JMP $693A       ;

6937    20 C8 68    JSR $68C8       ;
693A    A4 CF       LDY $CF         ; .
693C    A2 0B       LDX #$0B        ; .
693E    20 1E 08    JSR $081E       ; print the following text at column 0x0B of current text row

6941    D0 CC C5 C1 D3 C5 A0 D0 CC C1 C3 C5 A0 D4 C8 C5 00      PLEASE PLACE THE

6952    E6 CF       INC $CF         ; next text row
6954    20 0F 6A    JSR $6A0F       ; print disk name based on desired disk in $DE
6957    A4 CF       LDY $CF         ; .
6959    A2 0D       LDX #$0D        ; .
695B    20 1E 08    JSR $081E       ; print the following text at column 0x0D of current text row

695E    C9 CE D4 CF A0 C4 D2 C9 D6 C5 A0 B1 00      INTO DRIVE 1

696B    E6 CF       INC $CF         ; next text row
696D    A4 CF       LDY $CF         ; .
696F    A2 0B       LDX #$0B        ; .
6971    20 1E 08    JSR $081E       ; print the following text at column 0x0B of current text row

6974    C1 CE C4 A0 D0 D2 C5 D3 D3 A0 DB C5 D3 C3 DD 00     AND PRESS [ESC]

6984    20 00 08    JSR $0800       ; get a key press
6987    C9 9B       CMP #$9B        ; if not ESC
6989    D0 F9       BNE $6984       ; loop back
698B    4C E2 69    JMP $69E2       ; but if ESC jump

;; disk 2/4 desired?

698E    A5 D1       LDA $D1         ; get number of drives (?)
6990    C9 02       CMP #$02        ; .
6992    90 8C       BCC $6920       ; if < 2 go back to $6920 and continue as if one drive
6994    A9 02       LDA #$02        ; .
6996    85 DF       STA $DF         ; change the current drive number to 2
6998    A5 DE       LDA $DE         ; get the desired disk number
699A    C5 D3       CMP $D3         ; compare to the disk number recorded for drive 2
699C    F0 44       BEQ $69E2       ; if equal, jump to $69E2 below
699E    AD F8 67    LDA $67F8       ; otherwise,
69A1    C9 07       CMP #$07        ; if $67F8 == 0x07
69A3    D0 06       BNE $69AB       ; .
69A5    20 F1 68    JSR $68F1       ; clear rows 0x90 to 0xBF in hires and text row to 0x12
69A8    4C AE 69    JMP $69AE       ;

69AB    20 C8 68    JSR $68C8       ; otherwise
69AE    A4 CF       LDY $CF         ;   print the following at 0x0B, $CF
69B0    A2 0B       LDX #$0B        ;   .
69B2    20 1E 08    JSR $081E       ;   .

69B5    D0 CC C5 C1 D3 C5 A0 D0 CC C1 C3 C5 A0 D4 C8 C5 00                      PLEASE PLACE THE

69C6    E6 CF       INC $CF         ; next line
69C8    20 0F 6A    JSR $6A0F       ; print the name of the disk based on the desired disk number in $DE
69CB    A4 CF       LDY $CF         ; print the following at 0x0D, $CF
69CD    A2 0D       LDX #$0D        ; .
69CF    20 1E 08    JSR $081E       ; .

69D2    C9 CE D4 CF A0 C4 D2 C9 D6 C5 A0 B2 00                                  INTO DRIVE 2

69DF    4C 6B 69    JMP $696B       ; jump back to finish instructions

;;

69E2    A5 DF       LDA $DF         ; get drive number
69E4    18          CLC             ; .
69E5    69 B0       ADC #$B0        ; convert it to character code for displaying that number
69E7    8D FA 69    STA $69FA       ; fill it out in the BLOAD below
69EA    20 1B 08    JSR $081B       ; print to OS to load DISK file

69ED    84 C2 CC CF C1 C4 A0 C4 C9 D3 CB AC C4 B1 8D 00       ^DBLOAD DISK,D_.

69FD    A6 DF       LDX $DF         ; the drive number
69FF    A5 D0       LDA $D0         ; the disk number just loaded
6A01    95 D1       STA $D1,X       ; store in the array of which disk is in which drive
6A03    C5 DE       CMP $DE         ; is it the desired disk?
6A05    F0 07       BEQ $6A0E       ; if not,
6A07    C6 CF       DEC $CF         ;   go up two text lines
6A09    C6 CF       DEC $CF         ;   .
6A0B    4C 16 69    JMP $6916       ;   jump back to request again

6A0E    60          RTS             ;

;; print the name of the disk based on the desired disk number in $DE

6A0F    A6 DE       LDX $DE         ; get desired disk number
6A11    CA          DEX             ; .
6A12    D0 16       BNE $6A2A       ; if it was 1 (so X now 0)...
6A14    A4 CF       LDY $CF         ; Y = $CF (text row)
6A16    A2 0D       LDX #$0D        ; X = 0x0D
6A18    20 1E 08    JSR $081E       ; print the following at X,Y

6A1B    D5 CC D4 C9 CD C1 A0 C4 C9 D3 CB 00     ULTIMA DISK

6A27    E6 CF       INC $CF         ; increment text row
6A29    60          RTS             ;

6A2A    CA          DEX             ; .
6A2B    D0 19       BNE $6A46       ; if it was 2
6A2D    A4 CF       LDY $CF         ; Y = $CF (text row)
6A2F    A2 0C       LDX #$0C        ; X = 0x0D
6A31    20 1E 08    JSR $081E       ; print the following at X,Y

6A34    C2 D2 C9 D4 C1 CE CE C9 C1 A0 C4 C9 D3 CB 00        BRITANNIA DISK

6A43    E6 CF       INC $CF         ; increment text row
6A45    60          RTS             ;

6A46    CA          DEX             ; .
6A47    D0 14       BNE $6A5D       ; if it was 3
6A49    A4 CF       LDY $CF         ; Y = $CF (text row)
6A4B    A2 0E       LDX #$0E        ; X = 0x0D
6A4D    20 1E 08    JSR $081E       ; print the following at X,Y

6A50    D4 CF D7 CE A0 C4 C9 D3 CB 00       TOWN DISK

6A5A    E6 CF       INC $CF         ; increment text row
6A5C    60          RTS             ;

6A5D    CA          DEX             ; .
6A5E    D0 16       BNE $6A76       ; if it was 4
6A60    A4 CF       LDY $CF         ; Y = $CF (text row)
6A62    A2 0D       LDX #$0D        ; X = 0x0D
6A64    20 1E 08    JSR $081E       ; print the following at X,Y

6A67    C4 D5 CE C7 C5 CF CE A0 C4 C9 D3 CB 00      DUNGEON DISK

6A74    E6 CF       INC $CF         ; increment text row
6A76    60          RTS             ;


;; display Ath text

;; text ends with 0x00, line breaks

6A77    A8          TAY             ; put A in Y
6A78    A9 B3       LDA #$B3        ; put $6CB3 in $FE/$FF
6A7A    85 FE       STA $FE         ; .
6A7C    A9 6C       LDA #$6C        ; .
6A7E    85 FF       STA $FF         ; .

6A80    A2 00       LDX #$00        ; load value at ($FE)

; loop

6A82    A1 FE       LDA ($FE,X)     ; .
6A84    F0 06       BEQ $6A8C       ; if non-zero
6A86    20 AF 6A    JSR $6AAF       ;   increment $FE/$FF with wrap
6A89    4C 82 6A    JMP $6A82       ;   loop back

6A8C    88          DEY             ; otherwise, decrement Y
6A8D    F0 03       BEQ $6A92       ; if non-zero
6A8F    4C 86 6A    JMP $6A86       ; loop back

;; okay, we've found our Ath text

6A92    20 AF 6A    JSR $6AAF       ; otherwise, increment $FE/$FF with wrap
6A95    A2 00       LDX #$00        ; load value at ($FE)
6A97    A1 FE       LDA ($FE,X)     ; .
6A99    F0 13       BEQ $6AAE       ; if zero, return
6A9B    C9 FF       CMP #$FF        ; elif not 0xFF
6A9D    D0 09       BNE $6AA8       ; go to $6AA8
6A9F    E6 CF       INC $CF         ; otherwise (if 0xFF), go to next text row
6AA1    A9 00       LDA #$00        ; carriage return text column
6AA3    85 CE       STA $CE         ; .
6AA5    4C 92 6A    JMP $6A92       ; loop back

6AA8    20 24 08    JSR $0824       ; display character in game font
6AAB    4C 92 6A    JMP $6A92       ; loop back

6AAE    60          RTS             ;

;; increment $FE/$FF with wrap

6AAF    E6 FE       INC $FE         ;
6AB1    D0 02       BNE $6AB5       ;
6AB3    E6 FF       INC $FF         ;
6AB5    60          RTS             ;

;; draw bead on "abacus"

; A = side (0 = left; 1 = right)
; X = virtue
; Y = number of pairs shown

6AB6    48          PHA             ; push A onto the stack

6AB7    B9 EF 6A    LDA $6AEF,Y     ;
6ABA    85 C1       STA $C1         ;
6ABC    BD F6 6A    LDA $6AF6,X     ;
6ABF    85 C0       STA $C0         ;
6AC1    4A          LSR A           ;

6AC2    68          PLA             ; get saved A
6AC3    2A          ROL A           ; rotate left
6AC4    0A          ASL A           ; shift left

6AC5    AA          TAX             ; tranfer to X
6AC6    BD FE 6A    LDA $6AFE,X     ; put in $6AE3/$6AE4 the value references in $6AFE/$6AFF[X]
6AC9    8D E3 6A    STA $6AE3       ;
6ACC    BD FF 6A    LDA $6AFF,X     ;
6ACF    8D E4 6A    STA $6AE4       ;

; loop X from 0x00 to 0E

6AD2    A2 00       LDX #$00        ;
6AD4    A4 C1       LDY $C1         ; Y = $C1
6AD6    B9 00 E0    LDA $E000,Y     ; store base address of hires row Y ($C1)
6AD9    85 C2       STA $C2         ;   in $C2/C$3
6ADB    B9 C0 E0    LDA $E0C0,Y     ; .
6ADE    85 C3       STA $C3         ; .
6AE0    A4 C0       LDY $C0         ; Y = $C0 (hires byte column)
6AE2    BD FF FF    LDA $____,X     ; this address is filed in at 6AC6-6ACF based on data that 6AFE
6AE5    91 C2       STA ($C2),Y     ; write hires byte
6AE7    E6 C1       INC $C1         ; increment hires row
6AE9    E8          INX             ;
6AEA    E0 0E       CPX #$0E        ; if < 0x0E
6AEC    90 E6       BCC $6AD4       ;   loop back

6AEE    60          RTS             ;

;;

; $C1 set to one of these

6AEF    11 21 31 41 51 61 71

; $C0 set to one of these

6AF6    10 11 12 13 14 15 16 17

; addresses to populate $6AE2 with

6AFE    06 6B
6B00    14 6B
6B02    22 6B
6B04    30 6B

6B06    3A 7E 7E 7E 7E 7E 7F 7F 7E 7E 7E 7E 7E 3A 5D
6B15    7F 7F 7F 7F 7F 7F 7F 7F 7F 7F 7F
6B20    7F 5D 22 00 00
6B25    00
6B26    00
6B27    00
6B28    00
6B29    00
6B2A    00
6B2B    00
6B2C    00
6B2D    00
6B2E    00
6B2F    22
6B30    45
6B31    01
6B32    01
6B33    01
6B34    01
6B35    01
6B36    01
6B37    01
6B38    01
6B39    01
6B3A    01
6B3B    01
6B3C    01
6B3D    45

;; draw card
;; A = card to draw
;; X = position (0 = left; 1 = right)

6B3E    48          PHA             ;
6B3F    A9 10       LDA #$10        ; $C0/C1 = 1002 for left; 101C for right
6B41    85 C1       STA $C1         ; .
6B43    BD 84 6B    LDA $6B84,X     ; .
6B46    85 C0       STA $C0         ; .
6B48    68          PLA             ;
6B49    0A          ASL A           ;
6B4A    AA          TAX             ;
6B4B    BD 86 6B    LDA $6B86,X     ;
6B4E    8D 6C 6B    STA $6B6C       ;
6B51    BD 87 6B    LDA $6B87,X     ;
6B54    8D 6D 6B    STA $6B6D       ;

6B57    A2 00       LDX #$00        ;
6B59    A4 C1       LDY $C1         ;
6B5B    B9 00 E0    LDA $E000,Y     ;
6B5E    85 C2       STA $C2         ;
6B60    B9 C0 E0    LDA $E0C0,Y     ;
6B63    85 C3       STA $C3         ;
6B65    A9 0A       LDA #$0A        ;
6B67    85 C4       STA $C4         ;
6B69    A4 C0       LDY $C0         ;
6B6B    BD FF FF    LDA $____,X     ;
6B6E    91 C2       STA ($C2),Y     ;
6B70    E8          INX             ;
6B71    D0 03       BNE $6B76       ;
6B73    EE 6D 6B    INC $6B6D       ;
6B76    C8          INY             ;
6B77    C6 C4       DEC $C4         ;
6B79    D0 F0       BNE $6B6B       ;
6B7B    E6 C1       INC $C1         ;
6B7D    A5 C1       LDA $C1         ;
6B7F    C9 80       CMP #$80        ;
6B81    D0 D6       BNE $6B59       ;

6B83    60          RTS             ;

;; DATA

;; card display offsets?

6B84    02 1C

;; address of card images?

; 00 honesty        4000    MAGE
; 01 compassion     4460    BARD
; 02 valor          48C0    FIGHTER
; 03 justice        4D20    DRUID
; 04 sacrifice      5180    TINKER
; 05 honor          55E0    PALADIN
; 06 spirituality   5A40    RANGER
; 07 humility       5EA0    SHEPHERD

6B86    00 40 60 44 C0 48 20 4D 80 51 E0 55 40 5A A0 5E

;; SUPER-PACKER

6B96    D3 D5 D0 C5 D2 AD D0 C1 C3 CB C5 D2                                     SUPER-PACKER
6BA2    A0 C3 CF D0 D9 D2 C9 C7 C8 D4 A0 B1 B9 B8 B3                             COPYRIGHT 1983
6BB1    A0 C2 D9 A0 D3 D4 C5 D6 C5 CE A0 CD C5 D5 D3 C5                          BY STEVEN MEUSE

6BC1    A9 40       LDA #$40        ; $FC/$FD = $4000
6BC3    85 FD       STA $FD         ; .
6BC5    A2 00       LDX #$00        ; .
6BC7    86 B8       STX $B8         ; $B8 = 0x00
6BC9    86 BA       STX $BA         ; $BA = 0x00
6BCB    86 FC       STX $FC         ; .
6BCD    86 BB       STX $BB         ; $BB = 0x00
6BCF    A1 FC       LDA ($FC,X)     ; load two bytes from $4000 and put in $B4/$B5
6BD1    85 B4       STA $B4         ; .
6BD3    E6 FC       INC $FC         ; .
6BD5    A1 FC       LDA ($FC,X)     ; .
6BD7    85 B5       STA $B5         ; .
6BD9    E6 FC       INC $FC         ; increment to next byte in $4000
6BDB    A0 27       LDY #$27        ; Y = 0x27

; loop Y down to 00 (inclusive)

6BDD    A2 8F       LDX #$8F        ; X = 0x8F
6BDF    86 B6       STX $B6         ; $B6 = 0x8F

; loop X down to 00 (inclusive)

6BE1    BD 00 E0    LDA $E000,X     ; get hires memory base for row X
6BE4    85 FE       STA $FE         ;
6BE6    BD C0 E0    LDA $E0C0,X     ;
6BE9    85 FF       STA $FF         ;

6BEB    20 FC 6B    JSR $6BFC       ;

6BEE    C6 B6       DEC $B6         ; decrement graphics row
6BF0    A6 B6       LDX $B6         ;
6BF2    E0 FF       CPX #$FF        ;
6BF4    D0 EB       BNE $6BE1       ;
6BF6    88          DEY             ;
6BF7    C0 FF       CPY #$FF        ;
6BF9    D0 E2       BNE $6BDD       ;
6BFB    60          RTS             ;

;;

6BFC    A2 00       LDX #$00        ;
6BFE    24 B8       BIT $B8         ;
6C00    30 65       BMI $6C67       ;
6C02    24 BA       BIT $BA         ;
6C04    30 77       BMI $6C7D       ;
6C06    A1 FC       LDA ($FC,X)     ;
6C08    85 B2       STA $B2         ;
6C0A    C5 B4       CMP $B4         ;
6C0C    D0 20       BNE $6C2E       ;
6C0E    E6 FC       INC $FC         ;
6C10    D0 02       BNE $6C14       ;
6C12    E6 FD       INC $FD         ;
6C14    A1 FC       LDA ($FC,X)     ;
6C16    85 B7       STA $B7         ;
6C18    E6 FC       INC $FC         ;
6C1A    D0 02       BNE $6C1E       ;
6C1C    E6 FD       INC $FD         ;
6C1E    A1 FC       LDA ($FC,X)     ;
6C20    85 B2       STA $B2         ;
6C22    E6 FC       INC $FC         ;
6C24    D0 02       BNE $6C28       ;
6C26    E6 FD       INC $FD         ;
6C28    A9 80       LDA #$80        ;
6C2A    85 B8       STA $B8         ;
6C2C    D0 39       BNE $6C67       ;
6C2E    C5 B5       CMP $B5         ;
6C30    D0 2A       BNE $6C5C       ;
6C32    E6 FC       INC $FC         ;
6C34    D0 02       BNE $6C38       ;
6C36    E6 FD       INC $FD         ;
6C38    A1 FC       LDA ($FC,X)     ;
6C3A    85 B3       STA $B3         ;
6C3C    85 BB       STA $BB         ;
6C3E    E6 FC       INC $FC         ;
6C40    D0 02       BNE $6C44       ;
6C42    E6 FD       INC $FD         ;
6C44    A1 FC       LDA ($FC,X)     ;
6C46    85 B9       STA $B9         ;
6C48    E6 FC       INC $FC         ;
6C4A    D0 02       BNE $6C4E       ;
6C4C    E6 FD       INC $FD         ;
6C4E    A5 FC       LDA $FC         ;
6C50    85 B0       STA $B0         ;
6C52    A5 FD       LDA $FD         ;
6C54    85 B1       STA $B1         ;
6C56    A9 80       LDA #$80        ;
6C58    85 BA       STA $BA         ;
6C5A    D0 21       BNE $6C7D       ;
6C5C    A5 B2       LDA $B2         ;
6C5E    91 FE       STA ($FE),Y     ;
6C60    E6 FC       INC $FC         ;
6C62    D0 02       BNE $6C66       ;
6C64    E6 FD       INC $FD         ;
6C66    60          RTS             ;

;;

6C67    A5 B2       LDA $B2         ;
6C69    91 FE       STA ($FE),Y     ;
6C6B    C6 B7       DEC $B7         ;
6C6D    D0 F7       BNE $6C66       ;
6C6F    A9 00       LDA #$00        ;
6C71    85 B8       STA $B8         ;
6C73    24 BA       BIT $BA         ;
6C75    10 EF       BPL $6C66       ;
6C77    C6 BB       DEC $BB         ;
6C79    C6 BB       DEC $BB         ;
6C7B    D0 1C       BNE $6C99       ;

6C7D    A1 FC       LDA ($FC,X)     ;
6C7F    C5 B4       CMP $B4         ;
6C81    D0 0E       BNE $6C91       ;
6C83    A5 BB       LDA $BB         ;
6C85    D0 87       BNE $6C0E       ;
6C87    A5 B0       LDA $B0         ;
6C89    85 FC       STA $FC         ;
6C8B    A5 B1       LDA $B1         ;
6C8D    85 FD       STA $FD         ;
6C8F    D0 F4       BNE $6C85       ;
6C91    91 FE       STA ($FE),Y     ;
6C93    E6 FC       INC $FC         ;
6C95    D0 02       BNE $6C99       ;
6C97    E6 FD       INC $FD         ;
6C99    C6 BB       DEC $BB         ;
6C9B    D0 C9       BNE $6C66       ;
6C9D    A5 B3       LDA $B3         ;
6C9F    85 BB       STA $BB         ;
6CA1    C6 B9       DEC $B9         ;
6CA3    F0 09       BEQ $6CAE       ;
6CA5    A5 B0       LDA $B0         ;
6CA7    85 FC       STA $FC         ;
6CA9    A5 B1       LDA $B1         ;
6CAB    85 FD       STA $FD         ;
6CAD    60          RTS             ;

;;

6CAE    A9 00       LDA #$00        ;
6CB0    85 BA       STA $BA         ;
6CB2    60          RTS             ;

;;

DATA

6CB3    00

;; these are the virtue codes

; 00 honesty        MAGE
; 01 compassion     BARD
; 02 valor          FIGHTER
; 03 justice        DRUID
; 04 sacrifice      TINKER
; 05 honor          PALADIN
; 06 spirituality   RANGER
; 07 humility       SHEPHERD

; the texts go honesty+compassion, honesty+valor, honesty+justice, and so on
; then compassion+valor, compassion+justice, etc
; all the way to spirtuality+humility.

;; text 0x01

;; Honesty vs Compassion

6CB4

C5 EE F4 F2 F5 F3 F4 E5 E4 A0 F4 EF A0 E4 E5 EC E9 F6 E5 F2 A0 E1 EE A0 F5 EE E3 EF F5 EE F4 E5 E4 A0 F0 F5 F2 F3 E5 FF
EF E6 A0 E7 EF EC E4 AC A0 F4 E8 EF F5 A0 E4 EF F3 F4 A0 ED E5 E5 F4 A0 E1 A0 F0 EF EF F2 A0 E2 E5 E7 E7 E1 F2 AE FF
C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 E4 E5 EC E9 F6 E5 F2 A0 F4 E8 E5 A0 E7 EF EC E4 A0 EB EE EF F7 E9 EE E7 FF
F4 E8 E5 A0 D4 F2 F5 F3 F4 A0 E9 EE A0 F4 E8 E5 E5 A0 F7 E1 F3 A0 F7 E5 EC EC AD F0 EC E1 E3 E5 E4 BB A0 EF F2 FF
C2 A9 A0 F3 E8 EF F7 A0 C3 EF ED F0 E1 F3 F3 E9 EF EE AC A0 E7 E9 F6 E9 EE E7 A0 F4 E8 E5 A0 C2 E5 E7 E7 E1 F2 A0 E1 FF
E3 EF E9 EE AC A0 EB EE EF F7 E9 EE E7 A0 E9 F4 A0 F7 EF EE A7 F4 A0 E2 E5 A0 ED E9 F3 F3 E5 E4 BF 00

Entrusted to deliver an uncounted purse\n
of gold, thou dost meet a poor beggar.\n
Dost thou A) deliver the gold knowing\n
the Trust in thee was well-placed; or\n
B) show Compassion, giving the Beggar a\n
coin, knowing it won't be missed?

;; text 0x02

;; Honesty vs Valor

6D99

D4 E8 EF F5 A0 E8 E1 F3 A0 E2 E5 E5 EE A0 F0 F2 EF E8 E9 E2 E9 F4 E5 E4 A0 E2 F9 A0 F4 E8 F9 A0 E1 E2 F3 E5 EE F4 FF
CC EF F2 E4 A0 E6 F2 EF ED A0 EA EF E9 EE E9 EE E7 A0 F4 E8 F9 A0 E6 F2 E9 E5 EE E4 F3 A0 E9 EE A0 E1 A0 E3 EC EF F3 E5 FF
F0 E9 F4 E3 E8 E5 E4 A0 E2 E1 F4 F4 EC E5 AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 F2 E5 E6 F2 E1 E9 EE AC A0 F3 EF FF
F4 E8 EF F5 A0 ED E1 F9 A0 C8 EF EE E5 F3 F4 EC F9 A0 E3 EC E1 E9 ED A0 EF E2 E5 E4 E9 E5 EE E3 E5 BB A0 EF F2 FF
C2 A9 A0 F3 E8 EF F7 A0 D6 E1 EC EF F2 AC A0 E1 EE E4 A0 E1 E9 E4 A0 F4 E8 F9 A0 E3 EF ED F2 E1 E4 E5 F3 AC FF
EB EE EF F7 E9 EE E7 A0 F4 E8 EF F5 A0 ED E1 F9 A0 E4 E5 EE F9 A0 E9 F4 A0 EC E1 F4 E5 F2 BF 00

Thou has been prohibited by thy absent
Lord from joining thy friends in a close
pitched battle. Dost thou A) refrain, so
thou may Honestly claim obedience; or
B) show Valor, and aid thy comrades,
knowing thou may deny it later?

;; text 0x03

;; Honesty vs Justice

C1 A0 ED E5 F2 E3 E8 E1 EE F4 A0 EF F7 E5 F3 A0 F4 E8 F9 A0 E6 F2 E9 E5 EE E4 A0 ED EF EE E5 F9 AC A0 EE EF F7 FF
EC EF EE E7 A0 F0 E1 F3 F4 A0 E4 F5 E5 AE A0 D4 E8 EF F5 A0 E4 EF F3 F4 A0 F3 E5 E5 A0 F4 E8 E5 A0 F3 E1 ED E5 FF
ED E5 F2 E3 E8 E1 EE F4 A0 E4 F2 EF F0 A0 E1 A0 F0 F5 F2 F3 E5 A0 EF E6 A0 E7 EF EC E4 AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 FF
C1 A9 A0 C8 EF EE E5 F3 F4 EC F9 A0 F2 E5 F4 F5 F2 EE A0 F4 E8 E5 A0 F0 F5 F2 F3 E5 A0 E9 EE F4 E1 E3 F4 BB A0 EF F2 FF
C2 A9 A0 CA F5 F3 F4 EC F9 A0 E7 E9 F6 E5 A0 F4 E8 F9 A0 E6 F2 E9 E5 EE E4 A0 E1 A0 F0 EF F2 F4 E9 EF EE A0 EF E6 FF
F4 E8 E5 A0 E7 EF EC E4 A0 E6 E9 F2 F3 F4 BF 00

A merchant owes thy friend money, now
long past due. Thou dost see the same
merchant drop a purse of gold. Dost thou
A) Honestly return the purse intact; or
B) Justly give thy friend a portion of
the gold first?.

;; text 0x04

;; Honesty vs Sacrifice

D4 E8 E5 E5 A0 E1 EE E4 A0 F4 E8                Thee and th
F9 A0 E6 F2 E9 E5 EE E4 A0 E1 F2 E5 A0 F6 E1 EC y friend are val
E9 E1 EE F4 A0 E2 F5 F4 FF F0 E5 EE EE E9 EC E5 iant but.pennile
F3 F3 A0 F7 E1 F2 F2 E9 EF F2 F3 AE A0 D4 E8 EF ss warriors. Tho
F5 A0 E2 EF F4 E8 A0 E7 EF A0 EF F5 F4 A0 F4 EF u both go out to
FF F3 EC E1 F9 A0 E1 A0 ED E9 E7 E8 F4 F9 A0 E4 .slay a mighty d
F2 E1 E7 EF EE AE A0 D4 E8 F9 A0 E6 F2 E9 E5 EE ragon. Thy frien
E4 A0 F4 E8 E9 EE EB F3 FF E8 E5 A0 F3 EC E5 F7 d thinks.he slew
A0 E9 F4 AC A0 F4 E8 E5 E5 A0 E4 E9 E4 AE A0 D7  it, thee did. W
E8 E5 EE A0 E1 F3 EB E5 E4 AC A0 E4 EF F3 F4 FF hen asked, dost.
F4 E8 EF F5 A0 C1 A9 A0 D4 F2 F5 F4 E8 E6 F5 EC thou A) Truthful
EC F9 A0 E3 EC E1 E9 ED A0 F4 E8 E5 A0 E7 EF EC ly claim the gol
E4 BB A0 EF F2 FF C2 A9 A0 C1 EC EC EF F7 A0 F4 d; or.B) Allow t
E8 F9 A0 E6 F2 E9 E5 EE E4 A0 F4 E8 E5 A0 EC E1 hy friend the la
F2 E7 E5 A0 F2 E5 F7 E1 F2 E4 BF 00             rge reward?.

;; text 0x05

;; Honesty vs Honor

D4 E8 EF F5                                     Thou
A0 E1 F2 F4 A0 F3 F7 EF F2 EE A0 F4 EF A0 F0 F2  art sworn to pr
EF F4 E5 E3 F4 A0 F4 E8 F9 A0 CC EF F2 E4 A0 E1 otect thy Lord a
F4 FF E1 EE F9 A0 E3 EF F3 F4 AC A0 F9 E5 F4 A0 t.any cost, yet
F4 E8 EF F5 A0 EB EE EF F7 E5 F3 F4 A0 E8 E5 A0 thou knowest he
E8 E1 F4 E8 FF E3 EF ED ED E9 F4 F4 E5 E4 A0 E1 hath.committed a
A0 E3 F2 E9 ED E5 AE A0 C1 F5 F4 E8 EF F2 E9 F4  crime. Authorit
E9 E5 F3 A0 E1 F3 EB A0 F4 E8 E5 E5 FF EF E6 A0 ies ask thee.of
F4 E8 E5 A0 E1 E6 E6 E1 E9 F2 AC A0 E4 EF F3 F4 the affair, dost
A0 F4 E8 EF F5 A0 C1 A9 A0 E2 F2 E5 E1 EB A0 F4  thou A) break t
E8 E9 EE E5 FF EF E1 F4 E8 A0 E2 F9 A0 C8 EF EE hine.oath by Hon
E5 F3 F4 EC F9 A0 F3 F0 E5 E1 EB E9 EE E7 BB A0 estly speaking;
EF F2 A0 C2 A9 A0 F5 F0 E8 EF EC E4 FF C8 EF EE or B) uphold.Hon
EF F2 A0 E2 F9 A0 F3 E9 EC E5 EE F4 EC F9 A0 EB or by silently k
E5 E5 F0 E9 EE E7 A0 F4 E8 E9 EE E5 A0 EF E1 F4 eeping thine oat
E8 BF 00                                        h?.

;; text 0x06

;; Honesty vs Spirituality

D4 E8 F9 A0 E6 F2 E9 E5 EE E4 A0 F3 E5          Thy friend se
E5 EB F3 A0 E1 E4 ED E9 F4 F4 E1 EE E3 E5 A0 F4 eks admittance t
EF A0 F4 E8 F9 FF D3 F0 E9 F2 E9 F4 F5 E1 EC A0 o thy.Spiritual
EF F2 E4 E5 F2 AE A0 D4 E8 EF F5 A0 E1 F2 F4 A0 order. Thou art
E1 F3 EB E5 E4 A0 F4 EF A0 F6 EF F5 E3 E8 FF E6 asked to vouch.f
EF F2 A0 E8 E9 F3 A0 F0 F5 F2 E9 F4 F9 A0 EF E6 or his purity of
A0 D3 F0 E9 F2 E9 F4 AC A0 EF E6 A0 F7 E8 E9 E3  Spirit, of whic
E8 A0 F4 E8 EF F5 FF E1 F2 F4 A0 F5 EE F3 F5 F2 h thou.art unsur
E5 AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 e. Dost thou A)
C8 EF EE E5 F3 F4 EC F9 FF E5 F8 F0 F2 E5 F3 F3 Honestly.express
A0 F4 E8 F9 A0 E4 EF F5 E2 F4 BB A0 EF F2 A0 C2  thy doubt; or B
A9 A0 D6 EF F5 E3 E8 A0 E6 EF F2 A0 E8 E9 ED AC ) Vouch for him,
FF E8 EF F0 E9 EE E7 A0 E6 EF F2 A0 E8 E9 F3 A0 .hoping for his
D3 F0 E9 F2 E9 F4 F5 E1 EC A0 E9 ED F0 F2 EF F6 Spiritual improv
E5 ED E5 EE F4 BF 00                            ement?.

;; text 0x07

;; Honesty vs Humility

D4 E8 F9 A0 CC EF F2 E4 A0                      Thy Lord
ED E9 F3 F4 E1 EB E5 EE EC F9 A0 E2 E5 EC E9 E5 mistakenly belie
F6 E5 F3 A0 E8 E5 A0 F3 EC E5 F7 A0 E1 FF E4 F2 ves he slew a.dr
E1 E7 EF EE AE A0 D4 E8 EF F5 A0 E8 E1 F3 F4 A0 agon. Thou hast
F0 F2 EF EF E6 A0 F4 E8 E1 F4 A0 F4 E8 F9 A0 EC proof that thy l
E1 EE E3 E5 FF E6 E5 EC EC E5 E4 A0 F4 E8 E5 A0 ance.felled the
E2 E5 E1 F3 F4 AE A0 A0 D7 E8 E5 EE A0 E1 F3 EB beast.  When ask
E5 E4 AC A0 E4 EF F3 F4 A0 F4 E8 EF F5 FF C1 A9 ed, dost thou.A)
A0 C8 EF EE E5 F3 F4 EC F9 A0 E3 EC E1 E9 ED A0  Honestly claim
F4 E8 E5 A0 EB E9 EC EC A0 E1 EE E4 A0 F4 E8 E5 the kill and the
FF F0 F2 E9 FA E5 BB A0 EF F2 A0 C2 A9 A0 C8 F5 .prize; or B) Hu
ED E2 EC F9 A0 F0 E5 F2 ED E9 F4 A0 F4 E8 F9 A0 mbly permit thy
CC EF F2 E4 A0 E8 E9 F3 FF E2 E5 EC E9 E5 E6 BF Lord his.belief?
00

;; text 0x08

;; Compassion vs Valor

D4 E8 EF F5 A0 E4 EF F3 F4 A0 ED E1 EE E1 E7    Thou dost manag
E5 A0 F4 EF A0 E4 E9 F3 E1 F2 ED A0 F4 E8 F9 A0 e to disarm thy
ED EF F2 F4 E1 EC FF E5 EE E5 ED F9 A0 E9 EE A0 mortal.enemy in
E1 A0 E4 F5 E5 EC AE A0 C8 E5 A0 E9 F3 A0 E1 F4 a duel. He is at
A0 F4 E8 F9 A0 ED E5 F2 E3 F9 AE FF C4 EF F3 F4  thy mercy..Dost
A0 F4 E8 EF F5 A0 C1 A9 A0 F3 E8 EF F7 A0 C3 EF  thou A) show Co
ED F0 E1 F3 F3 E9 EF EE A0 E2 F9 FF F0 E5 F2 ED mpassion by.perm
E9 F4 F4 E9 EE E7 A0 E8 E9 ED A0 F4 EF A0 F9 E9 itting him to yi
E5 EC E4 BB A0 EF F2 A0 C2 A9 A0 F3 EC E1 F9 A0 eld; or B) slay
E8 E9 ED FF E1 F3 A0 E5 F8 F0 E5 E3 F4 E5 E4 A0 him.as expected
EF E6 A0 E1 A0 D6 E1 EC E9 E1 EE F4 A0 E4 F5 E5 of a Valiant due
EC E9 F3 F4 BF 00                               list?

;; text 0x09

;; Compassion vs Justice

C1 E6 F4 E5 F2 A0 B2 B0 A0 F9                   After 20 y
E5 E1 F2 F3 A0 F4 E8 EF F5 A0 E8 E1 F3 F4 A0 E6 ears thou hast f
EF F5 EE E4 A0 F4 E8 E5 FF F3 EC E1 F9 E5 F2 A0 ound the.slayer
EF E6 A0 F4 E8 F9 A0 E2 E5 F3 F4 A0 E6 F2 E9 E5 of thy best frie
EE E4 F3 AE A0 D4 E8 E5 A0 F6 E9 EC EC E1 E9 EE nds. The villain
FF F0 F2 EF F6 E5 F3 A0 F4 EF A0 E2 E5 A0 E1 A0 .proves to be a
ED E1 EE A0 F7 E8 EF A0 F0 F2 EF F6 E9 E4 E5 F3 man who provides
A0 F4 E8 E5 A0 F3 EF EC E5 FF F3 F5 F0 F0 EF F2  the sole.suppor
F4 A0 E6 EF F2 A0 E1 A0 F9 EF F5 EE E7 A0 E7 E9 t for a young gi
F2 EC AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 FF C1 A9 rl. Dost thou.A)
A0 F3 F0 E1 F2 E5 A0 E8 E9 ED A0 E9 EE A0 C3 EF  spare him in Co
ED F0 E1 F3 F3 E9 EF EE A0 E6 EF F2 A0 F4 E8 E5 mpassion for the
A0 E7 E9 F2 EC BB FF EF F2 A0 C2 A9 A0 F3 EC E1  girl;.or B) sla
F9 A0 E8 E9 ED A0 E9 EE A0 F4 E8 E5 A0 EE E1 ED y him in the nam
E5 A0 EF E6 A0 CA F5 F3 F4 E9 E3 E5 BF 00       e of Justice?

;; text 0x0A

;; Compassion vs Sacrifice

D4 E8                                           Th
E5 E5 A0 E1 EE E4 A0 F4 E8 F9 A0 E6 F2 E9 E5 EE ee and thy frien
E4 F3 A0 E8 E1 F6 E5 A0 E2 E5 E5 EE A0 F2 EF F5 ds have been rou
F4 E5 E4 FF E1 EE E4 A0 EF F2 E4 E5 F2 E5 E4 A0 ted.and ordered
F4 EF A0 F2 E5 F4 F2 E5 E1 F4 AE A0 C9 EE A0 E4 to retreat. In d
E5 E6 E9 E1 EE E3 E5 A0 EF E6 FF F4 E8 F9 A0 EF efiance of.thy o
F2 E4 E5 F2 F3 AC A0 E4 EF F3 F4 A0 F4 E8 EF F5 rders, dost thou
A0 C1 A9 A0 F3 F4 EF F0 A0 E9 EE FF C3 EF ED F0  A) stop in.Comp
E1 F3 F3 E9 EF EE A0 F4 EF A0 E1 E9 E4 A0 E1 A0 assion to aid a
F7 EF F5 EE E4 E5 E4 A0 E3 EF ED F0 E1 EE E9 EF wounded companio
EE BB FF EF F2 A0 C2 A9 A0 D3 E1 E3 F2 E9 E6 E9 n;.or B) Sacrifi
E3 E5 A0 F4 E8 F9 F3 E5 EC E6 A0 F4 EF A0 F3 EC ce thyself to sl
EF F7 A0 F4 E8 E5 FF F0 F5 F2 F3 F5 E9 EE E7 A0 ow the.pursuing
E5 EE E5 ED F9 AC A0 F3 EF A0 EF F4 E8 E5 F2 F3 enemy, so others
A0 E3 E1 EE A0 E5 F3 E3 E1 F0 E5 BF 00           can escape?.

;; text 0x0B

;; Compassion vs Honor

D4 E8 EF                                        Tho
F5 A0 E1 F2 F4 A0 F3 F7 EF F2 EE A0 F4 EF A0 F5 u art sworn to u
F0 E8 EF EC E4 A0 E1 A0 CC EF F2 E4 A0 F7 E8 EF phold a Lord who
FF F0 E1 F2 F4 E9 E3 E9 F0 E1 F4 E5 F3 A0 E9 EE .participates in
A0 F4 E8 E5 A0 E6 EF F2 E2 E9 E4 E4 E5 EE A0 F4  the forbidden t
EF F2 F4 F5 F2 E5 A0 EF E6 FF F0 F2 E9 F3 EF EE orture of.prison
E5 F2 F3 AE A0 C5 E1 E3 E8 A0 EE E9 E7 E8 F4 A0 ers. Each night
F4 E8 E5 E9 F2 A0 E3 F2 E9 E5 F3 A0 EF E6 FF F0 their cries of.p
E1 E9 EE A0 F2 E5 E1 E3 E8 A0 F4 E8 E5 E5 AE A0 ain reach thee.
C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 D3 E8 EF Dost thou A) Sho
F7 FF C3 EF ED F0 E1 F3 F3 E9 EF EE A0 E2 F9 A0 w.Compassion by
F2 E5 F0 EF F2 F4 E9 EE E7 A0 F4 E8 E5 A0 E4 E5 reporting the de
E5 E4 F3 BB A0 EF F2 FF C2 A9 A0 C8 EF EE EF F2 eds; or.B) Honor
A0 F4 E8 F9 A0 EF E1 F4 E8 A0 E1 EE E4 A0 E9 E7  thy oath and ig
EE EF F2 E5 A0 F4 E8 E5 A0 E4 E5 E5 E4 F3 BF 00 nore the deeds?.

;; text 0x0C

;; Compassion vs Spirituality

D4 E8 EF F5 A0 E8 E1 F3 F4 A0 E2 E5 E5 EE A0 F4 Thou hast been t
E1 F5 E7 E8 F4 A0 F4 EF A0 F0 F2 E5 F3 E5 F2 F6 aught to preserv
E5 A0 E1 EC EC FF EC E9 E6 E5 A0 E1 F3 A0 F3 E1 e all.life as sa
E3 F2 E5 E4 AE A0 C1 A0 ED E1 EE A0 EC E9 E5 F3 cred. A man lies
A0 E6 E1 F4 E1 EC EC F9 A0 F3 F4 F5 EE E7 FF E2  fatally stung.b
F9 A0 E1 A0 F6 E5 EE EF ED EF F5 F3 A0 F3 E5 F2 y a venomous ser
F0 E5 EE F4 AE A0 C8 E5 A0 F0 EC E5 E1 E4 F3 A0 pent. He pleads
E6 EF F2 A0 E1 FF ED E5 F2 E3 E9 E6 F5 EC A0 E4 for a.merciful d
E5 E1 F4 E8 AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 eath. Dost thou
C1 A9 A0 F3 E8 EF F7 FF C3 EF ED F0 E1 F3 F3 E9 A) show.Compassi
EF EE A0 E1 EE E4 A0 E5 EE E4 A0 E8 E9 F3 A0 F0 on and end his p
E1 E9 EE BB A0 EF F2 A0 C2 A9 A0 E8 E5 E5 E4 FF ain; or B) heed.
F4 E8 F9 A0 D3 F0 E9 F2 E9 F4 F5 E1 EC A0 E2 E5 thy Spiritual be
EC E9 E5 E6 F3 A0 E1 EE E4 A0 F2 E5 E6 F5 F3 E5 liefs and refuse
BF 00                                           ?

;; text 0x0D

;; Compassion vs Humility

C1 F3 A0 EF EE E5 A0 EF E6 A0 F4 E8 E5 A0       As one of the
CB E9 EE E7 A7 F3 A0 C7 F5 E1 F2 E4 AC A0 F4 E8 King's Guard, th
F9 A0 C3 E1 F0 F4 E1 E9 EE FF E8 E1 F3 A0 E1 F3 y Captain.has as
EB E5 E4 A0 F4 E8 E1 F4 A0 EF EE E5 A0 E1 ED EF ked that one amo
EE E7 F3 F4 A0 F9 EF F5 A0 F6 E9 F3 E9 F4 FF E1 ngst you visit.a
A0 E8 EF F3 F0 E9 F4 E1 EC A0 F4 EF A0 E3 E8 E5  hospital to che
E5 F2 A0 F4 E8 E5 A0 E3 E8 E9 EC E4 F2 E5 EE A0 er the children
F7 E9 F4 E8 FF F4 E1 EC E5 F3 A0 EF E6 A0 F4 E8 with.tales of th
F9 A0 F6 E1 EC E9 E1 EE F4 A0 E4 E5 E5 E4 F3 AE y valiant deeds.
A0 C4 EF F3 F4 A0 F4 E8 EF F5 FF C1 A9 A0 D3 E8  Dost thou.A) Sh
EF F7 A0 F4 E8 F9 A0 C3 EF ED F0 E1 F3 F3 E9 EF ow thy Compassio
EE A0 E1 EE E4 A0 F0 EC E1 F9 A0 F4 E8 E5 FF E2 n and play the.b
F2 E1 E7 E7 E1 F2 F4 BB A0 EF F2 A0 C2 A9 A0 C8 raggart; or B) H
F5 ED E2 EC F9 A0 EC E5 F4 A0 E1 EE EF F4 E8 E5 umbly let anothe
F2 A0 E7 EF BF 00                               r go?.

;; text 0x0E

;; Valor vs Justice

D4 E8 EF F5 A0 E8 E1 F3 F4 A0                   Thou hast
E2 E5 E5 EE A0 F3 E5 EE F4 A0 F4 EF A0 F3 E5 E3 been sent to sec
F5 F2 E5 A0 E1 A0 EE E5 E5 E4 E5 E4 FF F4 F2 E5 ure a needed.tre
E1 F4 F9 A0 F7 E9 F4 E8 A0 E1 A0 E4 E9 F3 F4 E1 aty with a dista
EE F4 A0 CC EF F2 E4 AE A0 D4 E8 F9 A0 E8 EF F3 nt Lord. Thy hos
F4 A0 E9 F3 FF E1 E7 F2 E5 E5 E1 E2 EC E5 A0 F4 t is.agreeable t
EF A0 F4 E8 E5 A0 F0 F2 EF F0 EF F3 E1 EC A0 E2 o the proposal b
F5 F4 A0 E9 EE F3 F5 EC F4 F3 FF F4 E8 F9 A0 E3 ut insults.thy c
EF F5 EE F4 F2 F9 A0 E1 F4 A0 E4 E9 EE EE E5 F2 ountry at dinner
AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 FF C1 A9 A0 D6 . Dost thou.A) V
E1 EC E9 E1 EE F4 EC F9 A0 E2 E5 E1 F2 A0 F4 E8 aliantly bear th
E5 A0 F3 EC F5 F2 F3 BB A0 EF F2 FF C2 A9 A0 CA e slurs; or.B) J
F5 F3 F4 EC F9 A0 F2 E9 F3 E5 A0 E1 EE E4 A0 E4 ustly rise and d
E5 ED E1 EE E4 A0 E1 EE A0 E1 F0 EF EC EF E7 F9 emand an apology
BF 00                                           ?

;; text 0x0F

;; Valor vs Sacrifice

C1 A0 ED E9 E7 E8 F4 F9 A0 EB EE E9 E7 E8       A mighty knigh
F4 A0 E1 E3 E3 EF F3 F4 F3 A0 F4 E8 E5 E5 A0 E1 t accosts thee a
EE E4 A0 E4 E5 ED E1 EE E4 F3 FF F4 E8 F9 A0 E6 nd demands.thy f
EF EF E4 AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 ood. Dost thou A
A9 A0 D6 E1 EC E9 E1 EE F4 EC F9 A0 F2 E5 E6 F5 ) Valiantly refu
F3 E5 FF E1 EE E4 A0 E5 EE E7 E1 E7 E5 A0 F4 E8 se.and engage th
E5 A0 EB EE E9 E7 E8 F4 BB A0 EF F2 A0 C2 A9 A0 e knight; or B)
D3 E1 E3 F2 E9 E6 E9 E3 E5 FF F4 E8 F9 A0 E6 EF Sacrifice.thy fo
EF E4 A0 F5 EE F4 EF A0 F4 E8 E5 A0 E8 F5 EE E7 od unto the hung
F2 F9 A0 EB EE E9 E7 E8 F4 BF 00                ry knight?.

;; text 0x10

;;

C4 F5 F2 E9 EE                                  Durin
E7 A0 E2 E1 F4 F4 EC E5 A0 F4 E8 EF F5 A0 E1 F2 g battle thou ar
F4 A0 EF F2 E4 E5 F2 E5 E4 A0 F4 EF A0 E7 F5 E1 t ordered to gua
F2 E4 FF F4 E8 F9 A0 E3 EF ED ED E1 EE E4 E5 F2 rd.thy commander
A7 F3 A0 E5 ED F0 F4 F9 A0 F4 E5 EE F4 AE A0 D4 's empty tent. T
E8 E5 A0 E2 E1 F4 F4 EC E5 FF E7 EF E5 F3 A0 F0 he battle.goes p
EF EF F2 EC F9 A0 E1 EE E4 A0 F4 E8 EF F5 A0 E4 oorly and thou d
EF F3 F4 A0 F9 E5 E1 F2 EE A0 F4 EF A0 E1 E9 E4 ost yearn to aid
FF F4 E8 F9 A0 E6 E5 EC EC EF F7 F3 AE A0 C4 EF .thy fellows. Do
F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 D6 E1 EC E9 E1 st thou A) Valia

WHAT IS THIS!?

00 08 94 13 20 1B 08 84 C2 CC CF C1 C4 A0 C1 81 .... ...BLOAD A.
CE C9 CD AC C1 A4 B4 B0 B0 B0 8D 84 C2 CC CF C1 NIM,A$4000..BLOA
C4 A0 C2 81 C7 CE C4 AC C1 A4 B7 C1 B0 B0 8D 00 D B.GND,A$7A00..
20 89 60 2C 54 C0 2C 57 C0 2C 52 C0 2C 50 C0 A9  .`,T@,W@,R@,P@)
70 85 10 A9 71 85 11 A9 00 85 12 A2 00 8A 9D 00 p..)q..)..."....
EE CA D0 FA 20 9D 60 20 3B 62 20 C8 60 20 3B 62 nJPz .` ;b H` ;b
20 E5 60 20 3B 62 20 6C 64 20 43 65 20 3B 62 20  e` ;b ld Ce ;b
05 61 20 3B 62 20 9F 65 20 3B 62 20 19 66 20 73 .a ;b .e ;b .f s
66 4C 4D 67 30 0D DA 08 DB 20 24 38 30 A2 20 86 fLMg0.Z.[ $80" .
81 A0 00 84 80 98 91 80 C8 D0 FB E6 81 CA D0 F6 . ......HP{f.JPv
60 A9 57 85 80 A9 62 85 81 A2 00 A1 80 F0 1C 8D `)W..)b..".!.p..
80 60 20 34 62 A9 BF 38 E1 80 8D 81 60 20 34 62 .` 4b)?8a...` 4b
20 04 62 A9 40 20 46 62 4C A5 60 60 A9 00 8D C0  .b)@ FbL%``)..@
61 A9 13 8D 82 60 8D 84 60 A9 11 8D 83 60 8D 85 a)...`..`)...`..
60 A2 04 A0 04 20 3E 61 60 A9 40 8D 80 60 A9 1F `". . >a`)@..`).
8D 81 60 20 0D 62 A9 30 20 46 62 EE 80 60 EE 80 ..` .b)0 Fbn.`n.

;; Valor vs Humility

F4 E8 EF F5 A0 E1 F2 F4 FF E1 A0 F3 EB E9 EC EC thou art.a skill
E6 F5 EC A0 F7 F2 E5 F3 F4 EC E5 F2 AE A0 D4 E8 ful wrestler. Th
EF F5 A0 E8 E1 F3 F4 A0 E2 E5 E5 EE A0 FF E1 F3 ou hast been .as
EB E5 E4 A0 F4 EF A0 E6 E9 E7 E8 F4 A0 E9 EE A0 ked to fight in
E1 A0 EC EF E3 E1 EC A0 E3 E8 E1 ED F0 E9 EF EE a local champion
F3 E8 E9 F0 AE FF C4 EF F3 F4 A0 F4 E8 EF F5 A0 ship..Dost thou
C1 A9 A0 E1 E3 E3 E5 F0 F4 A0 F4 E8 E5 A0 E9 EE A) accept the in
F6 E9 F4 E1 F4 E9 EF EE A0 E1 EE E4 FF D6 E1 EC vitation and.Val
E9 E1 EE F4 EC F9 A0 E6 E9 E7 E8 F4 A0 F4 EF A0 iantly fight to
F7 E9 EE BB A0 EF F2 A0 C2 A9 A0 C8 F5 ED E2 EC win; or B) Humbl
F9 FF E4 E5 E3 EC E9 EE E5 A0 EB EE EF F7 E9 EE y.decline knowin
E7 A0 F4 E8 EF F5 A0 E1 F2 F4 A0 F3 F5 F2 E5 A0 g thou art sure
F4 EF A0 F7 E9 EE BF 00                         to win?.

;; Justice vs Sacrifice

C4 F5 F2 E9 EE E7 A0 E1                         During a
A0 F0 E9 F4 E3 E8 E5 E4 A0 E2 E1 F4 F4 EC E5 AC  pitched battle,
A0 F4 E8 EF F5 A0 E4 EF F3 F4 A0 F3 E5 E5 FF E1  thou dost see.a
A0 E6 E5 EC EC EF F7 A0 E4 E5 F3 E5 F2 F4 A0 E8  fellow desert h
E9 F3 A0 F0 EF F3 F4 AC A0 E5 EE E4 E1 EE E7 E5 is post, endange
F2 E9 EE E7 FF ED E1 EE F9 AE A0 C1 F3 A0 E8 E5 ring.many. As he
A0 E6 EC E5 E5 F3 AC A0 E8 E5 A0 E9 F3 A0 F3 E5  flees, he is se
F4 A0 F5 F0 EF EE A0 E2 F9 FF F3 E5 F6 E5 F2 E1 t upon by.severa
EC A0 E5 EE E5 ED E9 E5 F3 AE A0 C4 EF F3 F4 A0 l enemies. Dost
F4 E8 EF F5 A0 C1 A9 A0 CA F5 F3 F4 EC F9 A0 EC thou A) Justly l
E5 F4 FF E8 E9 ED A0 E6 E9 E7 E8 F4 A0 E1 EC EF et.him fight alo
EE E5 BB A0 EF F2 A0 C2 A9 A0 D2 E9 F3 EB A0 D3 ne; or B) Risk S
E1 E3 F2 E9 E6 E9 E3 E9 EE E7 FF F4 E8 E9 EE E5 acrificing.thine
A0 EF F7 EE A0 EC E9 E6 E5 A0 F4 EF A0 E1 E9 E4  own life to aid
A0 E8 E9 ED BF 00                                him?

;; Justice vs Honor

D4 E8 EF F5 A0 E8 E1 F3 F4 A0                   Thou hast
F3 F7 EF F2 EE A0 F4 EF A0 E4 EF A0 F4 E8 F9 A0 sworn to do thy
CC EF F2 E4 A7 F3 FF E2 E9 E4 E4 E9 EE E7 A0 E9 Lord's.bidding i
EE A0 E1 EC EC AE A0 C8 E5 A0 E3 EF F6 E5 F4 F3 n all. He covets
A0 E1 A0 F0 E9 E5 E3 E5 A0 EF E6 FF EC E1 EE E4  a piece of.land
A0 E1 EE E4 A0 EF F2 E4 E5 F2 F3 A0 F4 E8 E5 A0  and orders the
EF F7 EE E5 F2 A0 F2 E5 ED EF F6 E5 E4 AE A0 C4 owner removed. D
EF F3 F4 FF F4 E8 EF F5 A0 C1 A9 A0 F3 E5 F2 F6 ost.thou A) serv
E5 A0 CA F5 F3 F4 E9 E3 E5 AC A0 F2 E5 E6 F5 F3 e Justice, refus
E9 EE E7 A0 F4 EF A0 E1 E3 F4 AC FF F4 E8 F5 F3 ing to act,.thus
A0 E2 E5 E9 EE E7 A0 E4 E9 F3 E7 F2 E1 E3 E5 E4  being disgraced
BB A0 EF F2 A0 C2 A9 A0 C8 EF EE EF F2 A0 F4 E8 ; or B) Honor th
E9 EE E5 FF EF E1 F4 E8 A0 E1 EE E4 A0 F5 EE E6 ine.oath and unf
E1 E9 F2 EC F9 A0 E5 F6 E9 E3 F4 A0 F4 E8 E5 A0 airly evict the
EC E1 EE E4 EF F7 EE E5 F2 BF 00                landowner?

;; Justice vs Spirituality

D4 E8 EF F5 A0                                  Thou
E4 EF F3 F4 A0 E2 E5 EC E9 E5 F6 E5 A0 F4 E8 E1 dost believe tha
F4 A0 F6 E9 F2 F4 F5 E5 A0 F2 E5 F3 E9 E4 E5 F3 t virtue resides
FF E9 EE A0 E1 EC EC A0 F0 E5 EF F0 EC E5 AE A0 .in all people.
D4 E8 EF F5 A0 E4 EF F3 F4 A0 F3 E5 E5 A0 E1 A0 Thou dost see a
F2 EF E7 F5 E5 FF F3 F4 E5 E1 EC A0 E6 F2 EF ED rogue.steal from
A0 F4 E8 F9 A0 CC EF F2 E4 AE A0 C4 EF F3 F4 A0  thy Lord. Dost
F4 E8 EF F5 A0 C1 A9 A0 E3 E1 EC EC FF E8 E9 ED thou A) call.him
A0 F4 EF A0 CA F5 F3 F4 E9 E3 E5 BB A0 EF F2 A0  to Justice; or
C2 A9 A0 F0 E5 F2 F3 EF EE E1 EC EC F9 A0 F4 F2 B) personally tr
F9 A0 F4 EF FF F3 F7 E1 F9 A0 E8 E9 ED A0 E2 E1 y to.sway him ba
E3 EB A0 F4 EF A0 F4 E8 E5 A0 D3 F0 E9 F2 E9 F4 ck to the Spirit
F5 E1 EC A0 F0 E1 F4 E8 A0 EF E6 FF E7 EF EF E4 ual path of.good
BF 00                                           ?.

;; Justice vs Humility

D5 EE F7 E9 F4 EE E5 F3 F3 E5 E4 AC A0 F4       Unwitnessed, t
E8 EF F5 A0 E8 E1 F3 F4 A0 F3 EC E1 E9 EE A0 E1 hou hast slain a
A0 E7 F2 E5 E1 F4 FF E4 F2 E1 E7 EF EE A0 E9 EE  great.dragon in
A0 F3 E5 EC E6 A0 E4 E5 E6 E5 EE F3 E5 AE A0 C1  self defense. A
A0 F0 EF EF F2 A0 F7 E1 F2 F2 E9 EF F2 FF E3 EC  poor warrior.cl
E1 E9 ED F3 A0 F4 E8 E5 A0 EF E6 E6 E5 F2 E5 E4 aims the offered
A0 F2 E5 F7 E1 F2 E4 AE A0 C4 EF F3 F4 A0 F4 E8  reward. Dost th
EF F5 FF C1 A9 A0 CA F5 F3 F4 EC F9 A0 F3 F4 E5 ou.A) Justly ste
F0 A0 E6 EF F2 F7 E1 F2 E4 A0 F4 EF A0 E3 EC E1 p forward to cla
E9 ED A0 F4 E8 E5 A0 FF F2 E5 F7 E1 F2 E4 BB A0 im the .reward;
EF F2 A0 C2 A9 A0 C8 F5 ED E2 EC F9 A0 E7 EF A0 or B) Humbly go
E1 E2 EF F5 F4 A0 EC E9 E6 E5 AC FF F3 E5 E3 F5 about life,.secu
F2 E5 A0 E9 EE A0 F4 E8 F9 A0 F3 E5 EC E6 AD E5 re in thy self-e
F3 F4 E5 E5 ED BF 00                            steem?.

;; Sacrifice vs Honor

D4 E8 EF F5 A0 E1 F2 F4 A0                      Thou art
E1 A0 E2 EF F5 EE F4 F9 A0 E8 F5 EE F4 E5 F2 A0 a bounty hunter
F3 F7 EF F2 EE A0 F4 EF A0 F2 E5 F4 F5 F2 EE FF sworn to return.
E1 EE A0 E1 EC EC E5 E7 E5 E4 A0 ED F5 F2 E4 E5 an alleged murde
F2 E5 F2 AE A0 C1 E6 F4 E5 F2 A0 E8 E9 F3 A0 E3 rer. After his c
E1 F0 F4 F5 F2 E5 AC FF F4 E8 EF F5 A0 E2 E5 EC apture,.thou bel
E9 E5 F6 E5 F3 F4 A0 E8 E9 ED A0 F4 EF A0 E2 E5 ievest him to be
A0 E9 EE EE EF E3 E5 EE F4 AE A0 A0 C4 EF F3 F4  innocent.  Dost
FF F4 E8 EF F5 A0 C1 A9 A0 D3 E1 E3 F2 E9 E6 E9 .thou A) Sacrifi
E3 E5 A0 F4 E8 F9 A0 F3 E9 FA E5 E1 E2 EC E5 A0 ce thy sizeable
E2 EF F5 EE F4 F9 FF E6 EF F2 A0 F4 E8 F9 A0 E2 bounty.for thy b
E5 EC E9 E5 E6 BB A0 EF F2 A0 C2 A9 A0 C8 EF EE elief; or B) Hon
EF F2 A0 F4 E8 F9 A0 EF E1 F4 E8 A0 F4 EF FF F2 or thy oath to.r
E5 F4 F5 F2 EE A0 E8 E9 ED A0 E1 F3 A0 F4 E8 EF eturn him as tho
F5 A0 E8 E1 F3 F4 A0 F0 F2 EF ED E9 F3 E5 E4 BF u hast promised?
00

;; Sacrifice vs Spirituality

D4 E8 EF F5 A0 E8 E1 F3 F4 A0 F3 F0 E5 EE F4    Thou hast spent
A0 F4 E8 F9 A0 EC E9 E6 E5 A0 E9 EE A0 E3 E8 E1  thy life in cha
F2 E9 F4 E1 E2 EC E5 FF E1 EE E4 A0 F2 E9 E7 E8 ritable.and righ
F4 E5 EF F5 F3 A0 F7 EF F2 EB AE A0 D4 E8 E9 EE teous work. Thin
E5 A0 F5 EE E3 EC E5 A0 F4 E8 E5 FF E9 EE EE EB e uncle the.innk
E5 E5 F0 E5 F2 A0 EC E9 E5 F3 A0 E9 EC EC A0 E1 eeper lies ill a
EE E4 A0 E1 F3 EB F3 A0 F9 EF F5 A0 F4 EF A0 F4 nd asks you to t
E1 EB E5 FF EF F6 E5 F2 A0 E8 E9 F3 A0 F4 E1 F6 ake.over his tav
E5 F2 EE AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 ern. Dost thou A
A9 A0 D3 E1 E3 F2 E9 E6 E9 E3 E5 FF F4 E8 F9 A0 ) Sacrifice.thy
EC E9 E6 E5 A0 EF E6 A0 F0 F5 F2 E9 F4 F9 A0 F4 life of purity t
EF A0 E1 E9 E4 A0 F4 E8 F9 A0 EB E9 EE BB A0 EF o aid thy kin; o
F2 FF C2 A9 A0 E4 E5 E3 EC E9 EE E5 A0 A6 A0 E6 r.B) decline & f
EF EC EC EF F7 A0 F4 E8 F9 A0 D3 F0 E9 F2 E9 F4 ollow thy Spirit
A7 F3 A0 E3 E1 EC EC BF 00                      's call?.

;; Compassion? vs Humility

D4 E8 EF F5 A0 E1 F2                            Thou ar
F4 A0 E1 EE A0 E5 EC E4 E5 F2 EC F9 AC A0 F7 E5 t an elderly, we
E1 EC F4 E8 F9 A0 E5 E3 E3 E5 EE F4 F2 E9 E3 AE althy eccentric.
FF D4 E8 F9 A0 E5 EE E4 A0 E9 F3 A0 EE E5 E1 F2 .Thy end is near
AE A0 C4 EF F3 F4 A0 F4 E8 EF F5 A0 C1 A9 A0 E4 . Dost thou A) d
EF EE E1 F4 E5 A0 E1 EC EC FF F4 E8 F9 A0 F7 E5 onate all.thy we
E1 EC F4 E8 A0 F4 EF A0 E6 E5 E5 E4 A0 E8 F5 EE alth to feed hun
E4 F2 E5 E4 F3 A0 EF E6 A0 F3 F4 E1 F2 F6 E9 EE dreds of starvin
E7 FF E3 E8 E9 EC E4 F2 E5 EE AC A0 E1 EE E4 A0 g.children, and
F2 E5 E3 E5 E9 F6 E5 A0 F0 F5 E2 EC E9 E3 A0 E1 receive public a
E4 F5 EC E1 F4 E9 EF EE BB FF EF F2 A0 C2 A9 A0 dulation;.or B)
C8 F5 ED E2 EC F9 A0 EC E9 F6 E5 A0 EF F5 F4 A0 Humbly live out
F4 E8 F9 A0 EC E9 E6 E5 AC A0 F7 E9 EC EC E9 EE thy life, willin
E7 FF F4 E8 F9 A0 E6 EF F2 F4 F5 EE E5 A0 F4 EF g.thy fortune to
A0 F4 E8 F9 A0 E8 E5 E9 F2 F3 BF 00              thy heirs?.

;; Honor vs Spirituality

C9 EE A0 F4                                     In t
E8 F9 A0 F9 EF F5 F4 E8 A0 F4 E8 EF F5 A0 F0 EC hy youth thou pl
E5 E4 E7 E5 E4 A0 F4 EF A0 ED E1 F2 F2 F9 A0 F4 edged to marry t
E8 F9 FF F3 F7 E5 E5 F4 E8 E5 E1 F2 F4 AE A0 CE hy.sweetheart. N
EF F7 A0 F4 E8 EF F5 A0 E1 F2 F4 A0 EF EE A0 E1 ow thou art on a
A0 F3 E1 E3 F2 E5 E4 FF F1 F5 E5 F3 F4 A0 E9 EE  sacred.quest in
A0 E4 E9 F3 F4 E1 EE F4 A0 EC E1 EE E4 F3 AE A0  distant lands.
D4 E8 F9 A0 F3 F7 E5 E5 F4 E8 E5 E1 F2 F4 A0 FF Thy sweetheart .
E1 F3 EB F3 A0 F4 E8 E5 E5 A0 F4 EF A0 EB E5 E5 asks thee to kee
F0 A0 F4 E8 F9 A0 F6 EF F7 AE A0 C4 EF F3 F4 A0 p thy vow. Dost
F4 E8 EF F5 FF C1 A9 A0 C8 EF EE EF F2 A0 F4 E8 thou.A) Honor th
F9 A0 F0 EC E5 E4 E7 E5 A0 F4 EF A0 F7 E5 E4 BB y pledge to wed;
A0 EF F2 A0 C2 A9 A0 E6 EF EC EC EF F7 FF F4 E8  or B) follow.th
F9 A0 D3 F0 E9 F2 E9 F4 F5 E1 EC A0 E3 F2 F5 F3 y Spiritual crus
E1 E4 E5 BF 00                                  ade?.

;; Honor vs Humility

D4 E8 EF F5 A0 E1 F2 F4 A0 E1 F4                Thou art at
A0 E1 A0 E3 F2 EF F3 F3 F2 EF E1 E4 F3 A0 E9 EE  a crossroads in
A0 F4 E8 F9 A0 EC E9 E6 E5 AE FF C4 EF F3 F4 A0  thy life..Dost
F4 E8 EF F5 A0 C1 A9 A0 C3 E8 EF EF F3 E5 A0 F4 thou A) Choose t
E8 E5 A0 C8 EF EE EF F2 E1 E2 EC E5 A0 EC E9 E6 he Honorable lif
E5 FF EF E6 A0 E1 A0 D0 E1 EC E1 E4 E9 EE AC A0 e.of a Paladin,
F3 F4 F2 E9 F6 E9 EE E7 A0 E6 EF F2 A0 D4 F2 F5 striving for Tru
F4 E8 A0 E1 EE E4 FF C3 EF F5 F2 E1 E7 E5 BB A0 th and.Courage;
EF F2 A0 C2 A9 A0 C3 E8 EF EF F3 E5 A0 F4 E8 E5 or B) Choose the
A0 C8 F5 ED E2 EC E5 A0 EC E9 E6 E5 FF EF E6 A0  Humble life.of
E1 A0 D3 E8 E5 F0 E8 E5 F2 E4 AC A0 E1 EE E4 A0 a Shepherd, and
E1 A0 F7 EF F2 EC E4 A0 EF E6 A0 F3 E9 ED F0 EC a world of simpl
E9 E3 E9 F4 F9 FF E1 EE E4 A0 F0 E5 E1 E3 E5 BF icity.and peace?
00

;; Spirituality vs Humility?

D4 E8 F9 A0 F0 E1 F2 E5 EE F4 F3 A0 F7 E9 F3    Thy parents wis
E8 A0 F4 E8 E5 E5 A0 F4 EF A0 E2 E5 E3 EF ED E5 h thee to become
A0 E1 EE FF E1 F0 F0 F2 E5 EE F4 E9 E3 E5 AE A0  an.apprentice.
D4 F7 EF A0 F0 EF F3 E9 F4 E9 EF EE F3 A0 E1 F2 Two positions ar
E5 A0 E1 F6 E1 E9 EC E1 E2 EC E5 AE FF C4 EF F3 e available..Dos
F4 A0 F4 E8 EF F5 A0 C1 A9 A0 C2 E5 E3 EF ED E5 t thou A) Become
A0 E1 EE A0 E1 E3 EF EC F9 F4 E5 A0 E9 EE A0 F4  an acolyte in t
E8 E5 FF D3 F0 E9 F2 E9 F4 F5 E1 EC A0 EF F2 E4 he.Spiritual ord
E5 F2 BB A0 EF F2 A0 C2 A9 A0 C2 E5 E3 EF ED E5 er; or B) Become
A0 E1 EE FF E1 F3 F3 E9 F3 F4 E1 EE F4 A0 F4 EF  an.assistant to
A0 E1 A0 E8 F5 ED E2 EC E5 A0 F6 E9 EC EC E1 E7  a humble villag
E5 A0 E3 EF E2 E2 EC E5 F2 BF 00                e cobbler?.

;;;; 0x1D

A0 A0 D4 E8 E5                                  The
A0 E4 E1 F9 A0 E9 F3 A0 F7 E1 F2 ED AC A0 F9 E5  day is warm, ye
F4 A0 F4 E8 E5 F2 E5 A0 E9 F3 A0 E1 FF E3 EF EF t there is a.coo
EC E9 EE E7 A0 E2 F2 E5 E5 FA E5 AE A0 D4 E8 E5 ling breeze. The
A0 EC E1 F4 E5 F3 F4 A0 E9 EE A0 E1 A0 F3 E5 F2  latest in a ser
E9 E5 F3 FF EF E6 A0 F0 E5 F2 F3 EF EE E1 EC A0 ies.of personal
E3 F2 E9 F3 E5 F3 A0 F3 E5 E5 ED F3 A0 E9 EE F3 crises seems ins
F5 F2 ED EF F5 EE F4 E1 E2 EC E5 AE FF D9 EF F5 urmountable..You
A0 E1 F2 E5 A0 E2 E5 E9 EE E7 A0 F0 F5 EC EC E5  are being pulle
E4 A0 E1 F0 E1 F2 F4 A0 E9 EE A0 E1 EC EC FF E4 d apart in all.d
E9 F2 E5 E3 F4 E9 EF EE F3 AE 00                irections.

;; 0x1E

D9 E5 F4 A0 F4                                  Yet t
E8 E9 F3 A0 E1 E6 F4 E5 F2 EE EF EF EE A0 F7 E1 his afternoon wa
EC EB A0 E9 EE A0 F4 E8 E5 A0 E3 EF F5 EE F4 F2 lk in the countr
F9 AD FF F3 E9 E4 E5 A0 F3 EC EF F7 EC F9 A0 E2 y-.side slowly b
F2 E9 EE E7 F3 A0 F2 E5 EC E1 F8 E1 F4 E9 EF EE rings relaxation
A0 F4 EF A0 F9 EF F5 F2 FF E8 E1 F2 F2 E9 E5 E4  to your.harried
A0 ED E9 EE E4 AE A0 D4 E8 E5 A0 F3 EF E9 EC A0  mind. The soil
E1 EE E4 A0 F3 F4 E1 E9 EE A0 EF E6 FF ED EF E4 and stain of.mod
E5 F2 EE A0 E8 E9 E7 E8 AD F4 E5 E3 E8 A0 EC E9 ern high-tech li
F6 E9 EE E7 A0 E2 E5 E7 E9 EE F3 A0 F4 EF A0 F7 ving begins to w
E1 F3 E8 FF EF E6 E6 A0 E9 EE A0 EC E1 F9 E5 F2 ash.off in layer
F3 AE A0 D4 E8 E1 F4 A0 F7 E9 EC EC EF F7 A0 F4 s. That willow t
F2 E5 E5 A0 EE E5 E1 F2 A0 F4 E8 E5 FF F3 F4 F2 ree near the.str
E5 E1 ED A0 EC EF EF EB F3 A0 E3 EF ED E6 EF F2 eam looks comfor
F4 E1 E2 EC E5 A0 E1 EE E4 A0 E9 EE F6 E9 F4 E9 table and inviti
EE E7 AE 00                                     ng.

;; 0x1F

D4 E8 E5 A0 E2 F5 FA FA A0 EF E6 A0             The buzz of
E4 F2 E1 E7 EF EE E6 EC E9 E5 F3 A0 E1 EE E4 A0 dragonflies and
F4 E8 E5 A0 F7 E8 E9 F3 F0 E5 F2 FF EF E6 A0 F4 the whisper.of t
E8 E5 A0 F7 E9 EC EC EF F7 A7 F3 A0 F3 F7 E1 F9 he willow's sway
E9 EE E7 A0 E2 F2 E1 EE E3 E8 E5 F3 A0 E2 F2 E9 ing branches bri
EE E7 FF E1 A0 E4 E5 E5 F0 A0 F0 E5 E1 E3 E5 AE ng.a deep peace.
A0 D3 E5 E1 F2 E3 E8 E9 EE E7 A0 E9 EE F7 E1 F2  Searching inwar
E4 A0 E6 EF F2 FF F4 F2 E1 EE F1 F5 E9 EC E9 F4 d for.tranquilit
F9 A0 E1 EE E4 A0 E8 E1 F0 F0 E9 EE E5 F3 F3 AC y and happiness,
A0 F9 EF F5 A0 E3 EC EF F3 E5 FF F9 EF F5 F2 A0  you close.your
E5 F9 E5 F3 AE 00                               eyes.

;; 0x20

C1 A0 E8 E9 E7 E8 AD F0 E9 F4                   A high-pit
E3 E8 E5 E4 A0 E3 E1 F3 E3 E1 E4 E9 EE E7 A0 F3 ched cascading s
EF F5 EE E4 A0 EC E9 EB E5 FF E3 F2 F9 F3 F4 E1 ound like.crysta
EC A0 F7 E9 EE E4 A0 E3 E8 E9 ED E5 F3 A0 E9 ED l wind chimes im
F0 E9 EE E7 E5 F3 A0 EF EE A0 F9 EF F5 F2 FF E6 pinges on your.f
EC EF E1 F4 E9 EE E7 A0 E1 F7 E1 F2 E5 EE E5 F3 loating awarenes
F3 AE A0 C1 F3 A0 F9 EF F5 A0 EF F0 E5 EE A0 F9 s. As you open y
EF F5 F2 FF E5 F9 E5 F3 AC A0 F9 EF F5 A0 F3 E5 our.eyes, you se
E5 A0 E1 A0 F3 E8 E9 ED ED E5 F2 E9 EE E7 A0 E2 e a shimmering b
EC F5 E5 EE E5 F3 F3 A0 F2 E9 F3 E5 FF E6 F2 EF lueness rise.fro
ED A0 F4 E8 E5 A0 E7 F2 EF F5 EE E4 AE A0 D4 E8 m the ground. Th
E5 A0 F3 EF F5 EE E4 A0 F3 E5 E5 ED F3 A0 F4 EF e sound seems to
A0 E2 E5 FF E5 ED E1 EE E1 F4 E9 EE E7 A0 E6 F2  be.emanating fr
EF ED A0 F4 E8 E9 F3 A0 E7 EC EF F7 E9 EE E7 A0 om this glowing
F0 EF F2 F4 E1 EC AE 00                         portal.

;; 0x21

C9 F4 A0 E9 F3 A0 E4 E9                         It is di
E6 E6 E9 E3 F5 EC F4 A0 F4 EF A0 EC EF EF EB A0 fficult to look
E1 F4 A0 F4 E8 E5 A0 E2 EC F5 E5 EE E5 F3 F3 AE at the blueness.
FF CC E9 E7 E8 F4 A0 F3 E5 E5 ED F3 A0 F4 EF A0 .Light seems to
E2 E5 EE E4 A0 E1 EE E4 A0 E4 E9 F3 F4 EF F2 F4 bend and distort
A0 E1 F2 EF F5 EE E4 FF E9 F4 AC A0 F7 E8 E9 EC  around.it, whil
E5 A0 F4 E8 E5 A0 F3 EF F5 EE E4 A0 F7 E1 F6 E5 e the sound wave
F3 A0 E2 E5 E3 EF ED E5 A0 F3 EF FF E9 EE F4 E5 s become so.inte
EE F3 E5 AC A0 F4 E8 E5 F9 A0 E1 F0 F0 E5 E1 F2 nse, they appear
A0 F4 EF A0 E2 E5 E3 EF ED E5 A0 F6 E9 F3 E9 E2  to become visib
EC E5 AE 00                                     le.

;; 0x22

D4 E8 E5 A0 F0 EF F2 F4 E1 EC A0 E8             The portal h
E1 EE E7 F3 A0 F4 E8 E5 F2 E5 A0 E6 EF F2 A0 E1 angs there for a
A0 ED EF ED E5 EE F4 AC FF F4 E8 E5 EE AC A0 F7  moment,.then, w
E9 F4 E8 A0 F4 E8 E5 A0 F2 F5 F3 E8 A0 EF E6 A0 ith the rush of
E1 EE A0 E9 ED F0 EC EF E4 E9 EE E7 FF F6 E1 E3 an imploding.vac
F5 F5 ED AC A0 E9 F4 A0 F3 E9 EE EB F3 A0 E9 EE uum, it sinks in
F4 EF A0 F4 E8 E5 A0 E7 F2 EF F5 EE E4 AE FF D3 to the ground..S
EF ED E5 F4 E8 E9 EE E7 A0 F2 E5 ED E1 E9 EE F3 omething remains
A0 F3 F5 F3 F0 E5 EE E4 E5 E4 A0 E9 EE A0 ED E9  suspended in mi
E4 E1 E9 F2 FF E6 EF F2 A0 E1 A0 ED EF ED E5 EE dair.for a momen
F4 AC A0 E2 E5 E6 EF F2 E5 A0 E6 E1 EC EC E9 EE t, before fallin
E7 A0 F4 EF A0 E5 E1 F2 F4 E8 FF F7 E9 F4 E8 A0 g to earth.with
E1 A0 E8 E5 E1 F6 F9 A0 F4 E8 F5 E4 AE 00       a heavy thud.

;; 0x23

D3 EF                                           So
ED E5 F7 E8 E1 F4 A0 F3 E8 E1 EB E5 EE A0 E2 F9 mewhat shaken by
A0 F4 E8 E9 F3 A0 F6 E9 F3 E9 EF EE AC A0 F9 EF  this vision, yo
F5 FF F2 E9 F3 E5 A0 F4 EF A0 F9 EF F5 F2 A0 E6 u.rise to your f
E5 E5 F4 A0 F4 EF A0 E9 EE F6 E5 F3 F4 E9 E7 E1 eet to investiga
F4 E5 AE A0 C1 FF E3 F2 F5 E4 E5 A0 E3 E9 F2 E3 te. A.crude circ
EC E5 A0 EF E6 A0 F3 F4 EF EE E5 F3 A0 F3 F5 F2 le of stones sur
F2 EF F5 EE E4 F3 A0 F4 E8 E5 FF F3 F0 EF F4 A0 rounds the.spot
F7 E8 E5 F2 E5 A0 F4 E8 E5 A0 F0 EF F2 F4 E1 EC where the portal
A0 E1 F0 F0 E5 E1 F2 E5 E4 AE A0 D4 E8 E5 F2 E5  appeared. There
A0 E9 F3 FF F3 EF ED E5 F4 E8 E9 EE E7 A0 E7 EC  is.something gl
E9 EE F4 E9 EE E7 A0 E9 EE A0 F4 E8 E5 A0 E7 F2 inting in the gr
E1 F3 F3 AE 00                                  ass.

;; 0x24

D9 EF F5 A0 F0 E9 E3 EB A0 F5 F0                You pick up
A0 E1 EE A0 E1 ED F5 EC E5 F4 A0 F3 E8 E1 F0 E5  an amulet shape
E4 A0 EC E9 EB E5 A0 E1 FF E3 F2 EF F3 F3 A0 F7 d like a.cross w
E9 F4 E8 A0 E1 A0 EC EF EF F0 A0 E1 F4 A0 F4 E8 ith a loop at th
E5 A0 F4 EF F0 AE A0 C9 F4 A0 E9 F3 A0 E1 EE FF e top. It is an.
C1 EE EB E8 AC A0 F4 E8 E5 A0 F3 E1 E3 F2 E5 E4 Ankh, the sacred
A0 F3 F9 ED E2 EF EC A0 EF E6 A0 EC E9 E6 E5 A0  symbol of life
E1 EE E4 FF F2 E5 E2 E9 F2 F4 E8 AE A0 C2 F5 F4 and.rebirth. But
A0 F4 E8 E9 F3 A0 E3 EF F5 EC E4 A0 EE EF F4 A0  this could not
E8 E1 F6 E5 A0 ED E1 E4 E5 FF F4 E8 E5 A0 F4 E8 have made.the th
F5 E4 AC A0 F3 EF A0 F9 EF F5 A0 EC EF EF EB A0 ud, so you look
E1 E7 E1 E9 EE A0 E1 EE E4 A0 E6 E9 EE E4 FF E1 again and find.a
A0 EC E1 F2 E7 E5 A0 E2 EF EF EB A0 F7 F2 E1 F0  large book wrap
F0 E5 E4 A0 E9 EE A0 F4 E8 E9 E3 EB A0 E3 EC EF ped in thick clo
F4 E8 A1 00                                     th!.

;; 0x25

D7 E9 F4 E8 A0 F4 F2 E5 ED E2 EC E9             With trembli
EE E7 A0 E8 E1 EE E4 F3 A0 F9 EF F5 A0 F5 EE F7 ng hands you unw
F2 E1 F0 A0 F4 E8 E5 FF E2 EF EF EB AE A0 C2 E5 rap the.book. Be
E8 EF EC E4 AC A0 F4 E8 E5 A0 E3 EC EF F4 E8 A0 hold, the cloth
E9 F3 A0 E1 A0 ED E1 F0 AC A0 E1 EE E4 FF F7 E9 is a map, and.wi
F4 E8 E9 EE A0 EC E9 E5 F3 A0 EE EF F4 A0 EF EE thin lies not on
E5 A0 E2 EF EF EB AC A0 E2 F5 F4 A0 F4 F7 EF AE e book, but two.
A0 D4 E8 E5 FF ED E1 F0 A0 E9 F3 A0 EF E6 A0 E1  The.map is of a
A0 EC E1 EE E4 A0 F3 F4 F2 E1 EE E7 E5 A0 F4 EF  land strange to
A0 F9 EF F5 AC A0 E1 EE E4 A0 F4 E8 E5 FF F3 F4  you, and the.st
F9 EC E5 A0 F3 F0 E5 E1 EB F3 A0 EF E6 A0 E1 EE yle speaks of an
E3 E9 E5 EE F4 A0 E3 E1 F2 F4 EF E7 F2 E1 F0 E8 cient cartograph
F9 AE 00                                        y.

;; 0x26

D4 E8 E5 A0 F3 E3 F2 E9 F0 F4 A0 EF EE          The script on
A0 F4 E8 E5 A0 E3 EF F6 E5 F2 A0 EF E6 A0 F4 E8  the cover of th
E5 A0 E6 E9 F2 F3 F4 FF E2 EF EF EB A0 E9 F3 A0 e first.book is
E1 F2 E3 E1 EE E5 A0 E2 F5 F4 A0 F2 E5 E1 E4 E1 arcane but reada
E2 EC E5 AE A0 D4 E8 E5 A0 F4 E9 F4 EC E5 FF E9 ble. The title.i
F3 BA FF A0 A0 A0 A0 A0 A0 A0 A0 D4 E8 E5 A0 C8 s:.        The H
E9 F3 F4 EF F2 F9 A0 EF E6 A0 C2 F2 E9 F4 E1 EE istory of Britan
EE E9 E1 FF A0 A0 A0 A0 A0 A0 A0 A0 A0 A0 A0 A0 nia.
A0 A0 A0 E1 F3 A0 F4 EF EC E4 A0 E2 F9 FF A0 A0    as told by.
A0 A0 A0 A0 A0 A0 A0 A0 A0 A0 CB F9 EC E5 A0 F4           Kyle t
E8 E5 A0 D9 EF F5 EE E7 E5 F2 00                he Younger

;; 0x27

D4 E8 E5 A0 EF                                  The o
F4 E8 E5 F2 A0 E2 EF EF EB A0 E9 F3 A0 E4 E9 F3 ther book is dis
F4 F5 F2 E2 E9 EE E7 A0 F4 EF A0 EC EF EF EB A0 turbing to look
E1 F4 AE FF C9 F4 F3 A0 F3 ED E1 EC EC A0 E3 EF at..Its small co
F6 E5 F2 A0 E1 F0 F0 E5 E1 F2 F3 A0 F4 EF A0 E2 ver appears to b
E5 A0 E6 E1 F3 E8 E9 EF EE E5 E4 FF EF F5 F4 A0 e fashioned.out
EF E6 A0 F3 EF ED E5 A0 E6 EF F2 ED A0 EF E6 A0 of some form of
EC E5 E1 F4 E8 E5 F2 F9 A0 E8 E9 E4 E5 AC A0 E2 leathery hide, b
F5 F4 FF E6 F2 EF ED A0 F7 E8 E1 F4 A0 E3 F2 E5 ut.from what cre
E1 F4 F5 F2 E5 A0 E9 F3 A0 F5 EE E3 E5 F2 F4 E1 ature is uncerta
E9 EE AE A0 D4 E8 E5 FF F2 E5 E4 E4 E9 F3 E8 A0 in. The.reddish
E2 EC E1 E3 EB A0 F3 EB E9 EE A0 F2 E1 E4 E9 E1 black skin radia
F4 E5 F3 A0 E1 EE A0 E9 EE F4 E5 EE F3 E5 FF E1 tes an intense.a
F5 F2 E1 A0 F3 F5 E7 E7 E5 F3 F4 E9 F6 E5 A0 EF ura suggestive o
E6 A0 E1 EE E3 E9 E5 EE F4 A0 F0 EF F7 E5 F2 AE f ancient power.
00

;; 0x28

D4 E8 E5 A0 F4 EF EE E7 F5 E5 A0 EF E6 A0 F4    The tongue of t
E8 E5 A0 F4 E9 F4 EC E5 A0 E9 F3 A0 E2 E5 F9 EF he title is beyo
EE E4 A0 F9 EF F5 F2 FF EB E5 E5 EE AE A0 D9 EF nd your.keen. Yo
F5 A0 E4 E1 F2 E5 A0 EE EF F4 A0 EF F0 E5 EE A0 u dare not open
F4 E8 E5 A0 E2 EF EF EB A0 E1 EE E4 FF E4 E9 F3 the book and.dis
F4 F5 F2 E2 A0 F7 E8 E1 F4 E5 F6 E5 F2 A0 F3 EC turb whatever sl
E5 E5 F0 F3 A0 F7 E9 F4 E8 E9 EE AE A0 D9 EF F5 eeps within. You
FF E4 E5 E3 E9 E4 E5 A0 F4 EF A0 F0 E5 F2 F5 F3 .decide to perus
E5 A0 F4 E8 E5 A0 C8 E9 F3 F4 EF F2 F9 AE A0 D3 e the History. S
E5 F4 F4 EC E9 EE E7 FF E2 E1 E3 EB A0 F5 EE E4 ettling.back und
E5 F2 A0 F4 E8 E5 A0 F7 E9 EC EC EF F7 A0 F4 F2 er the willow tr
E5 E5 AC A0 F9 EF F5 A0 EF F0 E5 EE A0 F4 E8 E5 ee, you open the
FF E2 EF EF EB A0 F4 EF A0 F4 E8 E5 A0 D4 E1 E2 .book to the Tab
EC E5 A0 EF E6 A0 C3 EF EE F4 E5 EE F4 F3 AE 00 le of Contents..

;; 0x29

FF FF

A0 A0 A0 A0 A8 D9 EF F5 A0 F2 E5 E1 E4 A0           (You read
F4 E8 E5 A0 C2 EF EF EB A0 EF E6 A0 C8 E9 F3 F4 the Book of Hist
EF F2 F9 A9 00                                  ory)

;; 0x2A

FF FF

A8 CE EF AC A0 F2 E5 E1 EC                      (No, real
EC F9 A1 A0 D2 E5 E1 E4 A0 F4 E8 E5 A0 C2 EF EF ly! Read the Boo
EB A0 EF E6 A0 C8 E9 F3 F4 EF F2 F9 A1 A9 00    k of History!)

;; 0x2B

C3                                              C
EC EF F3 E9 EE E7 A0 F4 E8 E5 A0 E2 EF EF EB AC losing the book,
A0 F9 EF F5 A0 E1 E7 E1 E9 EE A0 F0 E9 E3 EB A0  you again pick
F5 F0 FF F4 E8 E5 A0 C1 EE EB E8 AE A0 C1 F3 A0 up.the Ankh. As
F9 EF F5 A0 E8 EF EC E4 A0 E9 F4 AC A0 F9 EF F5 you hold it, you
A0 E2 E5 E7 E9 EE A0 F4 EF FF E8 E5 E1 F2 A0 E1  begin to.hear a
A0 E8 E1 F5 EE F4 E9 EE E7 EC F9 A0 E6 E1 ED E9  hauntingly fami
EC E9 E1 F2 AC A0 EC F5 F4 E5 AD EC E9 EB E5 FF liar, lute-like.
F3 EF F5 EE E4 A0 F7 E1 E6 F4 E9 EE E7 A0 EF F6 sound wafting ov
E5 F2 A0 E1 A0 EE E5 E1 F2 E2 F9 A0 E8 E9 EC EC er a nearby hill
AE A0 D3 F4 E9 EC EC FF E3 EC F5 F4 E3 E8 E9 EE . Still.clutchin
E7 A0 F4 E8 E5 A0 F3 F4 F2 E1 EE E7 E5 A0 E1 F2 g the strange ar
F4 E9 E6 E1 E3 F4 F3 AC A0 F9 EF F5 FF F2 E9 F3 tifacts, you.ris
E5 A0 F5 EE E2 E9 E4 E4 E5 EE A0 E1 EE E4 A0 E3 e unbidden and c
EC E9 ED E2 A0 F4 E8 E5 A0 F3 EC EF F0 E5 AE 00 limb the slope..

;; 0x2C

C9 EE A0 F4 E8 E5 A0 F6 E1 EC EC E5 F9 A0 E2 E5 In the valley be
EC EF F7 A0 F9 EF F5 A0 F3 E5 E5 A0 F7 E8 E1 F4 low you see what
A0 E1 F0 F0 E5 E1 F2 F3 FF F4 EF A0 E2 E5 A0 E1  appears.to be a
A0 E6 E1 E9 F2 AE A0 C9 F4 A0 F3 E5 E5 ED F3 A0  fair. It seems
F3 F4 F2 E1 EE E7 E5 A0 F4 E8 E1 F4 A0 F9 EF F5 strange that you
FF E3 E1 ED E5 A0 F4 E8 E1 F4 A0 F7 E1 F9 A0 E5 .came that way e
E1 F2 EC E9 E5 F2 A0 E1 EE E4 A0 EE EF F4 E9 E3 arlier and notic
E5 E4 FF EE EF F4 E8 E9 EE E7 AE A0 C1 F3 A0 F9 ed.nothing. As y
EF F5 A0 ED F5 EC EC A0 F4 E8 E9 F3 A0 EF F6 E5 ou mull this ove
F2 AC A0 F9 EF F5 F2 FF E6 E5 E5 F4 A0 E3 E1 F2 r, your.feet car
F2 F9 A0 F9 EF F5 A0 E4 EF F7 EE A0 F4 EF F7 E1 ry you down towa
F2 E4 A0 F4 E8 E5 A0 F3 E9 F4 E5 AE 00          rd the site.

;; 0x2D

D4 E8 E9                                        Thi
F3 A0 E9 F3 A0 EE EF A0 EF F2 E4 E9 EE E1 F2 F9 s is no ordinary
A0 F4 F2 E1 F6 E5 EC EC E9 EE E7 FF E3 E1 F2 EE  travelling.carn
E9 F6 E1 EC AC A0 E2 F5 F4 A0 E1 A0 D2 E5 EE E1 ival, but a Rena
E9 F3 F3 E1 EE E3 E5 A0 C6 E1 E9 F2 AE A0 D4 E8 issance Fair. Th
E5 FF F0 E5 EE EE E1 EE F4 F3 A0 EF EE A0 F4 E8 e.pennants on th
E5 A0 F4 E5 EE F4 A0 F4 EF F0 F3 A0 E2 EC EF F7 e tent tops blow
A0 E2 F2 E9 F3 EB EC F9 FF E9 EE A0 F4 E8 E5 A0  briskly.in the
EC E1 F4 E5 A0 E1 E6 F4 E5 F2 EE EF EF EE A0 E2 late afternoon b
F2 E5 E5 FA E5 AE 00                            reeze.

;; 0x2E

D4 E8 E5 A0 F4 E9 E3 EB E5                      The ticke
F4 A0 F4 E1 EB E5 F2 A0 E1 F4 A0 F4 E8 E5 A0 D2 t taker at the R
E5 EE C6 E1 E9 F2 A7 F3 A0 E7 E1 F4 E5 FF F3 F4 enFair's gate.st
E1 F2 F4 F3 A0 F4 EF A0 E1 F3 EB A0 F9 EF F5 A0 arts to ask you
E6 EF F2 A0 ED EF EE E5 F9 AC A0 E2 F5 F4 A0 F5 for money, but u
F0 EF EE FF F3 F0 EF F4 F4 E9 EE E7 A0 F9 EF F5 pon.spotting you
F2 A0 C1 EE EB E8 A0 F3 E1 F9 F3 AC A0 A7 D7 E5 r Ankh says, 'We
EC E3 EC ED E5 AC FF E6 F2 E9 E5 EE E4 AE A0 C5 lclme,.friend. E
EE F4 E5 F2 A0 E9 EE A0 F0 E5 E1 E3 E5 A0 E1 EE nter in peace an
E4 A0 E6 E9 EE E4 A0 F9 EF F5 F2 FF F0 E1 F4 E8 d find your.path
AE A7 00                                        .'

0x2F

D4 E8 E5 A0 ED F5 F3 E9 E3 A0 E3 EF EE          The music con
F4 E9 EE F5 E5 F3 A0 F4 EF A0 F0 F5 EC EC A0 F9 tinues to pull y
EF F5 FF E6 EF F2 F7 E1 F3 E4 A0 E1 ED EF EE E7 ou.forwasd among
F3 F4 A0 F4 E8 E5 A0 ED E5 F2 E3 E8 E1 EE F4 F3 st the merchants
A0 E1 EE E4 FF F6 E5 EE E4 EF F2 F3 AE A0 C7 EC  and.vendors. Gl
E9 ED F0 F3 E5 F3 A0 EF E6 A0 E6 E1 E2 F5 EC EF impses of fabulo
F5 F3 A0 F4 F2 E5 E1 F3 F5 F2 E5 F3 FF E3 E1 EE us treasures.can
A0 E2 E5 A0 F3 E5 E5 EE A0 E9 EE A0 F3 EF EF E5  be seen in sooe
A0 EF E6 A0 F4 E8 E5 A0 F3 E8 E1 E4 EF F7 F9 FF  of the shadowy.
E2 EF EF F4 E8 F3 AE 00                         booths.

0x30

D4 E8 E5 F3 E5 A0 F0 E5                         These pe
EF F0 EC E5 A0 E1 F2 E5 A0 F6 E5 F2 F9 A0 E8 E1 ople are very ha
F0 F0 F9 AE A0 D4 E8 E5 F9 A0 F3 E5 E5 ED FF F4 ppy. They seem.t
EF A0 E7 EC EF F7 A0 F7 E9 F4 E8 A0 E1 EE A0 E9 o glow with an i
EE EE E5 F2 A0 EC E9 E7 E8 F4 AE A0 D3 EF ED E5 nner light. Some
A0 EC EF EF EB FF F5 F0 A0 E1 F3 A0 F9 EF F5 A0  look.up as you
F0 E1 F3 F3 A0 E1 EE E4 A0 F3 ED E9 EC E5 AC A0 pass and smile,
E2 F5 F4 A0 F9 EF F5 A0 E3 E1 EE EE EF F4 FF F3 but you cannot.s
F4 EF F0 AD AD A0 F4 E8 E5 A0 ED F5 F3 E9 E3 A0 top-- the music
E3 EF ED F0 E5 EC F3 A0 F9 EF F5 A0 F4 EF A0 ED compels you to m
EF F6 E5 FF EF EE F7 E1 F2 E4 A0 F4 E8 F2 EF F5 ove.onward throu
E7 E8 A0 F4 E8 E5 A0 E3 F2 EF F7 E4 AE 00       gh the crowd.

0x31

D4 E8                                           Th
F2 EF F5 E7 E8 A0 F4 E8 E5 A0 E7 E1 F4 E8 E5 F2 rough the gather
E9 EE E7 A0 E4 F5 F3 EB A0 F9 EF F5 A0 F3 E5 E5 ing dusk you see
A0 E1 FF F3 E5 E3 EC F5 E4 E5 E4 A0 E7 F9 F0 F3  a.secluded gyps
F9 A0 F7 E1 E7 EF EE A0 F3 E9 F4 F4 E9 EE E7 A0 y wagon sitting
EF E6 E6 A0 E9 EE A0 F4 E8 E5 FF F7 EF EF E4 F3 off in the.woods
AE A0 D4 E8 E5 A0 ED F5 F3 E9 E3 A0 F3 E5 E5 ED . The music seem
F3 A0 F4 EF A0 E5 ED E1 EE E1 F4 E5 A0 E6 F2 EF s to emanate fro
ED FF F4 E8 E5 A0 F7 E1 E7 EF EE AE A0 C1 F3 A0 m.the wagon. As
F9 EF F5 A0 E4 F2 E1 F7 A0 EE E5 E1 F2 E5 F2 AC you draw nearer,
A0 E1 A0 F7 EF ED E1 EE A7 F3 FF F6 EF E9 E3 E5  a woman's.voice
A0 F7 E5 E1 F6 E5 F3 A0 E9 EE F4 EF A0 F4 E8 E5  weaves into the
A0 ED F5 F3 E9 E3 AC A0 F3 E1 F9 E9 EE E7 BA FF  music, saying:.
A7 D9 EF F5 A0 ED E1 F9 A0 E1 F0 F0 F2 EF E1 E3 'You may approac
E8 AC A0 CF A0 F3 E5 E5 EB E5 F2 AE A7 00       h, O seeker.'

0x32

D9 EF                                           Yo
F5 A0 E5 EE F4 E5 F2 A0 F4 EF A0 E6 E9 EE E4 A0 u enter to find
E1 EE A0 EF EC E4 A0 E7 F9 F0 F3 F9 A0 F3 E9 F4 an old gypsy sit
F4 E9 EE E7 FF E9 EE A0 E1 A0 F3 ED E1 EC EC AC ting.in a small,
A0 E3 F5 F2 F4 E1 E9 EE E5 E4 A0 F2 EF EF ED AE  curtained room.
A0 D3 E8 E5 A0 F7 E5 E1 F2 F3 A0 E1 EE FF C1 EE  She wears an.An
EB E8 A0 E1 F2 EF F5 EE E4 A0 E8 E5 F2 A0 EE E5 kh around her ne
E3 EB AE A0 C9 EE A0 E6 F2 EF EE F4 A0 EF E6 A0 ck. In front of
E8 E5 F2 A0 E9 F3 FF E1 A0 F2 EF F5 EE E4 A0 F4 her is.a round t
E1 E2 EC E5 A0 E3 EF F6 E5 F2 E5 E4 A0 E9 EE A0 able covered in
E4 E5 E5 F0 A0 E7 F2 E5 E5 EE FF F6 E5 EC F6 E5 deep green.velve
F4 AE A0 D4 E8 E5 A0 F2 EF EF ED A0 F3 ED E5 EC t. The room smel
EC F3 A0 F3 EF A0 E8 E5 E1 F6 E9 EC F9 A0 EF E6 ls so heavily of
FF E9 EE E3 E5 EE F3 E5 A0 F4 E8 E1 F4 A0 F9 EF .incense that yo
F5 A0 E6 E5 E5 EC A0 E4 E9 FA FA F9 AE 00       u feel dizzy.

0x33

D3 E5                                           Se
E5 E9 EE E7 A0 F4 E8 E5 A0 C1 EE EB E8 AC A0 F4 eing the Ankh, t
E8 E5 A0 E1 EE E3 E9 E5 EE F4 A0 E7 F9 F0 F3 F9 he ancient gypsy
FF F3 ED E9 EC E5 F3 A0 E1 EE E4 A0 F7 E1 F2 EE .smiles and warn
F3 A0 F9 EF F5 A0 EE E5 F6 E5 F2 A0 F4 EF A0 F0 s you never to p
E1 F2 F4 A0 F7 E9 F4 E8 FF E9 F4 AE A0 A7 D7 E5 art with.it. 'We
A0 E8 E1 F6 E5 A0 E2 E5 E5 EE A0 F7 E1 E9 F4 E9  have been waiti
EE E7 A0 F3 F5 E3 E8 A0 E1 A0 EC EF EE E7 FF F4 ng such a long.t
E9 ED E5 AC A0 E2 F5 F4 A0 E1 F4 A0 EC E1 F3 F4 ime, but at last
A0 F9 EF F5 A0 E8 E1 F6 E5 A0 E3 EF ED E5 AE A0  you have come.
D3 E9 F4 FF E8 E5 F2 E5 A0 E1 EE E4 A0 C9 A0 F3 Sit.here and I s
E8 E1 EC EC A0 F2 E5 E1 E4 A0 F4 E8 E5 A0 F0 E1 hall read the pa
F4 E8 A0 EF E6 A0 F9 EF F5 F2 FF E6 F5 F4 F5 F2 th of your.futur
E5 AE A7 00                                     e.'

0x34

D5 F0 EF EE A0 F4 E8 E5 A0 F4 E1 E2             Upon the tab
EC E5 A0 F3 E8 E5 A0 F0 EC E1 E3 E5 F3 A0 E1 A0 le she places a
E3 F5 F2 E9 EF F5 F3 FF F7 EF EF E4 E5 EE A0 EF curious.wooden o
E2 EA E5 E3 F4 A0 EC E9 EB E5 A0 E1 EE A0 E1 E2 bject like an ab
E1 E3 F5 F3 A0 E2 F5 F4 A0 F7 E9 F4 E8 EF F5 F4 acus but without
FF E2 E5 E1 E4 F3 AE A0 C9 EE A0 E8 E5 F2 A0 E8 .beads. In her h
E1 EE E4 F3 A0 F3 E8 E5 A0 E8 EF EC E4 F3 A0 E5 ands she holds e
E9 E7 E8 F4 FF F5 EE F5 F3 F5 E1 EC A0 E3 E1 F2 ight.unusual car
E4 F3 AE A0 A7 CC E5 F4 A0 F5 F3 A0 E2 E5 E7 E9 ds. 'Let us begi
EE A0 F4 E8 E5 FF E3 E1 F3 F4 E9 EE E7 AE A7 00 n the.casting.'.

0x35

D4 E8 E5 A0 E7 F9 F0 F3 F9 A0 F0 EC E1 E3 E5 F3 The gypsy places
A0 F4 E8 E5 A0 E6 E9 F2 F3 F4 A0 F4 F7 EF A0 E3  the first two c
E1 F2 E4 F3 FF 00                               ards.

0x36

D4 E8 E5 A0 E7 F9 F0 F3 F9 A0                   The gypsy
F0 EC E1 E3 E5 F3 A0 F4 F7 EF A0 ED EF F2 E5 A0 places two more
EF E6 A0 F4 E8 E5 A0 E3 E1 F2 E4 F3 FF 00       of the cards.

0x37

D4 E8                                           Th
E5 A0 E7 F9 F0 F3 F9 A0 F0 EC E1 E3 E5 F3 A0 F4 e gypsy places t
E8 E5 A0 EC E1 F3 F4 A0 F4 F7 EF A0 E3 E1 F2 E4 he last two card
F3 FF 00                                        s

0x38

F5 F0 EF EE A0 F4 E8 E5 A0 F4 E1 E2 EC          upon the tabl
E5 AC A0 F4 E8 E5 F9 A0 E1 F2 E5 A0 F4 E8 E5 A0 e, they are the
E3 E1 F2 E4 F3 A0 EF E6 FF 00                   cards of

;; virtue names $39 + virtue number

0x39

E8 EF EE E5 F3 F4 F9 00                         honesty

0x3A

E3 EF ED F0 E1 F3 F3 E9 EF EE 00                compassion

;; text 0x3B

F6 E1 EC EF F2 00                               valor

;; text 0x3C

EA F5 F3 F4 E9 E3 E5 00                         justice

;; text 0x3D

F3 E1 E3 F2 E9 E6 E9 E3 E5 00                   sacrifice

;; text 0x3E

E8 EF EE EF F2 00                               honor

;; text 0x3F

F3 F0 E9 F2 E9 F4 F5 E1 EC E9 F4 F9 00          spirituality

;; text 0x40

E8 F5 ED E9 EC E9 F4 F9 00                      humility

;; text 0x41

00

;; text 0x42

D7 E9 F4 E8 A0 F4 E8 E5 A0 E6 E9 EE E1 EC       With the final
A0 E3 E8 EF E9 E3 E5 AC A0 F4 E8 E5 A0 E9 EE E3  choice, the inc
E5 EE F3 E5 FF F3 F7 E5 EC EC F3 A0 F5 F0 A0 E1 ense.swells up a
F2 EF F5 EE E4 A0 F9 EF F5 AE A0 D4 E8 E5 A0 E7 round you. The g
F9 F0 F3 F9 A0 F3 F0 E5 E1 EB F3 FF E1 F3 A0 E9 ypsy speaks.as i
E6 A0 E6 F2 EF ED A0 E1 A0 E7 F2 E5 E1 F4 A0 E4 f from a great d
E9 F3 F4 E1 EE E3 E5 AC A0 E8 E5 F2 A0 F6 EF E9 istance, her voi
E3 E5 FF E7 F2 EF F7 E9 EE E7 A0 E6 E1 E9 EE F4 ce.growing faint
E5 F2 A0 F7 E9 F4 E8 A0 E5 E1 E3 E8 A0 F7 EF F2 er with each wor
E4 BA FF A7 D3 EF A0 E2 E5 A0 E9 F4 A1 A0 D4 E8 d:.'So be it! Th
F9 A0 F0 E1 F4 E8 A0 E9 F3 A0 E3 E8 EF F3 E5 EE y path is chosen
A1 A7 00                                        !'.

;; text 0x43

D4 E8 E5 F2 E5 A0 E9 F3 A0 E1 A0 ED EF          There is a mo
ED E5 EE F4 A0 EF E6 A0 E9 EE F4 E5 EE F3 E5 AC ment of intense,
A0 F7 F2 E5 EE E3 E8 E9 EE E7 FF F6 E5 F2 F4 E9  wrenching.verti
E7 EF AE A0 A0 C1 F3 A0 F9 EF F5 A0 E3 EC EF F3 go.  As you clos
E5 A0 F9 EF F5 F2 A0 E5 F9 E5 F3 AC A0 E1 FF F6 e your eyes, a.v
EF E9 E3 E5 A0 F7 E8 E9 F3 F0 E5 F2 F3 A0 F7 E9 oice whispers wi
F4 E8 E9 EE A0 F9 EF F5 F2 A0 ED E9 EE E4 AC A0 thin your mind,
A7 D3 E5 E5 EB FF F4 E8 E5 A0 E3 EF F5 EE F3 E5 'Seek.the counse
EC A0 EF E6 A0 F4 E8 F9 A0 F3 EF F6 E5 F2 E5 E9 l of thy soverei
E7 EE AE A7 A0 C1 E6 F4 E5 F2 A0 E1 FF ED EF ED gn.' After a.mom
E5 EE F4 AC A0 F4 E8 E5 A0 F3 F0 E9 EE EE E9 EE ent, the spinnin
E7 A0 F3 F5 E2 F3 E9 E4 E5 F3 AC A0 E1 EE E4 A0 g subsides, and
F9 EF F5 FF EF F0 E5 EE A0 F9 EF F5 F2 A0 E5 F9 you.open your ey
E5 F3 A0 F4 EF AE AE AE 00                      es to...

00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
```