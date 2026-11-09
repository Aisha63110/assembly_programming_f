# 05.asm - push and pop of 64-bit values

## What the program does
Loads RAX with the 64-bit value `0x1122334455667788` and RBX with `0x1234`, pushes both, then pops them back in reverse order into RBX and RAX.

## Stack flow (observed in GDB)
RSP at the start is `0x7fffffffde10` (top of stack holds argc = 1).

| Instruction | RSP after | Top of stack [RSP] | What happened |
|---|---|---|---|
| `push rax` | `0x...de08` | `0x1122334455667788` | The full 8-byte value is stored in one slot. RSP -8. |
| `push rbx` | `0x...de00` | `0x1234` | RBX stored on top. RSP -8. |
| `pop rbx` | `0x...de08` | `0x1122334455667788` | 0x1234 returned to RBX. RSP +8. |
| `pop rax` | `0x...de10` | `0x1` | The full 64-bit value returned to RAX. RSP +8. |

## Observations
- A 64-bit push always moves RSP by exactly 8 bytes, whatever the value: `0x1234` takes the same 8-byte slot as the large value.
- The pops are in the reverse order of the pushes (last in, first out). Because they are popped back into the same registers they came from, RAX and RBX end with their original values.
- The whole 64-bit value in RAX came back unchanged, which shows the stack stores the full register with nothing lost.
- RSP returned to `0x...de10`, so the stack is balanced.
