# 03.asm - call and ret (64-bit)

## What the program does
Puts 10 in RAX, calls `calculate` (which adds 5, so RAX becomes 15), returns to `after_call`, and exits. There is no `push` or `pop` in the code, yet the stack is still used.

## Stack flow (observed in GDB)

| Step | RSP | Top of stack | What happened |
|---|---|---|---|
| Before `call calculate` | `0x7fffffffde10` | `0x1` (argc) | Stack holds only OS data |
| After `call calculate` | `0x7fffffffde08` | `0x40100a` | `call` pushed the return address (the address of `after_call`). RSP -8. |
| `add rax, 5` | `0x...de08` | `0x40100a` | The procedure runs; RSP does not move. RAX = 10 + 5 = 15. |
| After `ret` | `0x7fffffffde10` | `0x1` | `ret` popped `0x40100a` into RIP. RSP +8. |

## Observations
- `call` is a hidden push: it saves the address to return to, so the processor knows where to continue. `ret` is a hidden pop that jumps to that address.
- The return address `0x40100a` matches the address of `after_call` in the disassembly, which proves `ret` goes back exactly to the instruction after the call.
- Execution resumed at `after_call` and RSP was back at `0x...de10`, so the stack was balanced.
- If `calculate` had pushed something without popping it, `ret` would have jumped to the wrong address and the program would have crashed.
