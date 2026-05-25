; =========================================================
;           NUMBER GUESSING GAME SYSTEM
; =========================================================
;
; FEATURE 1 : Dynamic Secret Number Generation
; FEATURE 2 : User Input System
; FEATURE 3 : Intelligent Hint Feedback System
; FEATURE 4 : Difficulty & Attempt System
; FEATURE 5 : Score Calculation and Ranking System
; FEATURE 6 : Game History and Replay System
;
; =========================================================

.MODEL SMALL
.STACK 100H

; =========================================================
;                         MACROS
; =========================================================

PRINT MACRO MSG
    LEA DX, MSG
    MOV AH, 09H
    INT 21H
ENDM

NEWLINE MACRO
    MOV AH, 02H
    MOV DL, 13
    INT 21H
    MOV DL, 10
    INT 21H
ENDM

; =========================================================
;                      DATA SEGMENT
; =========================================================

.DATA

; ================= TITLE =================

title1 DB 10,13,'#############################################################$'
title2 DB 10,13,'#                                                           #$'
title3 DB 10,13,'#               NUMBER GUESSING GAME SYSTEM                 #$'
title4 DB 10,13,'#                                                           #$'
title5 DB 10,13,'#############################################################$'

; ================= MENU =================

menu1 DB 10,13,'=================================================$'
menu2 DB 10,13,'               SELECT DIFFICULTY                 $'
menu3 DB 10,13,'=================================================$'

easyMsg   DB 10,13,'               [1] EASY MODE   (1-30) Attempts:7$'
mediumMsg DB 10,13,'               [2] MEDIUM MODE (1-50) Attempts:6$'
hardMsg   DB 10,13,'               [3] HARD MODE   (1-99) Attempts:5$'

guessMsg   DB 10,13,'Enter Your Guess: $'
attemptMsg DB 10,13,'Remaining Attempts: $'

invalidMsg DB 10,13,'Invalid Input! Try Again.$'

; ================= HINTS =================

correctMsg DB 10,13,'Correct Guess !!!$'

veryCloseHigh DB 10,13,'Very Close! Too High.$'
veryCloseLow  DB 10,13,'Very Close! Too Low.$'

closeHigh DB 10,13,'Close Guess! Too High.$'
closeLow  DB 10,13,'Close Guess! Too Low.$'

farHigh DB 10,13,'Far Away! Too High.$'
farLow  DB 10,13,'Far Away! Too Low.$'

; ================= WIN / LOSE =================

win1 DB 10,13,'*********************************************************$'
win2 DB 10,13,'*************** CONGRATULATIONS !!! *********************$'
win3 DB 10,13,'*********************************************************$'

lose1 DB 10,13,'#########################################################$'
lose2 DB 10,13,'################### GAME OVER ###########################$'
lose3 DB 10,13,'#########################################################$'

secretMsg DB 10,13,'Secret Number Was: $'

; ================= SCORE =================

scoreText DB 10,13,'Your Score: $'

rankTitle DB 10,13,'===== TOP SCORES =====$'
rank1Msg  DB 10,13,'1st : $'
rank2Msg  DB 10,13,'2nd : $'
rank3Msg  DB 10,13,'3rd : $'

; ================= HISTORY =================

historyTitle DB 10,13,'===== GAME HISTORY =====$'

; ================= REPLAY =================

replayMsg DB 10,13,'Play Again? (Y/N): $'

; ================= EXIT =================

exit1 DB 10,13,'#########################################################$'
exit2 DB 10,13,'#                                                       #$'
exit3 DB 10,13,'#         THANK YOU FOR PLAYING OUR GAME                #$'
exit4 DB 10,13,'#                                                       #$'
exit5 DB 10,13,'#########################################################$'

; ================= VARIABLES =================

secretNum DB ?
userGuess DB ?

difficulty DB ?
attempts   DB ?

minRange DB ?
maxRange DB ?

digit1 DB ?
digit2 DB ?

; ================= FEATURE 5 VARIABLES =================

score      DB ?
multiplier DB ?

scoreArray DB 3 DUP(0)

; ================= FEATURE 6 VARIABLES =================

history1  DB '-'
history2  DB '-'
history3  DB '-'

scoreHist1 DB 0
scoreHist2 DB 0
scoreHist3 DB 0

; =========================================================
;                       CODE SEGMENT
; =========================================================

.CODE

MAIN PROC

    MOV AX, @DATA
    MOV DS, AX

GAME_START:

    CALL DISPLAY_TITLE
    CALL LOADING_SCREEN
    CALL SELECT_DIFFICULTY
    CALL GENERATE_SECRET

GAME_LOOP:

    CALL INPUT_NUMBER
    CALL CHECK_GUESS

    CMP AL, 1
    JE PLAYER_WIN

    DEC attempts

    CMP attempts, 0
    JE PLAYER_LOSE

    PRINT attemptMsg

    MOV AL, attempts
    CALL PRINT_NUMBER

    NEWLINE

    JMP GAME_LOOP

; =========================================================
; PLAYER WIN
; =========================================================

PLAYER_WIN:

    PRINT win1
    PRINT win2
    PRINT win3

    CALL CALCULATE_SCORE

    PRINT scoreText

    MOV AL, score
    CALL PRINT_NUMBER

    CALL UPDATE_RANK
    CALL DISPLAY_RANK

    ; Shift history BEFORE writing new result into slot1
    CALL UPDATE_HISTORY

    MOV history1, 'W'
    MOV AL, score
    MOV scoreHist1, AL

    CALL DISPLAY_HISTORY

    CALL REPLAY_GAME

    CMP AL, 1
    JE GAME_START

    JMP EXIT_PROGRAM

; =========================================================
; PLAYER LOSE
; =========================================================

PLAYER_LOSE:

    PRINT lose1
    PRINT lose2
    PRINT lose3

    PRINT secretMsg

    MOV AL, secretNum
    CALL PRINT_NUMBER

    NEWLINE

    ; Shift history BEFORE writing new result into slot1
    CALL UPDATE_HISTORY

    MOV history1, 'L'
    MOV scoreHist1, 0

    CALL DISPLAY_HISTORY

    CALL REPLAY_GAME

    CMP AL, 1
    JE GAME_START

    JMP EXIT_PROGRAM

; =========================================================
; EXIT PROGRAM
; =========================================================

EXIT_PROGRAM:

    PRINT exit1
    PRINT exit2
    PRINT exit3
    PRINT exit4
    PRINT exit5

    MOV AX, 4C00H
    INT 21H

MAIN ENDP

; =========================================================
; DISPLAY TITLE
; =========================================================

DISPLAY_TITLE PROC

    PRINT title1
    PRINT title2
    PRINT title3
    PRINT title4
    PRINT title5

    NEWLINE

    RET

DISPLAY_TITLE ENDP

; =========================================================
; LOADING SCREEN
; =========================================================

LOADING_SCREEN PROC

    MOV CX, 20

LOAD_LOOP:

    MOV AH, 02H
    MOV DL, '.'
    INT 21H

    LOOP LOAD_LOOP

    NEWLINE

    RET

LOADING_SCREEN ENDP

; =========================================================
; SELECT DIFFICULTY
; =========================================================

SELECT_DIFFICULTY PROC

MENU_AGAIN:

    PRINT menu1
    PRINT menu2
    PRINT menu3

    PRINT easyMsg
    PRINT mediumMsg
    PRINT hardMsg

    NEWLINE

    MOV AH, 01H
    INT 21H

    SUB AL, 48

    MOV difficulty, AL

    CMP AL, 1
    JE EASY

    CMP AL, 2
    JE MEDIUM

    CMP AL, 3
    JE HARD

    PRINT invalidMsg
    JMP MENU_AGAIN

EASY:

    MOV attempts,   7
    MOV minRange,   1
    MOV maxRange,   30
    MOV multiplier, 1
    RET

MEDIUM:

    MOV attempts,   6
    MOV minRange,   1
    MOV maxRange,   50
    MOV multiplier, 2
    RET

HARD:

    MOV attempts,   5
    MOV minRange,   1
    MOV maxRange,   99
    MOV multiplier, 3
    RET

SELECT_DIFFICULTY ENDP

; =========================================================
; SECRET NUMBER GENERATION
; =========================================================

GENERATE_SECRET PROC

    MOV AH, 2CH
    INT 21H                 ; Returns: CH=hour, CL=min, DH=sec, DL=1/100sec

    MOV AL, DH
    ADD AL, DL
    ADD AL, CL
    ADD AL, CH              ; Mix all time components for better randomness

    MOV AH, 0

    MOV BL, maxRange
    SUB BL, minRange
    INC BL                  ; range size = maxRange - minRange + 1

    DIV BL                  ; AH = AL mod BL  (remainder)

    MOV AL, AH              ; FIX 4: Must use AH (remainder), not AL (quotient)
    ADD AL, minRange

    MOV secretNum, AL

    RET

GENERATE_SECRET ENDP

; =========================================================
; INPUT SYSTEM
; =========================================================

INPUT_NUMBER PROC

INPUT_AGAIN:

    NEWLINE
    PRINT guessMsg

    MOV AH, 01H
    INT 21H

    CMP AL, '0'
    JB INVALID_INPUT        ; FIX 5: Use JB/JA (unsigned) instead of JL/JG
                             ;        for character comparisons to avoid sign issues
    CMP AL, '9'
    JA INVALID_INPUT

    SUB AL, 48
    MOV digit1, AL

    MOV AH, 01H
    INT 21H

    CMP AL, 13
    JE SINGLE_DIGIT

    CMP AL, '0'
    JB INVALID_INPUT        ; FIX 5 (continued): same fix here
    CMP AL, '9'
    JA INVALID_INPUT

    SUB AL, 48
    MOV digit2, AL

    MOV AL, digit1

    MOV BL, 10
    MUL BL                  ; AX = digit1 * 10

    ADD AL, digit2          ; FIX 6: Result of MUL BL goes into AX;
                             ;        adding digit2 to AL is correct since
                             ;        result fits in a byte (max 9*10+9=99)
    JMP STORE_NUMBER

SINGLE_DIGIT:

    MOV AL, digit1

STORE_NUMBER:

    MOV userGuess, AL

    CMP AL, minRange
    JB INVALID_INPUT        ; FIX 5 (continued): use unsigned JB/JA

    CMP AL, maxRange
    JA INVALID_INPUT

    RET

INVALID_INPUT:

    PRINT invalidMsg
    JMP INPUT_AGAIN

INPUT_NUMBER ENDP

; =========================================================
; HINT SYSTEM
; =========================================================

CHECK_GUESS PROC

    MOV AL, userGuess

    CMP AL, secretNum
    JE CORRECT

    JA TOO_HIGH

TOO_LOW:

    MOV AL, secretNum
    SUB AL, userGuess

    CMP AL, 3
    JBE VERY_CLOSE_LOW      ; FIX 7: Use JBE/JBE (unsigned <=) instead of JLE
                             ;        since these are unsigned byte values
    CMP AL, 10
    JBE CLOSE_LOW

    PRINT farLow
    MOV AL, 0
    RET

TOO_HIGH:

    MOV AL, userGuess
    SUB AL, secretNum

    CMP AL, 3
    JBE VERY_CLOSE_HIGH     ; FIX 7 (continued)

    CMP AL, 10
    JBE CLOSE_HIGH

    PRINT farHigh
    MOV AL, 0
    RET

VERY_CLOSE_LOW:

    PRINT veryCloseLow
    MOV AL, 0
    RET

VERY_CLOSE_HIGH:

    PRINT veryCloseHigh
    MOV AL, 0
    RET

CLOSE_LOW:

    PRINT closeLow
    MOV AL, 0
    RET

CLOSE_HIGH:

    PRINT closeHigh
    MOV AL, 0
    RET

CORRECT:

    PRINT correctMsg
    MOV AL, 1
    RET

CHECK_GUESS ENDP

; =========================================================
; SCORE CALCULATION
; =========================================================

CALCULATE_SCORE PROC

    MOV AL, attempts
    MOV BL, multiplier

    MUL BL                  ; AX = attempts * multiplier

    MOV BL, 10
    MUL BL                  ; AX = attempts * multiplier * 10
                             ; FIX 8: Result may overflow AL for large values;
                             ;        store from AX (use AL, acceptable here since
                             ;        max score = 7*3*10 = 210 which fits in a byte)
    MOV score, AL

    RET

CALCULATE_SCORE ENDP

; =========================================================
; UPDATE RANK
; =========================================================

UPDATE_RANK PROC

    MOV AL, score

    CMP AL, scoreArray[0]
    JBE CHECK_SECOND        ; FIX 9: Use JBE (unsigned) for score comparison

    MOV BL, scoreArray[1]
    MOV scoreArray[2], BL

    MOV BL, scoreArray[0]
    MOV scoreArray[1], BL

    MOV scoreArray[0], AL
    RET

CHECK_SECOND:

    CMP AL, scoreArray[1]
    JBE CHECK_THIRD

    MOV BL, scoreArray[1]
    MOV scoreArray[2], BL

    MOV scoreArray[1], AL
    RET

CHECK_THIRD:

    CMP AL, scoreArray[2]
    JBE END_RANK

    MOV scoreArray[2], AL

END_RANK:

    RET

UPDATE_RANK ENDP

; =========================================================
; DISPLAY RANK
; =========================================================

DISPLAY_RANK PROC

    PRINT rankTitle
    NEWLINE

    PRINT rank1Msg
    MOV AL, scoreArray[0]
    CMP AL, 0
    JE RANK1_EMPTY
    CALL PRINT_NUMBER
    JMP RANK2
RANK1_EMPTY:
    MOV AH, 02H
    MOV DL, '-'
    INT 21H

RANK2:
    NEWLINE
    PRINT rank2Msg
    MOV AL, scoreArray[1]
    CMP AL, 0
    JE RANK2_EMPTY
    CALL PRINT_NUMBER
    JMP RANK3
RANK2_EMPTY:
    MOV AH, 02H
    MOV DL, '-'
    INT 21H

RANK3:
    NEWLINE
    PRINT rank3Msg
    MOV AL, scoreArray[2]
    CMP AL, 0
    JE RANK3_EMPTY
    CALL PRINT_NUMBER
    JMP RANK_DONE
RANK3_EMPTY:
    MOV AH, 02H
    MOV DL, '-'
    INT 21H

RANK_DONE:
    NEWLINE

    RET

DISPLAY_RANK ENDP

; =========================================================
; UPDATE HISTORY
; =========================================================
; Shifts OLD values first to make room in slot1 for new result.
; Caller must write new result into history1/scoreHist1 AFTER this call.

UPDATE_HISTORY PROC

    ; Step 1: slot2 -> slot3  (preserve older entry before it gets overwritten)
    MOV AL, history2
    MOV history3, AL

    MOV AL, scoreHist2
    MOV scoreHist3, AL

    ; Step 2: slot1 -> slot2  (move last game result back one slot)
    MOV AL, history1
    MOV history2, AL

    MOV AL, scoreHist1
    MOV scoreHist2, AL

    ; slot1 is now free for caller to store the newest game result

    RET

UPDATE_HISTORY ENDP

; =========================================================
; DISPLAY HISTORY
; =========================================================

DISPLAY_HISTORY PROC

    PRINT historyTitle
    NEWLINE

    ; --- Slot 1 ---
    MOV DL, history1
    CMP DL, '-'
    JE SHOW_EMPTY1

    MOV AH, 02H
    INT 21H

    MOV DL, ' '
    MOV AH, 02H
    INT 21H

    MOV AL, scoreHist1
    CALL PRINT_NUMBER
    JMP DONE1

SHOW_EMPTY1:
    MOV AH, 02H
    INT 21H

DONE1:
    NEWLINE

    ; --- Slot 2 ---
    MOV DL, history2
    CMP DL, '-'
    JE SHOW_EMPTY2

    MOV AH, 02H
    INT 21H

    MOV DL, ' '
    MOV AH, 02H
    INT 21H

    MOV AL, scoreHist2
    CALL PRINT_NUMBER
    JMP DONE2

SHOW_EMPTY2:
    MOV AH, 02H
    INT 21H

DONE2:
    NEWLINE

    ; --- Slot 3 ---
    MOV DL, history3
    CMP DL, '-'
    JE SHOW_EMPTY3

    MOV AH, 02H
    INT 21H

    MOV DL, ' '
    MOV AH, 02H
    INT 21H

    MOV AL, scoreHist3
    CALL PRINT_NUMBER
    JMP DONE3

SHOW_EMPTY3:
    MOV AH, 02H
    INT 21H

DONE3:
    NEWLINE

    RET

DISPLAY_HISTORY ENDP

; =========================================================
; REPLAY SYSTEM
; =========================================================

REPLAY_GAME PROC

REPLAY_INPUT:

    NEWLINE

    PRINT replayMsg

    MOV AH, 01H
    INT 21H

    CMP AL, 'Y'
    JE PLAY_YES

    CMP AL, 'y'
    JE PLAY_YES

    CMP AL, 'N'
    JE PLAY_NO

    CMP AL, 'n'
    JE PLAY_NO

    NEWLINE
    PRINT invalidMsg

    JMP REPLAY_INPUT

PLAY_YES:

    MOV AL, 1
    RET

PLAY_NO:

    MOV AL, 0
    RET

REPLAY_GAME ENDP

; =========================================================
; PRINT NUMBER
; =========================================================

PRINT_NUMBER PROC

    MOV AH, 0

    MOV BL, 10
    DIV BL                  ; AL = tens digit, AH = ones digit

    CMP AL, 0
    JE PRINT_ONES           ; Skip leading zero

    ADD AL, 48

    MOV DL, AL
    MOV AH, 02H
    INT 21H

PRINT_ONES:

    MOV AL, AH
    ADD AL, 48

    MOV DL, AL
    MOV AH, 02H
    INT 21H

    RET

PRINT_NUMBER ENDP

END MAIN


