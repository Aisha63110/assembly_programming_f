# 06_loop

All programs are 64-bit. Each loop needs a counter, a body, and a conditional jump back.

## 01_loop_instruction.asm
Intended to print the message 5 times using `loop` with rcx. It never stops: `syscall` overwrites rcx with the return address, so `loop` decrements a huge value instead of 4 and prints millions of times. Fix: save and restore rcx (see 09), or use another register as the counter (see 02).

## 02_dec_jnz.asm
Prints the message 5 times. `dec r12` sets ZF when r12 reaches 0, and `jnz repeat` loops while ZF is clear. The counter is in r12, which syscall does not modify.

## 04_while_loop.asm
Prints 1 to 5, one per line. It jumps first to `check_condition`, where `cmp r12, 6` and `jl loop_body` run the body only while r12 < 6. The condition is tested before the body, which makes it a while loop. The counter is converted to a character by adding '0', so it only works for single digits.

## 07_sum_loop.asm
Adds 1+2+3+4+5 in r13 using r12 as the counter, then prints `Sum = 15`.

| r12 | r13 | Flags after cmp | Why |
|-----|-----|-----------------|-----|
| 2 to 5 | 1, 3, 6, 10 | CF, SF set; ZF clear | r12 - 6 is negative and needs a borrow, so SF != OF and jl jumps back |
| 6 | 15 | ZF set; CF, SF clear | r12 - 6 = 0, so jl does not jump and the loop ends |

## 08_infinite_loop.asm
Prints "Running..." forever because `jmp repeat` is unconditional and nothing exits the loop. Stop it with Ctrl+C. It has no `_start` label, so ld falls back to the start of .text, which happens to be `repeat`.

## 09_loop_syscall.asm
Prints the message 5 times using `loop` with rcx. rcx is saved with `push rcx` before the syscall and restored with `pop rcx` afterwards, so the counter survives. `loop` decrements rcx and jumps if it is not zero; it does not change any flags.
