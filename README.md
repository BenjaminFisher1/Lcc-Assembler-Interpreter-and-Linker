# Lcc-Assembler-Interpreter-and-Linker
Three projects dealing with LCC assembly, translating assembly language to machine code, encode assembly source files and execute them, and link multiple object files into a single executable file. 
Based off of program shells provided in the [textbook](https://a.co/d/bN9Kix4) "C and C++ Under the Hood: 2nd Edition" by Anthony J. Dos Reis

## **[Assembler](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/assembler.c):**
This program is a two-pass assembler for the LCC instruction set.

- Pass 1: reads the input assembly file, builds a symbol table (labels -> addresses),
  and computes location counters (handles .zero to advance loc_ctr).
  
- Pass 2: re-parses the file, translates mnemonics and operands into 16-bit
  machine words (writes binary words to an output file with ".e" extension).

Supported mnemonics include:
  - br*/branch variants
  - add
  - and
  - not
  - ld
  - st
  - ldr
  - str
  - bl
  - blr
  - jmp
  - ret
  - lea
  - halt
  - nl
  - dout
  - .word
  - .zero
  
Utility functions:
- **mystrcmpi/mystrncmpi:** case-insensitive string compares.
- **isreg/getreg:** detect and parse register operands (r0..r7).
- **getadd:** lookup label addresses in the symbol table.

Errors are reported with line number and the original source line.
### Usage:
_Make sure gcc is installed._

Compile assembler: gcc assembler.c -o assembler

Run Programs: ./assembler yourProgram.a

## **[Interpreter](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/interpreter.c):**
This program is a small LCC interpreter.
- Opens a binary lcc file, checks for the two-byte header 'o' 'C', and loads up to 65536 16-bit words into mem[].
- Uses registers r[0..7] and a program counter pc. r[7] is used as the link register.
- Runs a fetch-decode-execute loop: fetches ir = mem[pc++], extracts instruction fields (opcode, pcoffset9, pcoffset11, imm5, offset6, eopcode, trapvec, register fields, and bit flags).
  
- Decodes opcode with a switch and executes implemented instructions:
  -   branches (case 0)
  -   add (1)
  -   ld (2)
  -   st (3)
  -   jsr/bl/blr (4)
  -   and (5)
  -   ldr/str (6/7)
  -   not (9)
  -   jmp/ret (12)
  -   lea (14)
  -   trap (15) which handles halt (exit)
  -   nl (newline)
  -   dout (print register value).

  
- Maintains condition flags n (Negative) and z (Zero) via setnz(), and carry/overflow via setcv() called by arithmetic operations.
- Memory accesses are direct indexing into mem[] or via base+offset addressing for ldr/str.
- The interpreter loops until a trap/halt causes exit or a file error occurs.

### Usage:
_Ensure gcc is installed_

Compile interpreter: gcc interpreter.c -o interpreter

Run Programs: ./interpreter programName.e

## **[Linker](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/linker.c):**
This program is a simple two-pass linker for object modules.
- Reads one or more object (.o) modules passed on the command line, parses their headers (entries: S start, G global defs, E external refs, e external refs with different bit fields, V relocatable values, A absolute relocations)
- Collects and adjusts addresses
- Resolves external references against global definitions
- Applies relocations
- Writes a single executable file "link.e" in the same object file format (with header entries and machine code).
- Performs basic error checking for undefined references and multiple definitions.

### Usage: 
_Make sure gcc is installed._

gcc linker.c -o linker

linker moduleName1 moduleName2 etc..
