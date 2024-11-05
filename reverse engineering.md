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
```
# ls -la debugger0_a
-rwxrwxrwx 1 root root 16472 Nov  5 10:50 debugger0_a
# ./debugger0_a
```
- Since nothing seemed to happen, I ran `gdb debugger0_a`
- since we need the `eax` register at the end of `main`, i tried to run `disassemble main`
```
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
