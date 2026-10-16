# 04_logic_and_conditions

Logic instructions always clear CF and OF. ZF, SF and PF describe the result's bits. AF is undefined for these instructions, so it is ignored.

## and.asm
Operation: `10101010 AND 11001100 = 10001000` (0x88). A result bit is 1 only where both inputs are 1.

| Flag | Status | Reason |
|------|--------|--------|
| ZF | Cleared (0) | The result 10001000 is not zero. |
| SF | Set (1) | The most significant bit of the result is 1. |
| PF | Set (1) | The low byte has two 1-bits (even count). |
| CF | Cleared (0) | AND always clears CF. |
| OF | Cleared (0) | AND always clears OF. |

## or.asm
Operation: `10101010 OR 11001100 = 11101110` (0xEE). A result bit is 1 where either input is 1.

| Flag | Status | Reason |
|------|--------|--------|
| ZF | Cleared (0) | The result 11101110 is not zero. |
| SF | Set (1) | The most significant bit of the result is 1. |
| PF | Set (1) | The low byte has six 1-bits (even count). |
| CF | Cleared (0) | OR always clears CF. |
| OF | Cleared (0) | OR always clears OF. |

## xor.asm
Operation: `10101010 XOR 11001100 = 01100110` (0x66). A result bit is 1 where the inputs differ.

| Flag | Status | Reason |
|------|--------|--------|
| ZF | Cleared (0) | The result 01100110 is not zero. |
| SF | Cleared (0) | The most significant bit of the result is 0. |
| PF | Set (1) | The low byte has four 1-bits (even count). |
| CF | Cleared (0) | XOR always clears CF. |
| OF | Cleared (0) | XOR always clears OF. |

## not.asm
Operation: `NOT 10101010 = 01010101` (0x55). Every bit is flipped.

| Flag | Status | Reason |
|------|--------|--------|
| All flags | Unchanged | NOT does not affect any flags. The flag register stayed the same before and after the instruction (only IF set). |

## test.asm
Operation: `10101010 TEST 00001111`. TEST computes the AND (`00001010`) but discards the result, so AL stays 0xAA and only the flags change.

| Flag | Status | Reason |
|------|--------|--------|
| ZF | Cleared (0) | The AND result 00001010 is not zero (some bits overlap). |
| SF | Cleared (0) | The most significant bit of the AND result is 0. |
| PF | Set (1) | The low byte of the result has two 1-bits (even count). |
| CF | Cleared (0) | TEST always clears CF. |
| OF | Cleared (0) | TEST always clears OF. |

TEST is used to check bits or test for zero without destroying the register's value.
