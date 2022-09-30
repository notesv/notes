# Unit 1 : Assembler

Two Passes (Passes : the times the input file is processed)

Source Program -> Front End -> Intermediate Phase -> Back end -> Output

### Two Types of Error:

- Syntactic
- Semantic

## Assemby Language Statments

- Imperative Statments
    - Indicate an action to be performed during the execution
    - Mnemonics

- Declarative Statments
    - ```[label] DS <constant>```

- Assembler Directives
    - ```START <constant>```
    - ```END [<operand spec>]```

#### Forward Referencing and Back Patching

- Constant and Literals

    - The location of literal can't be specified
    - Literals starts with `=`
        - for e.g ```ADD AREG, ='5'```
- [Literals](https://qr.ae/pvbGUd)

#### Location Counter

- Initialized to 0

#### Hypothetical Representation

```Line NO) (Displacement) [ + ] OP_CODE OPERAND (REGISTER) MEMORY_SPACE```

For e.g :

``` 101) + 09 0 113 ```

## Advanced Assembler Directives

- Used for Location Counter Manipulations
- Assembler Directives don't change the Location Counter

### ORIGIN

New Value given to the Location Counter Reflected in the Next line
for e.g

```asm
200 ) ORIGIN  203
203 ) C DC 10
204 ) STOP
```

### EQU

```asm
Symbol EQU <address specification>

; Operand Specification:

    BACK EQU LOOP

; Constant:

    BACK EQU 200
```

### LTORG

Allows programmer to specify where literal should be stored

### Second Pass Happens on the intermediate representation

## First Pass

- Scan Program File
- Find all labels and calculate the corresponding address
    this is called the symbol table
[[Idea]]
## Mnemocnic Operation Codes

Instruction Code | Assembly Mnemonic | Remarks
-----------------| ------------------ | --------
00 | STOP | stops execution
01 | ADD | First Operand is Modified
02 | SUB | First Operand is Modified
03 | MULT | First Operand is Modified
04 | MOVER | Register <- Memory Move
05 | MOVEM | Memory <- Register Move
06 | COMP | Sets Condition Code
07 | BC | Branch on Condition
08 | DIV | Analogous to SUB
09 | READ | Performs Reading and Printing
10 | PRINT | Performs Reading and Printing


## Code for Declaration Statements and Directives


### Declarative Statments

 DC | 01
-----------------| -----------------
DS | 02

### Assembler Directives

 START | 01
-----------------| -----------------
END | 02
ORIGIN | 03
EQU | 04
LTORG | 05

### Registers

 AREG | 01
-----------------| -----------------
BREG | 02
CREG | 03
DREG | 04

### Syntax for BC

 LT | 01
-----------------| -----------------
LE | 02
EQ | 03
GT | 04
GE | 05
ANY| 06

## Tasks of two pass Assembler

### Pass 1

- Separate the symbol, mnemonic opcode and operand fields.
- Build the symbol table, literal Table, pool table
- Perform LC Processing
- Construct Intermediate representation

### Pass 2

- Synthesize the target program
- Evaluate Fields and generate code
- Process pseudo opcodes

#### Back Patching

Assigning the Address of the Symbols after re-visiting and then re-assigning the values in the Symbol Table.

## Data Sturctures of Assembler (PASS 1)

- OPTAB (Mnemonics Table)

| Mnemonic Opcode | Class | Mnemonic Info |
| ----------------| ------| ------------- |
| MOVER           | IS    | (04, 1)       |
| DS              | DL    | R#7           |
| START           | AD    | R#11          |

- SYMTAB (Symbol Table)

| Symbol  | Address | Length |
| --------|---------| ------ |
| LOOP    | 202     | 1      |
| NEXT    | 214     | 1      |
| LAST    | 216     | 1      |
| A       | 217     | 1      |
| BACK    | 202     | 1      |
| B       | 218     | 1      |

- LITTAB (Literal Table)

| Literal | Address |
| --------|---------|
| ='5'    |         |
| ='1'    |         |
| ='1'    |         |

- POOLTAB (Pool Table)

| Literal Number|
| --------|
| #1    |
| #3    |

#### Literal Table and Symbol Table values are assigned with back tracking

# Intermediate Code Representation


|(Class, OP_CODE) | `(REGISTER/BRANCHING_CONDITION)` | (CONSTANT/ SYMBOL, VALUE)
| --------|---------| ------- |

### For e.g

```asm
(AD, 01) (C, 200)

(IS, 09) A

(IS, 04) (1)(S, 01)
.
.
.
```


# Memory Requirements using Variant 1 (A) VS Variant 2 (B)

## (A)

| Pass 1 |
| --------|
| Data Structures |
| Work Area |

| Pass 2 |
| --------|
| --------|
| Data Structures |
| Work Area |

## (B)

| Pass 1 |
| --------|
| Data Structures |
| Work Area |
| --------- |

| Pass 2 |
| --------|
| Data Structures |
| Work Area |
| --------- |

# First Pass of two pass Assembler

- Input
    - Mnemonics Opcode Table (MOT)
    - Copy of Source Program (.ASM File)

- Output
    - Location Counter
    - OPTAB
    - SYMTAB
    - LITTAB
    - POOLTAB

--------------------------------------------------------------------------------------------------------------------

# Unit 2 : MacroProcessors Loaders and Linker

 - Pre-processor for assembler

## Macro Definition

- Header

    ```MACRO SAMPLE A, B, C```

- Body

    ```
    PRINT A
    PRINT B
    PRINT C
    .
    .
    .
     ```
- Footer

    ```MEND SAMPLE```

- Macro Call

## Types of Macros

- Simple Macro
- Macro with parameters
- Nested Macro
    - Macro call withing a macro Definition.

## Data Structures required:

- Macro Name Table ( MNT )
- Macro Definition Table ( MDT )

## Modified MNT & MDT

Name Of Macro | No. of Parameters  | Starting Index |
-- | - | -
SAMPLE1 | 0 | 1
SAMPLE2 | 0 | 4
SAMPLE3 | 0 | 7


1 | LOAD A |
-- | - |
2 | ADD B
3 | MEND
4 | LOAD X
5 | SUB Y
6 | MEND
7 | LOAD P
8 | DIV Q
9 | MEND

## Macros with Parameters

Two Types of arguments

- Formal
- Actual
