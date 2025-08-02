## General-Purpose Registers (64-bit)

| Register | Purpose & Convention                         | Common Uses                                       |
| -------- | -------------------------------------------- | ------------------------------------------------- |
| **RAX**  | **Accumulator** (return values, syscalls)   | Function return values, syscall numbers          |
| **RBX**  | **Base register** (callee-saved)            | General storage, base pointer for arrays         |
| **RCX**  | **Counter** (caller-saved, 4th syscall arg) | Loop counters, shift counts, string operations   |
| **RDX**  | **Data register** (caller-saved, 3rd syscall arg) | High part of multiplication, I/O operations |
| **RSI**  | **Source Index** (2nd syscall arg)          | Source pointer for string/memory operations      |
| **RDI**  | **Destination Index** (1st syscall arg)     | Destination pointer, function's 1st argument     |
| **RBP**  | **Base Pointer** (callee-saved)             | Stack frame base (optional), local variable access |
| **RSP**  | **Stack Pointer** (special)                 | Always points to top of stack (auto-managed)     |
| **R8-R15** | **Extended registers** (R8-R11 caller-saved) | Additional general-purpose storage             |

### Register Sizes
- **64-bit**: RAX, RBX, RCX, etc. (full register)
- **32-bit**: EAX, EBX, ECX, etc. (lower 32 bits, zeros upper 32)
- **16-bit**: AX, BX, CX, etc. (lower 16 bits)
- **8-bit**: AL/AH, BL/BH, CL/CH, etc. (low/high byte of 16-bit)

## Data Movement Instructions

| Instruction | Syntax | Purpose | Example |
| ----------- | ------ | ------- | ------- |
| `mov` | `mov dest, src` | Copy data between locations | `mov rax, rbx` |
| `lea` | `lea dest, [addr]` | Load effective address | `lea rax, [rbx+rcx*2+8]` |
| `push` | `push src` | Push onto stack | `push rax` |
| `pop` | `pop dest` | Pop from stack | `pop rbx` |
| `xchg` | `xchg op1, op2` | Exchange values | `xchg rax, rbx` |

## Arithmetic Instructions

| Instruction | Syntax | Purpose | Notes |
| ----------- | ------ | ------- | ----- |
| `add` | `add dest, src` | Addition | `add rax, 5` |
| `sub` | `sub dest, src` | Subtraction | `sub rbx, rcx` |
| `mul` | `mul src` | Unsigned multiply | `rax = rax * src`, high bits in `rdx` |
| `imul` | `imul dest, src` | Signed multiply | `imul rax, rbx` (various forms) |
| `div` | `div src` | Unsigned divide | Quotient in `rax`, remainder in `rdx` |
| `idiv` | `idiv src` | Signed divide | Quotient in `rax`, remainder in `rdx` |
| `inc` | `inc dest` | Increment by 1 | `inc rax` |
| `dec` | `dec dest` | Decrement by 1 | `dec rbx` |
| `neg` | `neg dest` | Negate (two's complement) | `neg rax` |

## Bitwise & Logical Instructions

| Instruction | Syntax | Purpose | Example |
| ----------- | ------ | ------- | ------- |
| `and` | `and dest, src` | Bitwise AND | `and rax, 0xFF` |
| `or` | `or dest, src` | Bitwise OR | `or rbx, rcx` |
| `xor` | `xor dest, src` | Bitwise XOR | `xor rax, rax` (clear register) |
| `not` | `not dest` | Bitwise NOT | `not rax` |
| `shl` | `shl dest, count` | Shift left | `shl rax, 2` (multiply by 4) |
| `shr` | `shr dest, count` | Shift right (logical) | `shr rbx, cl` |
| `sar` | `sar dest, count` | Shift right (arithmetic) | `sar rax, 1` (signed divide by 2) |

## Comparison & Control Flow

| Instruction | Purpose | Example | Description |
| ----------- | ------- | ------- | ----------- |
| `cmp` | Compare values | `cmp rax, rbx` | Sets flags for conditional jumps |
| `test` | Bitwise test | `test rax, rax` | Sets flags (like AND but doesn't store) |
| `jmp` | Unconditional jump | `jmp .loop` | Always jump to label |
| `je`/`jz` | Jump if equal/zero | `je .equal` | Jump if last comparison was equal |
| `jne`/`jnz` | Jump if not equal/not zero | `jne .not_equal` | Jump if last comparison was not equal |
| `jl`/`jnge` | Jump if less | `jl .less` | Signed less than |
| `jg`/`jnle` | Jump if greater | `jg .greater` | Signed greater than |
| `ja` | Jump if above | `ja .above` | Unsigned greater than |
| `jb` | Jump if below | `jb .below` | Unsigned less than |
| `call` | Call function | `call func` | Push return address and jump |
| `ret` | Return from function | `ret` | Pop return address and jump |
| `loop` | Loop with counter | `loop .start` | Decrement RCX and jump if not zero |

## Memory Addressing Modes

| Format | Example | Description |
| ------ | ------- | ----------- |
| Register | `mov rax, rbx` | Direct register access |
| Immediate | `mov rax, 42` | Constant value |
| Memory Direct | `mov rax, [addr]` | Direct memory address |
| Register Indirect | `mov rax, [rbx]` | Memory at address in RBX |
| Base + Offset | `mov rax, [rbx+8]` | Memory at RBX + 8 |
| Base + Index | `mov rax, [rbx+rcx]` | Memory at RBX + RCX |
| Full Format | `mov rax, [base+index*scale+disp]` | `[rbx+rcx*2+8]` |

### Scale Values: 1, 2, 4, 8 (for byte, word, dword, qword indexing)

## String/Memory Operations

| Instruction | Purpose | Example | Notes |
| ----------- | ------- | ------- | ----- |
| `movsb` | Move byte | `rep movsb` | Copy from [RSI] to [RDI] |
| `movsw` | Move word | `movsw` | Copy 2 bytes |
| `movsd` | Move dword | `movsd` | Copy 4 bytes |
| `movsq` | Move qword | `movsq` | Copy 8 bytes |
| `stosb` | Store byte | `rep stosb` | Store AL to [RDI] |
| `lodsb` | Load byte | `lodsb` | Load from [RSI] to AL |
| `cmpsb` | Compare bytes | `repe cmpsb` | Compare [RSI] with [RDI] |

## Common Patterns & Idioms

```assembly
; Clear register (faster than mov rax, 0)
xor rax, rax

; Set register to -1
mov rax, -1
; or
or rax, -1

; Multiply by power of 2
shl rax, 3          ; multiply by 8

; Test if register is zero
test rax, rax       ; sets zero flag if rax == 0
jz .is_zero

; Function prologue
push rbp
mov rbp, rsp
sub rsp, 32         ; allocate stack space

; Function epilogue
mov rsp, rbp
pop rbp
ret
```

## Flags Register (RFLAGS)

| Flag | Bit | Purpose | Set When |
| ---- | --- | ------- | -------- |
| **CF** | 0 | Carry | Unsigned overflow |
| **ZF** | 6 | Zero | Result is zero |
| **SF** | 7 | Sign | Result is negative |
| **OF** | 11 | Overflow | Signed overflow |

## Common Syscalls (Linux x86-64)

| RAX | Syscall | RDI | RSI | RDX |
| --- | ------- | --- | --- | --- |
| 0 | sys_read | fd | buffer | count |
| 1 | sys_write | fd | buffer | count |
| 60 | sys_exit | status | - | - |
