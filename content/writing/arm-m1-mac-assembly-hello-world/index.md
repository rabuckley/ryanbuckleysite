---
title: "Hello World in ARM assembly on Apple Silicon"
description: "How to write a 'Hello, World!' program in ARM assembly for Apple Silicon Macs"
categories: ["Computers"]
author: Ryan Buckley
date: 2025-05-31T11:44:37+01:00
draft: false
---

I believe that knowing the basics of the level of abstraction beneath your software stack is a useful way to understand how and why your stack works. I also just find low-level control of hardware interesting and fun.

For those reasons, I wanted to write a simple "Hello, World!" program in assembly. I've read more x64 assembly than ARM so I thought I'd try writing ARM assembly to run on my M1 Mac.

The general form of an ARM assembly instruction is `mnemonic destination, source1, source2`, where

- `mnemonic` is a shorthand for the instruction to execute,
- `destination` is where the result of the instruction will be stored
- the source operands are the values to be used in the instruction.

For example, to add two numbers, you'd write,

```asm
add X0, #5, #10
```

This adds the immediate values `5` and `10`, and stores the result in register `X0`. Immediate values are prefixed with `#` to distinguish them from register names, which are [generally prefixed](https://developer.arm.com/documentation/102374/0102/Registers-in-AArch64---general-purpose-registers) with `W` for 32-bit registers and `X` for 64-bit registers.

(Note, the syntax highlighting treats immediate values as comments, but they're not!)

To add two registers, `X1` and `X2`, into `X0` you'd write,

```asm
add X0, X1, X2
```

## Hello, World!

On Unix-like systems, "[everything is a file](https://en.wikipedia.org/wiki/Everything_is_a_file)" and MacOS is no exception. That means writing to standard output is just writing to [file descriptor 1](https://en.wikipedia.org/wiki/File_descriptor). The [syscall we need](https://github.com/apple-opensource/xnu/blob/4f43d4276fc6a87f2461a3ab18287e4a2e5a1cc0/bsd/kern/syscalls.master#L48) to use to write into the file descriptor is,

```
4	AUE_NULL	ALL	{ user_ssize_t write(int fd, user_addr_t cbuf, user_size_t nbyte); }
```

To invoke it, we need to set up the registers,

- `X0` with the file descriptor (1 for standard output)
- `X1` with the memory address of the string to write
- `X2` with the length of the string
- `X16` with the syscall number 4 for `write`

The code to do that is,

```asm
_print:
  mov X0, #1 // Move file descriptor 1 (stdout) into X0
  adr X1, message // Load the address of the message into X1
  mov X2, #message_length // Move the length of the message into X2
  mov X16, #4 // Move 4 (syscall number for write) into X16
  svc 0 // Execute software interrupt to invoke the syscall

message: .ascii "Hello, World!\n"
message_length = . - message
```

The `message` [label](https://developer.arm.com/documentation/100069/0611/Symbols--Literals--Expressions--and-Operators/Labels) points to the memory storing the ASCII string "Hello, World!\n". The `message_length` is calculated as the address of the end of the message subtract the address of the start of the message, which gives the length in bytes, so if the message changes, the length will automatically update.

That's the majority of the code. The remainder is declaring the entry point and terminating the program.

To declare the entry point, we use the `.global` directive with the label for the entry point, `_main`.

```asm
.global _main // Declare the entry point
.align 2  // Align to a 2-byte boundary

_main:
_print:
  mov X0, #1
  adr X1, message
  mov X2, #message_length
  mov X16, #4
  svc 0

message: .ascii "Hello, World!\n"
message_length = . - message
```

Note, labels are not like functions that return once finished. They 'fall through', meaning that if you don't jump to another label, the next instruction will be executed. This is why we can write `_main:` and then immediately `_print:` without needing to jump to it. For more complex control flow, you can use [subroutines and `bl`](https://developer.arm.com/documentation/ddi0596/2020-12/Base-Instructions/BL--Branch-with-Link-).

To terminate the program, we need to call the [`exit` syscall](https://github.com/apple-opensource/xnu/blob/4f43d4276fc6a87f2461a3ab18287e4a2e5a1cc0/bsd/kern/syscalls.master#L45) in much the same way as we called `write`. It's defined as,

```
1	AUE_EXIT	ALL	{ void exit(int rval) NO_SYSCALL_STUB; }
```

Which requires the registers set up as follows:

```asm
_terminate:
  mov X0, #0 // Move 0 into X0 to indicate success
  mov X16, #1 // Move 1 (syscall number for exit) into X16
  svc 0 // Execute software interrupt to invoke the syscall
```

Putting the whole program together,

```asm
.global _main // Declare the entry point
.align 2  // Align to a 2-byte boundary

_main:
_print:
  mov X0, #1
  adr X1, message
  mov X2, #message_length
  mov X16, #4
  svc 0

_terminate:
  mov X0, #0
  mov X16, #1
  svc 0

message: .ascii "Hello, World!\n"
message_length = . - message
```

The easiest way to compile and link the code is using `clang`,

```bash
clang -o hello_world hello_world.s -arch arm64
```

Alternatively, you can use `as` and `ld` directly,

```bash
as -o hello_world.o hello_world.s -arch arm64
ld hello_world.o -o hello_world -l System -syslibroot `xcrun -sdk macosx --show-sdk-path`
```

Then run the program with,

```bash
./hello_world
```

All being well, "Hello, World!" should be printed to the terminal.
