| Register | Purpose (by convention)                      | Example Use                                       |
| -------- | -------------------------------------------- | ------------------------------------------------- |
| **RAX**  | **Accumulator** (used for return values)     | `mov rax, 1`                                      |
| **RBX**  | **Base register**, general-purpose           | `mov rbx, 123`                                    |
| **RCX**  | **Counter** (loops, shifts, syscall arg 4)   | `loop`, `shl rax, cl`                             |
| **RDX**  | **Data** register (used with `rax` often)    | `div rbx` → quotient in `rax`, remainder in `rdx` |
| **RSI**  | **Source index** for string/memory ops       | `movsb`, `mov rsi, src_ptr`                       |
| **RDI**  | **Destination index** for string/memory ops  | `mov rdi, dest_ptr`                               |
| **RBP**  | **Base pointer** for stack frames (optional) | Used for local vars in functions                  |
| **RSP**  | **Stack pointer**                            | Always points to top of the stack                 |
