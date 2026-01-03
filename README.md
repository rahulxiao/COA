# Assembly Language Practice Solutions

This repository contains solutions to assembly language problems from the Final Term Practice Sheet. All solutions are written in **8086 assembly language** for a DOS environment.

---

## Table of Contents

- [Problems 1-10: Logic Conversions](#problems-1-10-logic-conversions)
    - [Problem 1](#problem-1)
    - [Problem 2](#problem-2)
    - [Problem 3](#problem-3)
    - [Problem 4](#problem-4)
    - [Problem 5](#problem-5)
    - [Problem 6](#problem-6)
    - [Problem 7](#problem-7)
    - [Problem 8](#problem-8)
    - [Problem 9](#problem-9)
    - [Problem 10](#problem-10)
- [Problems 11-14: Input/Output Programs](#problems-11-14-inputoutput-programs)
    - [Problem 11](#problem-11)
    - [Problem 12](#problem-12)
    - [Problem 13](#problem-13)
    - [Problem 14](#problem-14)
- [Problems 27-29: String and Bit Manipulation](#problems-27-29-string-and-bit-manipulation)
    - [Problem 27](#problem-27)
    - [Problem 28](#problem-28)
    - [Problem 29](#problem-29)

---

## Problems 1-10: Logic Conversions

These are code snippets demonstrating how to convert pseudocode logic into assembly.

### Problem 1
**Convert:**
IF AX < 0
THEN
PUT -1 IN BX
END IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AX, -5      ; Initial value for testing
    
    CMP AX, 0       ; Compare AX with 0
    JGE END_IF1     ; Jump if Greater or Equal (if AX is not less than 0)
    MOV BX, -1      ; If AX < 0, put -1 in BX

END_IF1:
    MOV AH, 4CH     ; Exit to DOS
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Since `AX` is -5 (negative), `BX` will contain `-1` (FFFFh).

### Problem 2
**Convert:**
IF AL < 0
THEN
   put FFh in AH
ELSE
   put 0 in AH
END_IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AL, -1      ; Initial value
    
    CMP AL, 0       ; Compare AL with 0
    JGE ELSE2       ; Jump if Greater or Equal
    MOV AH, 0FFh    ; THEN part: put FFh (-1) in AH
    JMP END_IF2     ; Skip the ELSE part
ELSE2:
    MOV AH, 0       ; ELSE part: put 0 in AH
END_IF2:

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> For `AL = -1`, `AH` will be `FFh`. If `AL` were positive, `AH` would be `0`.

### Problem 3
**Convert (DL contains ASCII code of a character):**
(IF DL >= "A") AND (DL <= 'Z')
THEN
   display DL
END_IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV DL, 'G'     ; Test character
    
    CMP DL, 'A'     ; Compare DL with 'A'
    JL  END_IF3     ; Jump if Less (not >= 'A')
    CMP DL, 'Z'     ; Compare DL with 'Z'
    JG  END_IF3     ; Jump if Greater (not <= 'Z')
    
    ; If both conditions are met, display the character
    MOV AH, 2       ; Function to display a character
    INT 21h         ; Call DOS interrupt

END_IF3:
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Displays the character `G` on the screen.

### Problem 4
**Convert (Nested IF-ELSE):**
IF AX < BX
THEN
   IF BX < CX
   THEN
      put 0 in AX
   ELSE
      put 0 in BX
   END_IF
END_IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AX, 10
    MOV BX, 20
    MOV CX, 30      ; AX < BX and BX < CX
    
    CMP AX, BX
    JGE END_IF4_OUTER
    
    ; Inner IF
    CMP BX, CX
    JGE ELSE4_INNER
    MOV AX, 0
    JMP END_IF4_INNER
ELSE4_INNER:
    MOV BX, 0
END_IF4_INNER:

END_IF4_OUTER:
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Since `10 < 20` and `20 < 30`, `AX` will be set to `0`.

### Problem 5
**Convert (OR condition):**
IF (AX < BX) OR (BX < CX)
THEN
   put 0 in DX
ELSE
   put 1 in DX
END_IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AX, 50
    MOV BX, 20
    MOV CX, 30      ; AX < BX is False (50 < 20), BX < CX is True (20 < 30)
    
    CMP AX, BX      ; Check first condition
    JL  THEN5       ; If AX < BX, jump to THEN part
    CMP BX, CX      ; Check second condition
    JL  THEN5       ; If BX < CX, jump to THEN part
    
    ; ELSE part
    MOV DX, 1       
    JMP END_IF5
THEN5:
    MOV DX, 0
END_IF5:

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Since `BX < CX` is true, the `OR` condition is satisfied, and `DX` will be `0`.

### Problem 6
**Convert (Chained IF-ELSE IF-ELSE):**
IF AX < BX
THEN
   put 0 in AX
ELSE
   IF BX < CX
   THEN
      put 0 in BX
   ELSE
      put 0 in CX
   END_IF
END_IF

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AX, 30
    MOV BX, 20
    MOV CX, 40      ; AX < BX is False, BX < CX is True
    
    CMP AX, BX
    JL  THEN6
    
    CMP BX, CX
    JL  ELSE_IF6
    
    MOV CX, 0
    JMP END_IF6

THEN6:
    MOV AX, 0
    JMP END_IF6

ELSE_IF6:
    MOV BX, 0
    
END_IF6:
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Since `BX < CX` (20 < 40), `BX` will be set to `0`.

### Problem 7
**Task:** Display "o" if AL contains 1 or 3; display "e" if AL contains 2 or 4.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV AL, 3       ; Test value
    
    CMP AL, 1
    JE  DISPLAY_O
    CMP AL, 3
    JE  DISPLAY_O
    CMP AL, 2
    JE  DISPLAY_E
    CMP AL, 4
    JE  DISPLAY_E
    JMP END_PROG7

DISPLAY_O:
    MOV DL, 'o'
    MOV AH, 2
    INT 21h
    JMP END_PROG7

DISPLAY_E:
    MOV DL, 'e'
    MOV AH, 2
    INT 21h

END_PROG7:
    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Displays `o` because `AL` is 3.

### Problem 8
**Task:** Print all ASCII codes from 0 to 255.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV CX, 256     ; Loop 256 times
    MOV DL, 0       ; Start with ASCII code 0
PRINT_LOOP:
    MOV AH, 2       ; Function to display a character
    INT 21h         ; Display character in DL
    INC DL          ; Move to the next ASCII code
    LOOP PRINT_LOOP

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> Displays all characters from the extended ASCII set (0-255).

### Problem 9
**Task:** Put the sum of the first 50 terms of the arithmetic sequence 1, 5, 9, 13, ... in DX.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV CX, 50      ; Number of terms
    MOV AX, 1       ; First term
    MOV DX, 0       ; Initialize sum to 0
SUM_LOOP:
    ADD DX, AX      ; Add current term to sum
    ADD AX, 4       ; Calculate next term (common difference = 4)
    LOOP SUM_LOOP

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> `DX` will contain the sum of the sequence. For 50 terms, the sum is `4950` (1356h).

### Problem 10
**Task:** Put the sum 100 + 95 + 90 + ... + 5 in AX.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    MOV CX, 20      ; Number of terms = 20
    MOV BX, 100     ; First term
    MOV AX, 0       ; Initialize sum to 0
SUM_LOOP10:
    ADD AX, BX      ; Add current term to sum
    SUB BX, 5       ; Calculate next term
    LOOP SUM_LOOP10

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```

**Output:**
> `AX` will contain the sum `1050` (041Ah).

Problems 11-14: Input/Output Programs
These are complete, runnable programs with user input and output.
### Problem 11
**Task:** Read a character and display it 50 times on the next line.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    ; Read a character
    MOV AH, 1
    INT 21h
    MOV BL, AL      ; Save the character in BL

    ; Move to the next line
    MOV AH, 2
    MOV DL, 10      ; Line feed
    INT 21h
    MOV DL, 13      ; Carriage return
    INT 21h

    ; Loop to print the character 50 times
    MOV CX, 50
PRINT_LOOP11:
    MOV AH, 2
    MOV DL, BL
    INT 21h
    LOOP PRINT_LOOP11

    ; Exit program
    MOV AH, 4CH
    INT 21h
MAIN ENDP
END MAIN
```

### Problem 12
**Task:** Display "?", read two capital letters, and display them in alphabetical order on the next line.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    ; Display '?'
    MOV AH, 2
    MOV DL, '?'
    INT 21h

    ; Read first character
    MOV AH, 1
    INT 21h
    MOV BL, AL

    ; Read second character
    MOV AH, 1
    INT 21h
    MOV BH, AL

    ; Move to next line
    MOV AH, 2
    MOV DL, 10
    INT 21h
    MOV DL, 13
    INT 21h

    ; Compare and swap if necessary
    CMP BL, BH
    JLE ORDER_OK
    XCHG BL, BH

ORDER_OK:
    ; Display first character
    MOV AH, 2
    MOV DL, BL
    INT 21h

    ; Display second character
    MOV DL, BH
    INT 21h

    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP
END MAIN
```

### Problem 13
**Task:** Take an input character, check if it is a letter, and toggle its case.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.CODE
MAIN PROC
    ; Read character
    MOV AH, 1
    INT 21h
    MOV BL, AL

    ; Check if uppercase ('A'-'Z')
    CMP BL, 'A'
    JL  CHECK_LOWER
    CMP BL, 'Z'
    JG  CHECK_LOWER
    ; Convert to lowercase
    OR BL, 20h
    JMP DISPLAY
    
CHECK_LOWER:
    ; Check if lowercase ('a'-'z')
    CMP BL, 'a'
    JL  DISPLAY
    CMP BL, 'z'
    JG  DISPLAY
    ; Convert to uppercase
    AND BL, 0DFh

DISPLAY:
    ; Move to next line
    MOV AH, 2
    MOV DL, 10
    INT 21h
    MOV DL, 13
    INT 21h
    ; Display converted character
    MOV DL, BL
    INT 21h

    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP
END MAIN
```

### Problem 14
**Task:** Take an input character, check if it is a number, and determine if it's odd or even using TEST.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.DATA
    MSG_ODD  DB 'The number is ODD.$'
    MSG_EVEN DB 'The number is EVEN.$'
    MSG_ERR  DB 'Not a valid number.$'
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    ; Read character
    MOV AH, 1
    INT 21h

    ; Move to next line
    MOV AH, 2
    MOV DL, 10
    INT 21h
    MOV DL, 13
    INT 21h

    ; Check if it is a number ('0'-'9')
    CMP AL, '0'
    JL  NOT_NUMBER
    CMP AL, '9'
    JG  NOT_NUMBER

    ; Convert from ASCII to decimal value
    SUB AL, '0'

    ; Test the least significant bit
    TEST AL, 1
    JZ   IS_EVEN
    
IS_ODD:
    LEA DX, MSG_ODD
    JMP SHOW_MSG

IS_EVEN:
    LEA DX, MSG_EVEN
    JMP SHOW_MSG
    
NOT_NUMBER:
    LEA DX, MSG_ERR
    
SHOW_MSG:
    MOV AH, 9
    INT 21h
    
    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP
END MAIN
```

Problems 27-29: String and Bit Manipulation
### Problem 27
**Task:** Take a binary string, reverse it, and count the '1's and '0's.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.DATA
    PROMPT1   DB 'Enter a binary number: $'
    PROMPT2   DB 0AH, 0DH, 'Reversed binary: $'
    PROMPT3   DB 0AH, 0DH, 'Number of 1s: $'
    PROMPT4   DB 0AH, 0DH, 'Number of 0s: $'
    BUFFER    DB 20, ?, 20 DUP('$')
    COUNT1    DW 0
    COUNT0    DW 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    ; Prompt for input
    LEA DX, PROMPT1
    MOV AH, 9
    INT 21h

    ; Read string from user
    LEA DX, BUFFER
    MOV AH, 0AH
    INT 21h
    
    ; Get length of input string
    MOV CL, [BUFFER+1]
    MOV CH, 0
    
    ; Setup pointer to end of string
    LEA SI, BUFFER+2
    ADD SI, CX
    DEC SI

    ; Print reverse prompt
    LEA DX, PROMPT2
    MOV AH, 9
    INT 21h
    
    ; Loop to reverse and count
REVERSE_LOOP:
    MOV DL, [SI]
    
    ; Print the character
    MOV AH, 2
    INT 21h
    
    ; Count 1s and 0s
    CMP DL, '1'
    JE  IS_ONE
    CMP DL, '0'
    JE  IS_ZERO
    JMP SKIP_COUNT

IS_ONE:
    INC COUNT1
    JMP SKIP_COUNT
IS_ZERO:
    INC COUNT0
    
SKIP_COUNT:
    DEC SI
    LOOP REVERSE_LOOP

    ; Print count of 1s
    LEA DX, PROMPT3
    MOV AH, 9
    INT 21h
    MOV AX, COUNT1
    CALL PRINT_NUM

    ; Print count of 0s
    LEA DX, PROMPT4
    MOV AH, 9
    INT 21h
    MOV AX, COUNT0
    CALL PRINT_NUM

    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP

; Procedure to print a number in AX
PRINT_NUM PROC
    MOV CX, 0
    MOV BX, 10
PRINT_NUM_LOOP:
    MOV DX, 0
    DIV BX
    PUSH DX
    INC CX
    CMP AX, 0
    JNE PRINT_NUM_LOOP
    
PRINT_NUM_POP:
    POP DX
    ADD DL, '0'
    MOV AH, 2
    INT 21h
    LOOP PRINT_NUM_POP
    RET
PRINT_NUM ENDP

END MAIN
```

### Problem 28
**Task:** Take input of a 3-character hex password. Validate it and re-prompt on error.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.DATA
    PROMPT    DB 'Enter a 3-character (hex) password: $'
    MSG_ERR   DB 0AH, 0DH, 'Invalid input!$'
    MSG_OK    DB 0AH, 0DH, 'Password accepted.$'
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

RE_ENTER:
    ; Print prompt
    LEA DX, PROMPT
    MOV AH, 9
    INT 21h
    
    ; Read and validate 3 characters
    MOV CX, 3
    
CHECK_LOOP:
    ; Read one character
    MOV AH, 1
    INT 21h
    
    ; Validate: Check if digit '0'-'9'
    CMP AL, '0'
    JL  INVALID
    CMP AL, '9'
    JLE VALID_CHAR
    ; Check if 'A'-'F'
    CMP AL, 'A'
    JL  INVALID
    CMP AL, 'F'
    JLE VALID_CHAR
    ; Check if 'a'-'f'
    CMP AL, 'a'
    JL  INVALID
    CMP AL, 'f'
    JG  INVALID

VALID_CHAR:
    LOOP CHECK_LOOP
    JMP ACCEPTED

INVALID:
    LEA DX, MSG_ERR
    MOV AH, 9
    INT 21h
    JMP RE_ENTER

ACCEPTED:
    LEA DX, MSG_OK
    MOV AH, 9
    INT 21h

    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP
END MAIN
```

### Problem 29
**Task:** Take binary input (max 8 digits), count the '1' bits, print the count, and state if it's ODD or EVEN.

**Solution:**
```assembly
.MODEL SMALL
.STACK 100H
.DATA
    PROMPT1   DB 'USER INPUT: $'
    PROMPT2   DB 0AH, 0DH, 'NUMBER OF 1 BITS: $'
    PROMPT3   DB 0AH, 0DH, 'OUTPUT: $'
    MSG_ODD   DB 'ODD$'
    MSG_EVEN  DB 'EVEN$'
    BUFFER    DB 10, ?, 10 DUP('$')
    ONE_COUNT DW 0
.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    ; Prompt for input
    LEA DX, PROMPT1
    MOV AH, 9
    INT 21h

    ; Read string
    LEA DX, BUFFER
    MOV AH, 0AH
    INT 21h
    
    ; Setup loop to count 1s
    MOV CL, [BUFFER+1]
    MOV CH, 0
    LEA SI, BUFFER+2

COUNT_LOOP:
    CMP BYTE PTR [SI], '1'
    JNE SKIP_INC
    INC ONE_COUNT
SKIP_INC:
    INC SI
    LOOP COUNT_LOOP

    ; Print count of 1s
    LEA DX, PROMPT2
    MOV AH, 9
    INT 21h
    MOV AX, ONE_COUNT
    CALL PRINT_NUM

    ; Print ODD/EVEN output
    LEA DX, PROMPT3
    MOV AH, 9
    INT 21h
    
    ; Check if count is odd or even
    MOV AX, ONE_COUNT
    TEST AX, 1
    JZ  IS_EVEN
    
IS_ODD:
    LEA DX, MSG_ODD
    JMP SHOW_RESULT

IS_EVEN:
    LEA DX, MSG_EVEN

SHOW_RESULT:
    MOV AH, 9
    INT 21h

    ; Exit
    MOV AH, 4CH
    INT 21h
MAIN ENDP

; Procedure to print a number in AX
PRINT_NUM PROC
    MOV CX, 0
    MOV BX, 10
PRINT_NUM_LOOP:
    MOV DX, 0
    DIV BX
    PUSH DX
    INC CX
    CMP AX, 0
    JNE PRINT_NUM_LOOP
    
PRINT_NUM_POP:
    POP DX
    ADD DL, '0'
    MOV AH, 2
    INT 21h
    LOOP PRINT_NUM_POP
    RET
PRINT_NUM ENDP

END MAIN
```

### How to Run

1. Use an assembler like MASM or TASM
2. Assemble: `masm filename.asm`
3. Link: `link filename.obj`
4. Run in DOSBox or a DOS environment: `filename.exe`

### License
These solutions are provided for educational purposes.
