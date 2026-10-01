# Mul Folder - Arithmetic Operations and EFLAGS

## mul1.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| CF (Carry) | Cleared (0) | For MUL, CF (and OF) are set only when the result requires the upper half of the destination register (AH) to represent it. Since 250 fits entirely within AL (0-255), AH is 0, so CF is cleared. |
| OF (Overflow) | Cleared (0) | OF mirrors CF for MUL - it is set under the exact same condition (result doesn't fit in the lower half). Since CF is cleared here, OF is cleared too. |
| ZF, SF, AF, PF | Undefined | The MUL instruction does not define meaningful values for these flags. Any value gdb displays for them is leftover/incidental and should not be relied upon. |

### Key Takeaway
Unlike ADD and SUB, which set CF/OF based on the sign and magnitude of the numbers involved, MUL uses CF and OF purely to indicate whether the full result fit within the "expected" register size (AL for byte multiplication) or spilled into the upper half (AH). Since 25 x 10 = 250 fits within a single byte, no spill occurred, and both flags are cleared.


## mul2.asm

### Operation



### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| CF (Carry) | Set (1) | The result (600,000) is too large to fit in AX alone (max 65,535), so it spilled into DX. Since the upper half (DX) is non-zero, CF is set. |
| OF (Overflow) | Set (1) | OF mirrors CF for MUL. Since the result required DX to represent it fully, OF is set alongside CF. |
| ZF, SF, AF, PF | Undefined | As with mul1.asm, MUL does not define meaningful values for these flags. |

### Key Takeaway
This example directly contrasts mul1.asm: there, the product (250) fit entirely within AL, so CF/OF were cleared. Here, the product (600,000) exceeds AX's 16-bit capacity and spills into DX, so CF/OF are both set. This confirms that for MUL, CF and OF simply indicate "did the result need the extra register" - not whether the math is valid or invalid, since multiplication results are always mathematically correct.