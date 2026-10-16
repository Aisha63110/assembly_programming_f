# assembly_programming_f

## and.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| ZF (Zero) | Cleared (0) | The result (10001000) is not zero. |
| SF (Sign) | Set (1) | The most significant bit of the result is 1. |
| PF (Parity) | Set (1) | The low byte has two 1-bits (an even count), so parity is set. |
| CF (Carry) | Cleared (0) | AND always clears CF, because logical operations produce no carry. |
| OF (Overflow) | Cleared (0) | AND always clears OF, because logical operations cannot overflow. |

### Key Takeaway
AND keeps a bit only where both operands have a 1. Unlike ADD and SUB, logic instructions always clear CF and OF, and ZF, SF and PF simply describe the result's bits.
