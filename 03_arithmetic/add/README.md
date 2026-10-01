# Add Folder - Arithmetic Operations and EFLAGS

## add1.asm

### Operation



### Flags Observed
| Flag | Status | Reason                               |
|------|--------|-----------------------------------------|
| ZF (Zero) | Cleared (0) | The result (130) is not zero. |
| SF (Sign) | Set (1) | The most significant bit of the result (10000010) is 1, so the CPU interprets this as a negative value in signed terms. |
| CF (Carry) | Cleared (0) | The unsigned sum (130) fits within the 8-bit range (0-255), so there was no carry out of the most significant bit. |
| OF (Overflow) | Set (1) | Two positive numbers (120 and 10) produced a result with the sign bit set, which is invalid in signed arithmetic. This indicates signed overflow. |
| PF (Parity) | Set (1) | The least significant byte of the result (10000010) has an even number of 1-bits (two), so parity is set. |
| AF (Auxiliary Carry) | Set (1) | Adding the lower nibbles of num1 and num2 (1000 + 1010) overflowed 4 bits, producing a carry into the upper nibble. |

### Key Takeaway
Even though the unsigned addition is valid (130 fits in 8 bits, so CF = 0), the *signed* interpretation overflows (OF = 1) because adding two positive numbers produced a result that looks negative. This demonstrates why CF and OF track different things: CF checks unsigned range, OF checks signed range.

## add2.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| ZF (Zero) | Cleared (0) | The result (32500) is not zero. |
| SF (Sign) | Cleared (0) | 32500 is below 32768, so the most significant bit of the 16-bit result is 0, correctly indicating a positive value. |
| CF (Carry) | Cleared (0) | The unsigned sum (32500) fits within the 16-bit range (0-65535), so there is no carry out of the top bit. |
| OF (Overflow) | Cleared (0) | The sum (32500) stays below the signed 16-bit maximum (32767), so no signed overflow occurs. |

### Key Takeaway
Unlike add1.asm, this addition stays within both the unsigned and signed range for its register size, so all arithmetic flags (ZF, SF, CF, OF) remain cleared. This illustrates that overflow depends on the actual operand values relative to the register width, not on the `add` instruction itself.