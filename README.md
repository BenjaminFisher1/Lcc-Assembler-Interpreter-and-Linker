# Lcc-Assembler-Interpreter-and-Linker
## Overview

Three projects dealing with LCC assembly: translating assembly language to machine code, encoding assembly source files and executing them, and linking multiple object files into a single executable file.

Based on program shells provided in the [textbook](https://a.co/d/bN9Kix4) *"C and C++ Under the Hood: 2nd Edition"* by Anthony J. Dos Reis.

### Workflow

1. Compile assembly code using **Assembler**
2. *(Optional)* Link multiple modules using **Linker**
3. Execute program using **Interpreter**

---

## [Assembler](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/assembler.c)

Two-pass assembler for the LCC instruction set.

**Pass 1:** Reads input assembly file, builds symbol table (labels → addresses), computes location counters.

**Pass 2:** Re-parses file, translates mnemonics and operands into 16-bit machine words (outputs ".e" extension).

**Supported Mnemonics:** `br*` · `add` · `and` · `not` · `ld` · `st` · `ldr` · `str` · `bl` · `blr` · `jmp` · `ret` · `lea` · `halt` · `nl` · `dout` · `.word` · `.zero`

**Utilities:** `mystrcmpi/mystrncmpi` (case-insensitive compare) · `isreg/getreg` (register parsing) · `getadd` (symbol lookup)

Errors include line number and source context.

### Compile & Use
```bash
gcc assembler.c -o assembler
./assembler yourProgram.a
```

---

## [Interpreter](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/interpreter.c)

LCC virtual machine interpreter executing binary object files.

**Core:** Memory (65536 16-bit words), registers r0–r7 (r7 = link register), program counter.

**Execution:** Fetch-decode-execute loop with instruction decoding via opcode switch.

**Instructions:** Branches · Arithmetic (add, and, not) · Memory (ld, st, ldr, str) · Control (jsr, jmp, ret, lea) · I/O (nl, dout) · Halt

**Features:** Condition flags (n, z), carry/overflow tracking, base+offset addressing.

### Compile & Use
```bash
gcc interpreter.c -o interpreter
./interpreter programName.e
```

---

## [Linker](https://github.com/BenjaminFisher1/Lcc-Assembler-Interpreter-and-Linker/blob/main/linker.c)

Two-pass linker combining multiple object modules into executables.

**Operations:** Parse headers (S, G, E, e, V, A entries) · Collect and adjust addresses · Resolve external references · Apply relocations · Write "link.e"

**Error Checking:** Undefined references, duplicate definitions.

### Compile & Use
```bash
gcc linker.c -o linker
./linker moduleName1 moduleName2 ...
```

