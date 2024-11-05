# GDB Baby Step 1
## Problem
Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.
## Thought Process
- Since the title suggested that it would require the GNU Debugger to solve this level, I did some research to understand how the GNU Debugger works
- GDB allows us to dissect a program while it is running, providing valuable insight into behaviour, performance and finding chinks in the armor of the system.
- GDB plays a significant role in reverse engineering systems on the x86_64 architecture, the most common in modern computing. GDB allows you to step through the assembly code, examine registers, observe stack changes, and much more, all of which are essential in gaining a thorough understanding of the software’s functionality and structure​​.
- Following are some commands used by GDB:
  1. gdb: Start GDB without a file.
  2. gdb filename: Start GDB with a file.
  4. run: Run the program, it can also run the program with arguments.
  5. break: set a breakpoint at a location
  6. info breakpoints: lists all breakpoints
  7. next: Run the program until the following line.
  8. step: Step into function calls.
  9. print: prints the value of a variable
  10. display: display the value continually
  11. list: List the source code.
  12. list function: List the source code of a function.
  13. list line: List the source code around a specific line.
  14. disassemble function: Disassemble the code of a function
  15. disassemble/r function: Disassemble the code of a function with raw opcodes
- To start solving, I first checked the permissions of the file and tried running it.
```console
# ls -la debugger0_a
-rwxrwxrwx 1 root root 16472 Nov  5 10:50 debugger0_a
# ./debugger0_a
```
- Since nothing seemed to happen, I ran `gdb debugger0_a`
- since we need the `eax` register at the end of `main`, i tried to run `disassemble main`
```gdb
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
```
- We found `0x86342` in the register
- We convert it to int and find `549698`
- so the picoCTF flag is : `picoCTF{549698}`
# ARMssembly 1
## Problem
For what argument does this program print `win` with variables 83, 0 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})
## Thought Process
- Since it is in assembly, I tried `gdb` and `gcc`, thinking it would assist, but it failed.
- So, i thought i will try out `cat chall_1.S` to read the contents of the file
```arm
        .arch armv8-a
        .file   "chall_1.c"
        .text
        .align  2
        .global func
        .type   func, %function
func:
        sub     sp, sp, #32
        str     w0, [sp, 12]
        mov     w0, 83
        str     w0, [sp, 16]
        str     wzr, [sp, 20]
        mov     w0, 3
        str     w0, [sp, 24]
        ldr     w0, [sp, 20]
        ldr     w1, [sp, 16]
        lsl     w0, w1, w0
        str     w0, [sp, 28]
        ldr     w1, [sp, 28]
        ldr     w0, [sp, 24]
        sdiv    w0, w1, w0
        str     w0, [sp, 28]
        ldr     w1, [sp, 28]
        ldr     w0, [sp, 12]
        sub     w0, w1, w0
        str     w0, [sp, 28]
        ldr     w0, [sp, 28]
        add     sp, sp, 32
        ret
        .size   func, .-func
        .section        .rodata
        .align  3
.LC0:
        .string "You win!"
        .align  3
.LC1:
        .string "You Lose :("
        .text
        .align  2
        .global main
        .type   main, %function
main:
        stp     x29, x30, [sp, -48]!
        add     x29, sp, 0
        str     w0, [x29, 28]
        str     x1, [x29, 16]
        ldr     x0, [x29, 16]
        add     x0, x0, 8
        ldr     x0, [x0]
        bl      atoi
        str     w0, [x29, 44]
        ldr     w0, [x29, 44]
        bl      func
        cmp     w0, 0
        bne     .L4
        adrp    x0, .LC0
        add     x0, x0, :lo12:.LC0
        bl      puts
        b       .L6
.L4:
        adrp    x0, .LC1
        add     x0, x0, :lo12:.LC1
        bl      puts
.L6:
        nop
        ldp     x29, x30, [sp], 48
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
        .section        .note.GNU-stack,"",@progbits
```
- I attempted to make some sense out of this, but i did not understand any of it except the fact that 2 functions called `main` and `func` existed since I have no prior knowledge of Assembly code.
- Following some research about ARM Assembly Code, these were my findings:
- In ARM assembly language, `LDR` is an instruction that loads data from memory into a register. It stands for "Load Register."
- `sub` allocates bytes to the stack
- `str` stores a value at an address
-  `mov` sets a value
- with this knowledge, i tried understanding the code
- `main` function takes a command line argument, which gets operated upon in `func`. The win condition is to get 0 as output.
- from `func`, we get that the function argument is stored in `w0` at `[sp, 12]`, `83` is stored at `[sp, 16]`, `0` (zero register using wzr) is stored at `[sp, 20]` and `3` at `[sp, 24]`
- coming to the operations side,
```arm
    ldr     w0, [sp, 20]         // Load 0 from [sp, 20] into w0
    ldr     w1, [sp, 16]         // Load 83 from [sp, 16] into w1
    lsl     w0, w1, w0           // Left shift 83 by 0 bits, so w0 remains 83
    str     w0, [sp, 28]         // Store 83 at [sp, 28]

    ldr     w1, [sp, 28]         // Load 83 from [sp, 28] into w1
    ldr     w0, [sp, 24]         // Load 3 from [sp, 24] into w0
    sdiv    w0, w1, w0           // Divide 83 by 3, result is 27
    str     w0, [sp, 28]         // Store 27 at [sp, 28]

    ldr     w1, [sp, 28]         // Load 27 from [sp, 28] into w1
    ldr     w0, [sp, 12]         // Load input from [sp, 12] into w0
    sub     w0, w1, w0           // Subtract input from 27, result in w0
    str     w0, [sp, 28]         // Store result at [sp, 28]
```
