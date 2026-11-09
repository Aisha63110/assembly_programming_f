# 04.asm - a stack frame with local variables (32-bit)

## What the program does
The 32-bit version of 02.asm (`BITS 32`, uses `int 0x80`). `_start` calls `work`, which stores two local variables (100 and 200) using EBP/ESP, reads them back into EAX and EBX, and returns.

## Stack flow (observed in GDB)
ESP at the start is `0xffffcf50` (top of stack holds argc = 1). Slots are 4 bytes in 32-bit mode.

| Step | ESP after | What happened |
|---|---|---|
| `call work` | `0xffffcf4c` | Pushes the return address `0x8049005`. ESP -4. |
| `push ebp` | `0xffffcf48` | Saves the caller's EBP (value 0). ESP -4. |
| `mov ebp, esp` | `0xffffcf48` | EBP becomes the frame base. ESP unchanged. |
| `sub esp, 8` | `0xffffcf40` | Reserves 8 bytes for two 4-byte locals. ESP -8. |
| `mov dword [ebp-4], 100` | `0xffffcf40` | Stores 100 at `ebp-4`. |
| `mov dword [ebp-8], 200` | `0xffffcf40` | Stores 200 at `ebp-8`, which is the top of the stack. |
| `mov eax, [ebp-4]` / `mov ebx, [ebp-8]` | `0xffffcf40` | EAX = 100, EBX = 200. |
| `mov esp, ebp` | `0xffffcf48` | Releases the locals. |
| `pop ebp` | `0xffffcf4c` | Restores the caller's EBP. ESP +4. |
| `ret` | `0xffffcf50` | Pops the return address into EIP. ESP +4. |

## Observations
- Same logic as 02.asm, but every stack slot is 4 bytes instead of 8: the return address and the saved EBP each move ESP by 4, and two dword locals need 8 bytes instead of 16.
- ESP finished at `0xffffcf50`, the same value as before the call, so the frame was cleaned up correctly.
- The locals sit at fixed offsets from EBP (`ebp-4`, `ebp-8`), which stay valid however ESP changes.
