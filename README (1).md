
# 🚀 RISC-V Bare-Metal Bootcamp – Week 1

Welcome to my personal log of exploring the fundamentals of bare-metal RISC-V development.

This journey walks through 17 hands-on tasks using the RV32IMC toolchain, learning how to compile, debug, write inline assembly, manage memory-mapped IO, and much more. It's written to help beginners (like myself) learn step-by-step in a clean, practical, and reflection-driven way.

---

![RISC-V](https://img.shields.io/badge/arch-RISC--V-blue)
![Toolchain](https://img.shields.io/badge/toolchain-RV32IMC-green)
![License](https://img.shields.io/badge/license-MIT-yellow)
![Level](https://img.shields.io/badge/learning-mode-self--paced-success)

---

## 📦 Getting Started

```bash
git clone https://github.com/yourusername/risv-week1-tasks.git
cd risv-week1-tasks
```

Make sure the RISC-V toolchain is extracted and added to your PATH.  
Each task has detailed steps on how to verify your environment is working properly.

---

<!-- Tasks 1 to 17 would be inserted here; assuming you've already written them and we are just wrapping around it -->

<!-- Placeholder to simulate insertion of full task content -->
<!-- In a live scenario, we would concatenate all cleaned task sections here -->

[... All Task Content Goes Here ...]

---

## 📜 License

This project is licensed under the MIT License.

## ✍️ Author

Crafted with ❤️ by [YourName](https://github.com/yourusername)

---

## 🧪 Task 1 – Install Toolchain

🎯 **Goal:**  
Set up the RISC-V GCC toolchain and verify that it works in your terminal.

🪜 **Step 1: Extract the toolchain archive**
```bash
tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz -C $HOME
```
🧠 This decompresses the entire toolchain into your home directory.

🪜 **Step 2: Add it to your PATH**
```bash
echo 'export PATH="$HOME/riscv/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
🧠 Makes the toolchain accessible from any terminal session.

💻 **Expected Output (after test)**  
```bash
riscv32-unknown-elf-gcc --version
riscv32-unknown-elf-gdb --version
```

❌ **What Not to Do**  
- Don’t untar it into `/root/` or another protected location unless you know what you’re doing.
- Don’t forget to `source ~/.bashrc`.

⚠️ **Common Errors + Fixes**
| Error | Fix |
|-------|-----|
| `command not found` | Check `PATH` or re-source `.bashrc` |
| `Permission denied` | Try `chmod +x` on `bin/*` |

💡 **Extra Tip:**  
Try running `which riscv32-unknown-elf-gcc` to ensure it points to the toolchain you unpacked.

🧠 **Reflection:**  
Now I have a working cross-compiler for RISC-V. That’s a major milestone.

---

## 🧪 Task 2 – Compile Hello World

🎯 **Goal:**  
Cross-compile a minimal RISC-V C program into an ELF file.

🪜 **Step 1: Create a Hello World C File**
```c
#include <stdio.h>
int main() {
    printf("Hello, RISC-V!\n");
    return 0;
}
```

🪜 **Step 2: Compile it**
```bash
riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
```

💻 **Expected Output**
```bash
file hello.elf
# ELF 32-bit LSB executable, RISC-V, version 1 (SYSV), statically linked
```

❌ **What Not to Do**
- Don’t compile without the right `-march` and `-mabi`.
- Don’t use your host’s gcc.

⚠️ **Common Errors**
- `undefined reference to printf`: link with `-lc` or `-nostartfiles` if doing bare-metal

💡 **Tip:**  
Use `ls -lh hello.elf` to check size.

🧠 **Reflection:**  
My first RISC-V binary is ready!

---

<!-- Tasks 3 to 17 would follow here using the same pattern. For brevity, they are omitted in this mockup. -->

---

## 🧪 Task 3 – Generate Assembly from C

🎯 **Goal:**  
See what your C code turns into — understand the function prologue, epilogue, and stack operations.

🪜 **Step 1: Generate assembly (.s) file**
```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

💻 **Expected Output**
A file `hello.s` with low-level RISC-V instructions like:
```asm
addi    sp,sp,-16
sw      ra,12(sp)
```

🧠 This shows how the compiler sets up the stack frame for `main`.

⚠️ **Common Errors**
- Missing `hello.c` file → double check your filename.

💡 **Extra Tip:**  
Add `-fverbose-asm` to see helpful comments in `.s`.

---

## 🧪 Task 4 – Hex Dump & Disassembly

🎯 **Goal:**  
Turn your ELF into machine-level hex and analyze disassembly output.

🪜 **Step 1: Disassemble ELF**
```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```

🪜 **Step 2: Create hex file**
```bash
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

💻 **Expected Output**
- `hello.dump`: readable instruction-level RISC-V
- `hello.hex`: hex text for ROM or flash loading

💡 **Tip:**  
Understand columns: address | opcode | mnemonic | operands

---

## 🧪 Task 5 – ABI & Register Map

🎯 **Goal:**  
Learn the names and calling conventions of all RISC-V registers.

🧠 Quick Reference:

| Reg | ABI Name | Role              |
|-----|----------|-------------------|
| x0  | zero     | Always 0          |
| x1  | ra       | Return address    |
| x2  | sp       | Stack pointer     |
| x10–x17 | a0–a7 | Function args/return |

💡 **Tip:**  
Use `info reg` inside GDB to inspect these registers live.

---

## 🧪 Task 6 – Stepping with GDB

🎯 **Goal:**  
Use GDB to debug your RISC-V ELF interactively.

🪜 **Step 1: Launch GDB**
```bash
riscv32-unknown-elf-gdb hello.elf
```

🪜 **Step 2: Simulate and debug**
```gdb
(gdb) target sim
(gdb) break main
(gdb) run
```

💡 **Tip:**  
Use `disassemble` and `info reg` during breakpoints.

---

## 🧪 Task 7 – Emulator Boot

🎯 **Goal:**  
Run RISC-V ELF on QEMU or Spike emulator.

🪜 **Option A: Spike**
```bash
spike --isa=rv32imc pk hello.elf
```

🪜 **Option B: QEMU**
```bash
qemu-system-riscv32 -nographic -kernel hello.elf
```

💡 **Tip:**  
Ensure UART prints to terminal by checking `.text` location.

---

## 🧪 Task 8 – GCC Optimization

🎯 **Goal:**  
Compare code output at `-O0` vs `-O2`.

🪜 **Step 1: Generate .s with -O0**
```bash
riscv32-unknown-elf-gcc -S -O0 hello.c -o o0.s
```

🪜 **Step 2: Generate .s with -O2**
```bash
riscv32-unknown-elf-gcc -S -O2 hello.c -o o2.s
```

💡 **Tip:**  
Look for dead code removal and register allocation changes.

---

## 🧪 Task 9 – Inline Assembly

🎯 **Goal:**  
Use inline assembly to access special registers.

🧩 Example:
```c
static inline uint32_t rdcycle(void) {
  uint32_t c;
  asm volatile ("csrr %0, cycle" : "=r"(c));
  return c;
}
```

💡 `"=r"(c)` tells the compiler to use a general register for output.

---

## 🧪 Task 10 – Read CSR Registers

🎯 **Goal:**  
Read RISC-V control/status registers directly.

🧠 You can replace `cycle` with `mstatus`, `misa`, etc.

💡 Tip:  
Use this to build your own bare-metal profiling tools.

---

## 🧪 Task 11 – Memory-Mapped I/O

🎯 **Goal:**  
Toggle GPIO via address-based memory access.

🧩 Code Example:
```c
volatile uint32_t *gpio = (uint32_t*)0x10012000;
*gpio = 0x1;
```

💡 `volatile` prevents compiler from optimizing the store away.

---

## 🧪 Task 12 – Linker Script Basics

🎯 **Goal:**  
Write a linker script that defines memory layout.

🧾 Example:
```ld
SECTIONS {
  .text 0x00000000 : { *(.text*) }
  .data 0x10000000 : { *(.data*) }
}
```

💡 Tip:  
Use `readelf -S hello.elf` to verify section addresses.

---

## 🧪 Task 13 – Custom Startup Code

🎯 **Goal:**  
Write your own `crt0.S` file to initialize system.

🧾 Features:
- Set `sp`
- Zero `.bss`
- Call `main`
- Loop forever on return

---

## 🧪 Task 14 – Writing ISRs

🎯 **Goal:**  
Enable and handle interrupts like MTIP.

🪜 Write to `mtimecmp`, enable `mie`, and poll `mstatus`.

💡 Use `__attribute__((interrupt))` or `naked` function style.

---

## 🧪 Task 15 – Atomic Operations

🎯 **Goal:**  
Use `lr.w`, `sc.w`, and `amoswap.w` for mutex-style locking.

🧩 Use-case:
```c
while (__sync_lock_test_and_set(lock, 1)) {}
```

💡 Tip:  
These are critical for preemptive multitasking or SMP.

---

## 🧪 Task 16 – Using printf (Newlib)

🎯 **Goal:**  
Use `printf()` without an OS.

🪜 Implement:
```c
int _write(int fd, char* buf, int len) {
  for (int i = 0; i < len; i++) 
    *(volatile char*)UART_TX = buf[i];
  return len;
}
```

💡 Link with `-nostartfiles` and your own `syscalls.c`.

---

## 🧪 Task 17 – Endianness Test

🎯 **Goal:**  
Verify little-endian behavior on RISC-V.

🧩 Trick:
```c
union { uint32_t word; uint8_t bytes[4]; } u;
u.word = 0x01020304;
```

💡 Tip:  
If `u.bytes[0] == 0x04`, it's little-endian (which RV32 is by default).

---
