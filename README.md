# Lcc-Assembler-Interpreter-and-Linker
Three projects dealing with LCC assembly, translating assembly language to machine code, encode assembly source files to executables, and link multiple object files into a single executable file. 
Based off of program shells provided in the [textbook](https://a.co/d/bN9Kix4) "C and C++ Under the Hood: 2nd Edition" by Anthony J. Dos Reis

## **[Assembler](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/assembler.c):**
This program is a two-pass assembler for the LCC instruction set.
- Pass 1: reads the input assembly file, builds a symbol table (labels -> addresses),
  and computes location counters (handles .zero to advance loc_ctr).
- Pass 2: re-parses the file, translates mnemonics and operands into 16-bit
  machine words (writes binary words to an output file with ".e" extension).
Supported mnemonics/directives include:
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
- ** getadd:** lookup label addresses in the symbol table.

Errors are reported with line number and the original source line.
#### Usage:
_Make sure gcc is installed. _

Compile assembler: gcc assembler.c -o assembler

Run Programs: ./assembler yourProgram.a

## **Interpreter:**

## **[Linker](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/linker.c):**
This program is a simple two-pass linker for object modules.
- Reads one or more object (.o) modules passed on the command line, parses their headers (entries: S start, G global defs, E external refs, e external refs with different bit fields, V relocatable values, A absolute relocations)
- Collects and adjusts addresses
- Resolves external references against global definitions
- Applies relocations
- Writes a single executable file "link.e" in the same object file format (with header entries and machine code).
- Performs basic error checking for undefined references and multiple definitions.

#### Usage: l <obj module name1> <obj module name2> ...
