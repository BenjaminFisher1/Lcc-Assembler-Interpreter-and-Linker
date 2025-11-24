# Lcc-Assembler-Interpreter-and-Linker
Three projects dealing with LCC assembly, translating assembly language to machine code, encode assembly source files to executables, and link multiple object files into a single executable file. 
Based off of program shells provided in the [textbook](https://a.co/d/bN9Kix4)

## **Assembler:**

## **Interpreter:**

## **[Linker](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/linker.c):**
- Simple two-pass linker for custom object modules.
- Reads one or more object (.o) modules passed on the command line, parses their headers (entries: S start, G global defs, E external refs, e external refs with different bit fields, V relocatable values, A absolute relocations)
- Collects and adjusts addresses
- Resolves external references against global definitions
- Applies relocations
- Writes a single executable file "link.e" in the same object file format (with header entries and machine code).
- Performs basic error checking for undefined references and multiple definitions.

#### Usage: l <obj module name1> <obj module name2> ...
