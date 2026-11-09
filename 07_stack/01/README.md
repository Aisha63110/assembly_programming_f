# 01.asm - push and pop with registers (64-bit)

## What the program does
Loads 100 into RAX and 200 into RBX, pushes both onto the stack, pops them into RCX and RDX, then exits with `syscall`.

## Stack flow (observed in GDB)
RSP at the start is `0x7fffffffde20`. The value stored there (`1`) is `argc`, placed by Linux before the program starts. It belongs to the operating system, not to this program.

| Instruction | RSP after | Top of stack [RSP] | What happened |
|---|---|---|---|
| (start) | `0x...de20` | `0x1` (argc) | Stack contains only OS data |
| `push rax` | `0x...de18` | `0x64` (100) | RSP decreased by 8, then RAX was stored there |
| `push rbx` | `0x...de10` | `0xc8` (200) | RSP decreased by 8 again; 200 is now on top of 100 |
| `pop rcx` | `0x...de18` | `0x64` | Top value (200) copied into RCX, RSP increased by 8 |
| `pop rdx` | `0x...de20` | `0x1` | Next value (100) copied into RDX, RSP back at its start |

## Observations
- The stack grows downward: every push lowers RSP by 8 (one 64-bit slot) and every pop raises it by 8.
- It is last in, first out. RBX was pushed last, so it is popped first: RCX = 200 and RDX = 100, the reverse of the push order.
- Two pushes and two pops are balanced, so RSP ends exactly where it started (`0x...de20`). An unbalanced stack would leave RSP somewhere else.
- The absolute addresses change between runs and machines, but the offsets (8 bytes per push) stay the same.
