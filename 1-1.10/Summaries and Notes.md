# Summary and Notes: CS:APP (Chapter 1)

> **Quick Note**: Practical exercises and source code are located in the `hello/` subdirectory.

## 1.1 Information is Bits + Context
All information in a computer system—files on disk, programs in memory, or data on a network—is represented as a **sequence of bits**.

* **Bytes**: Bits are organized into 8-bit chunks called bytes.
* **ASCII**: Most systems represent text characters using the ASCII standard, where each character has a unique byte-sized integer value (e.g., `#` is 35, `i` is 105).
* **The Invisible `\n`**: Even the newline character is stored as a byte (Value 10 in ASCII).
* **The Power of Context**: The only thing that distinguishes different data objects (integers, strings, instructions) is the **context** in which we view them.
* **Approximations**: Machine representations of numbers are finite approximations; they can behave in unexpected ways (like overflows), which is the core of Chapter 2.


## 1.2 Programs are Translated by Others into Different Forms
To run `hello.c`, the high-level C code must be translated into a sequence of low-level **machine-language instructions**. This is done by a **compiler driver**.

### The Compilation System
The translation happens in four distinct phases:

| Phase | Tool | Process | Result |
| :--- | :--- | :--- | :--- |
| **Preprocessing** | `cpp` | Handles directives (`#`). Injects header content (like `stdio.h`) directly into the file. | `hello.i` (Expanded C) |
| **Compilation** | `cc1` | Translates the high-level C code into **Assembly language**. | `hello.s` (Text ASM) |
| **Assembly** | `as` | Translates ASM into machine-language instructions (binary). | `hello.o` / `hello.obj` |
| **Linking** | `ld` | Merges your object file with pre-compiled library objects (like `printf.o`). | `hello.exe` (Executable) |


## 1.3 Practical Analysis: Assembly (x86-64 Windows)
Using the Zig toolchain (`zig cc -S hello.c`), we can inspect the generated assembly. While the raw output contains many directives for the OS (like `.seh`), the core logic looks like this:

```assembly
main:                  # Program Entry Point
    pushq   %rbp       # Save the base pointer (Setup)
    subq    $32, %rsp  # Reserve 32 bytes in the stack (Allocation)
    
    callq   __main     # Initialize the C Runtime Environment
    
    # --- Logic (printf, etc.) would go here ---
    
    addq    $32, %rsp  # Free reserved stack memory (Cleanup)
    popq    %rbp       # Restore the original base pointer
    retq               # Return control to the Operating System
```

> **Key Takeaway**: Assembly reveals the "Symmetry of the Stack". Notice how the `subq` (requesting memory) is perfectly balanced by the `addq` (releasing memory) at the end. This is fundamental for memory safety in Embedded Systems.