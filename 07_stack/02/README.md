# 02.asm - a stack frame with local variables (64-bit)

## What the program does
`_start` calls the procedure `work`. `work` builds a stack frame, stores two local variables (100 and 200), reads them back into RAX and RBX, tears the frame down and returns.

## Stack flow (observed in GDB)
RSP at the start is `0x7fffffffde10` (top of stack holds argc = 1).

| Step | RSP after | What happened |
|---|---|---|
| `call work` | `0x...de08` | `call` pushes the return address `0x401005` (the instruction after the call). RSP -8. |
| `push rbp` | `0x...de00` | Saves the caller's RBP (value 0) on the stack. RSP -8. |
| `mov rbp, rsp` | `0x...de00` | RBP now points to the saved RBP. This is the frame base; RSP does not change. |
| `sub rsp, 16` | `0x...ddf0` | Reserves 16 bytes for two local variables. RSP -16. |
| `mov qword [rbp-8], 100` | `0x...ddf0` | Stores 100 at `rbp-8`. Nothing pushed, RSP unchanged. |
| `mov qword [rbp-16], 200` | `0x...ddf0` | Stores 200 at `rbp-16`, which is also the top of the stack. |
| `mov rax, [rbp-8]` / `mov rbx, [rbp-16]` | `0x...ddf0` | Reads the locals back: RAX = 100, RBX = 200. |
| `mov rsp, rbp` | `0x...de00` | Releases the locals in one step by moving RSP back to the frame base. |
| `pop rbp` | `0x...de08` | Restores the caller's RBP. RSP +8. |
| `ret` | `0x...de10` | Pops the return address `0x401005` into RIP. Execution continues in `_start`. RSP +8. |

## Observations
- The stack is used three ways here: for the return address (`call`/`ret`), for the saved RBP (`push`/`pop`), and for local variables reserved by lowering RSP.
- The local variables were written with `mov` relative to RBP, not with `push`. RSP only moved when the space was reserved (`sub`) and released (`mov rsp, rbp`).
- RSP returned to `0x...de10`, exactly its value before the `call`, so the stack is balanced.
- Using RBP as a fixed reference means the locals stay reachable at `[rbp-8]` and `[rbp-16]` even if RSP changes.
