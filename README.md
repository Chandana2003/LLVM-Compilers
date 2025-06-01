# LLVM-Compilers

### LLVM Compiler Optimizations Project Overview

This project consists of three main compiler passes implemented using the LLVM framework:

* **HelloPass**
* **Reaching Definition Analysis**
* **Common Subexpression Elimination**

The directory structure is organized as follows:

* The `LLVM/` folder contains installation and build instructions for LLVM.
* The `Pass/` directory includes the implementation code for all three passes.
* The `test/` folder provides sample input and output files used for validating the passes. For example, `1.ll` is a sample LLVM IR input, and `1.out` is its corresponding output after pass execution.

Within the `test/phase2` and `test/phase3` subfolders, two scripts assist in testing:

* `create_input.sh` converts a C source file (e.g., `1.c`) into LLVM IR format (`1.ll`) using Clang.
* `test.sh` runs the implemented pass (e.g., Liveness Analysis) using the LLVM `opt` tool and outputs the results to a `.out` file.

---

### Pass Implementation Details

Each pass extends the `FunctionPass` class and overrides the `runOnFunction(Function &F)` method, which is called once per function in the LLVM IR code. The function’s name can be accessed via `F.getName()`.

The pass typically follows this structure:

* Iterate over all basic blocks in the function:

  ```cpp
  for (auto& basic_block : F) { ... }
  ```
* Within each basic block, iterate through all instructions:

  ```cpp
  for (auto& inst : basic_block) { ... }
  ```
* To analyze operands, cast the instruction to `User` and access its operands:

  ```cpp
  auto* ptr = dyn_cast<User>(&inst);
  for (auto it = ptr->op_begin(); it != ptr->op_end(); ++it) { ... }
  ```

### Instruction Analysis

* To identify binary operations (like addition, multiplication), use:

  ```cpp
  if (inst.isBinaryOp()) {
      inst.getOpcodeName(); // returns operation name, e.g., "add", "mul"
  }
  ```
* Specific operations can be matched with:

  ```cpp
  if (inst.getOpcode() == Instruction::Add) { ... }
  if (inst.getOpcode() == Instruction::Mul) { ... }
  ```

### Control Flow Analysis

* Predecessors and successors of a basic block can be accessed using:

  ```cpp
  for (auto* pred : predecessors(&basic_block)) { ... }
  for (auto* succ : successors(&basic_block)) { ... }
  ```

---

