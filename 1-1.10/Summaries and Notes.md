# Summary and notes (written as I read. Needed to crystalize my memory)
> **Quick Note**: The practical reference is hello.c, inside the folder of the same name.
##1.1
The source program or source file of a program is a sequence of bits (aka binary) organized in 8-bit chuncks which are called bytes.

Most computer systems use ASCII, which represents each different character by the value of a byte-sized integer value.

Since hello.c and any other source program/file is stored as a sequence of bytes, each character written has it's own byte value stored.

Something interesting is that even "\n" (new line) has its own ASCII value, represented by the value 10.

The important thing to catch here, is understanding that everything is converted in some way or another, into bits and bytes.

It is important to understand that everything done in this format are representations of numbers made into 1s and 0s that at the end of the day (or line), are simply
approximations of real numbers that may (or may not) behave in weird or unexpected ways.

##1.2
A written program in C, always starts as High-Level since it's readable by humans. But, computers can't understand well this, so what does it do?
It converts our C code into a readable (mostly for them) sequence of low-level instructions. This instructions are then, converted into a executable program and stored
as binary inside the disk.

The translation of the program is made by a compiler driver:
hello.c -> Preprocessor (cpp) -> hello.i -> Compiler (cc1) -> hello.s -> Assembler (as) -> hello.o (Linux) or .obj (Windows) -> Linker (ld) which grabs printf from the header-> hello.exe

zig cc hello.c -o hello.exe

| Phase | What happens |
| :--- | :--- |
| `Preprocessing phase` | The preprocessor checks for headers (The "#" in the #include <stdio.h>), which tells it to go and grab whatever is in there and add it to the main program. Which results in a bigger C program, generally with the suffix .i |
| `Compilation phase` | The compiler translates de .i file into a .s, which stores the low-level instructions in ASM (Assembly language). |
| `Assembly phase` | It grabs the .s and makes it a .o file in Linux or a .obj file for Windows. If you wonder how does it store everything, well, it's just machine language, if you were to put it in a text editor, it would be pure gibberish. |
| `Linking phase` | If you notice, the hello.c program calls the printf function, which is part of the standard C library and it's its own objetc (.o/.obj). The job of the linker is to merge both objects and convert them into an executable. |

```main:                  # Where the program starts
    pushq   %rbp       # Saves the pointer from the base
    subq    $32, %rsp  # Reserves 32 bytes in the stack
    
    callq   __main     # Calls the start of the system
    
	>The section where the logic is (printf and etc.)
    
    addq    $32, %rsp  # Frees the memory that was reserved
    popq    %rbp       # Restores the original pointer
    retq               # Returns to the OS```
	
>This is the simplified assembly for x86-64 for Windows, in the Intel/AMD dialect.