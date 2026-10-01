# Div Folder - Arithmetic Operations and EFLAGS

## div1.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| All flags (CF, ZF, SF, OF, AF, PF) | Undefined | Per Intel's x86 specification, the DIV instruction does not define any of these flags based on its result. Any values gdb displays (e.g. AF appearing set here) are incidental leftovers from prior instructions or internal CPU state, not a meaningful outcome of the division. They should not be interpreted or relied upon. |

### Key Takeaway
DIV is unique among the four operations covered in this assignment: ADD and SUB define CF/OF based on carry/borrow and sign overflow, and MUL defines CF/OF based on whether the result spilled into the upper register - but DIV leaves all flags undefined. The actual result of a division is fully captured in the quotient (AL) and remainder (AH) registers, not in the flags register.


## div2.asm

### Operation


### Flags Observed
| Flag | Status | Reason |
|------|--------|--------|
| All flags (CF, ZF, SF, OF, AF, PF) | Undefined | As with div1.asm, the DIV instruction does not define any of these flags based on its result. The AF shown here is incidental, carried over from register state, and not a meaningful outcome of the division. |

### Key Takeaway
This example confirms the same behavior as div1.asm at a larger scale (16-bit dividend/divisor instead of 8-bit): the flags register provides no reliable information about a DIV operation's outcome. The division's actual result is fully captured in AX (quotient) and DX (remainder), reinforcing that programmers must check these registers directly rather than inspecting flags after a division.