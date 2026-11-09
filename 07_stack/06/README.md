# 06.asm - push and pop of 32-bit values

## What the program does
The 32-bit version of 05.asm (`BITS 32`). Loads EAX with `0x11223344` and EBX with `0x1234`, pushes both, then pops them back in reverse order into EBX and EAX.

## Stack flow (observed in GDB)
ESP at the start is `0xffffcf50` (top of stack holds argc = 1).

| Instruction | ESP after | Top of stack [ESP] | What happened |
|---|---|---|---|
| `push eax` | `0xffffcf4c` | `0x11223344` | EAX stored. ESP -4. |
| `push ebx` | `0xffffcf48` | `0x1234` | EBX stored on top. ESP -4. |
| `pop ebx` | `0xffffcf4c` | `0x11223344` | 0x1234 returned to EBX. ESP +4. |
| `pop eax` | `0xffffcf50` | `0x1` | 0x11223344 returned to EAX. ESP +4. |

## Observations
- In 32-bit mode each push moves ESP by 4 bytes, compared with 8 bytes in 05.asm. This is the only real difference between the two programs.
- The order is last in, first out, and the values go back into the registers they came from.
- ESP returned to `0xffffcf50`, so the stack is balanced.
- The addresses start with `0xffff...` here instead of `0x7fff...` because 32-bit programs use a smaller address space.
