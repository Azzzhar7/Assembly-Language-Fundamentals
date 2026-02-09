# 8086 HELLO WORLD - VGA Video Memory Demo

## Overview
This project demonstrates how to display the text **"HELLO WORLD"** on the screen using **direct VGA text-mode video memory** in 8086 assembly.  
Instead of using DOS print functions, it writes ASCII characters and color attributes directly into memory.

- **Programming Language:** x86 Assembly (8086)
- **Emulator:** [Emu8086](https://www.emu8086.com/) or any 8086-compatible DOS emulator
- **Output:** HELLO WORLD appears at the top-left corner in light gray on black background.

---

## Program Features
- Direct memory access to VGA text video memory (`B800h`)
- Loop-based printing of characters
- Uses BIOS keyboard interrupt to wait for a key press
- Fully commented and exam-friendly

---

## How to Run
1. Open `hello_world.asm` in **Emu8086** or any DOS 8086 emulator.
2. Assemble the program.
3. Run the `.COM` file.
4. Observe **HELLO WORLD** printed at the top-left of the screen.
5. Press any key to exit.

---

## Code

```asm
; ------------------------------------------------------------
; Program: HELLO WORLD using 8086 Video Memory (Loop Version)
; About  : Displays "HELLO WORLD" using VGA text memory
; Output : HELLO WORLD at top-left, light gray on black
; Run    : Load in Emu8086 → Assemble → Run (.COM program)
; ------------------------------------------------------------

org 100h                  ; ORG = Origin → start at 0100h (DOS .COM rule)

mov ax, 0B800h            ; AX = Accumulator → load video memory segment B800h
mov ds, ax                ; DS = Data Segment → point to video memory

; Prepare string and length
mov si, hello_string      ; SI = Source Index → points to start of string
mov di, 0                 ; DI = Destination Index → video memory offset
mov cx, 11                ; CX = Counter → length of string "HELLO WORLD"

print_loop:
    mov al, [si]          ; AL = ASCII character from string
    mov [di], al          ; store ASCII at even offset
    mov [di+1], 07h       ; store color (light gray on black) at odd offset
    add di, 2             ; move to next character slot (skip color byte)
    inc si                ; move to next character in string
    loop print_loop       ; decrement CX, repeat until CX=0

mov ah, 0                 ; AH = High byte of AX → BIOS keyboard function 00h
int 16h                   ; wait for any key
ret                       ; terminate program

; Data Section (String)
hello_string db 'HELLO WORLD' ; DB = Define Byte → ASCII codes for the text
