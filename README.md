
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



## 🧪 Task 3 – Generate Assembly from C
... (remaining task content continues from previous steps and ends at Task 17)
