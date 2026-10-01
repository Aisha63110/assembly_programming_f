# Sub Folder - Arithmetic Operations and EFLAGS

## sub1.asm

### Operation



### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| ZF (Zero) | Cleared (0) | The result (-30) is not zero. |
| SF (Sign) | Set (1) | The most significant bit of the result (11100010) is 1, correctly indicating a negative value. |
| CF (Carry/Borrow) | Set (1) | In subtraction, CF acts as a borrow flag. Since the unsigned value of num1 (50) is less than num2 (80), a borrow was required, so CF is set. |
| OF (Overflow) | Cleared (0) | Both operands are positive (same sign), so no signed overflow can occur. The result (-30) fits within the signed 8-bit range (-128 to 127). |
| AF (Auxiliary Carry) | Cleared (0) | Comparing the lower nibbles of num1 (0010) and num2 (0000), no borrow was needed from the upper nibble. |
| PF (Parity) | Set (1) | The result (11100010) has an even number of 1-bits (four), so parity is set. |

### Key Takeaway
For subtraction, CF represents a borrow rather than a carry — it is set whenever the minuend is unsigned-smaller than the subtrahend. OF only activates when the operands have different signs and the result's sign doesn't match the expected outcome; since both operands here were positive, no signed overflow occurred even though the result is negative.


## sub2.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| ZF (Zero) | Cleared (0) | The result (-1000) is not zero. |
| SF (Sign) | Set (1) | The most significant bit of the result is 1, correctly indicating a negative value. |
| CF (Carry/Borrow) | Set (1) | Since the unsigned value of num1 (1000) is less than num2 (2000), a borrow was required, so CF is set. |
| OF (Overflow) | Cleared (0) | Both operands are positive (same sign), so no signed overflow can occur. The result (-1000) fits within the signed 16-bit range (-32768 to 32767). |
| AF (Auxiliary Carry) | Cleared (0) | Comparing the lower nibbles of num1 (8) and num2 (0), no borrow was needed from the upper nibble. |
| PF (Parity) | Set (1) | The low byte of the result (00011000) has an even number of 1-bits (two), so parity is set. |

### Key Takeaway
This example confirms the same borrow behavior seen in sub1.asm, scaled to 16-bit operands: CF reflects an unsigned borrow, while OF stays cleared because the operands share the same sign, keeping the signed result within range despite being negative.