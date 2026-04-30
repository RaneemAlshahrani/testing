## 1. Go to the folder containing `bzip2.c`

```bash
cd ~/Downloads
```

**Explanation:**
Moves the terminal into the `Downloads` folder because your `bzip2.c` file is stored there.

---

## 2. Check the files in the folder

```bash
ls
```

**Explanation:**
Lists the files in the current folder. You used it to confirm that `bzip2.c` exists.

---

## 3. Install LLVM/Clang 14

```bash
sudo apt install clang-14
```

**Explanation:**
Installs `clang-14`, `opt-14`, and LLVM tools. You needed this because `clang-18` was not available on your Ubuntu system.

---

## 4. Compile C source code into LLVM bitcode

```bash
clang-14 -g -O0 -fno-discard-value-names -c -emit-llvm bzip2.c -o bzip2.bc
```

**Explanation:**
Converts `bzip2.c` into LLVM bitcode file `bzip2.bc`.

Meaning of options:

```text
-g                         includes debug/source line information
-O0                        disables optimization
-fno-discard-value-names   keeps variable/function names readable
-c                         compile only, do not link
-emit-llvm                 output LLVM bitcode instead of normal object code
-o bzip2.bc                name the output file bzip2.bc
```

---

## 5. Install Graphviz

```bash
sudo apt install graphviz
```

**Explanation:**
Installs the `dot` command, which converts `.dot` graph files into readable PDF or PNG diagrams.

---

## 6. Generate the call graph

```bash
opt-14 -enable-new-pm=0 -dot-callgraph bzip2.bc
```

**Explanation:**
Uses LLVM `opt` to generate the program’s call graph from `bzip2.bc`.

Because LLVM 14 uses the older pass style, you needed:

```text
-enable-new-pm=0
```

This produced:

```text
bzip2.bc.callgraph.dot
```

---

## 7. Show hidden and normal files

```bash
ls -a
```

**Explanation:**
Lists all files, including hidden files that start with `.`. LLVM creates CFG files as hidden files, like:

```text
.uncompressStream.dot
.BZ2_bzWrite.dot
```

---

## 8. Convert call graph DOT file to PDF

```bash
dot -Tpdf bzip2.bc.callgraph.dot -o bzip2_callgraph.pdf
```

**Explanation:**
Converts the call graph from DOT format into a PDF diagram.

Input:

```text
bzip2.bc.callgraph.dot
```

Output:

```text
bzip2_callgraph.pdf
```

---

## 9. Open the call graph PDF

```bash
xdg-open bzip2_callgraph.pdf
```

**Explanation:**
Opens the generated call graph PDF so you can view it.

---

## 10. Generate CFGs for all functions

```bash
opt-14 -enable-new-pm=0 -dot-cfg bzip2.bc
```

**Explanation:**
Generates Control Flow Graph DOT files for every function in `bzip2.c`.

This creates many hidden `.dot` files, including the required ones:

```text
.uncompressStream.dot
.BZ2_bzWrite.dot
```

---

## 11. Rename required CFG files

```bash
mv .uncompressStream.dot uncompressStream_cfg.dot
mv .BZ2_bzWrite.dot BZ2_bzWrite_cfg.dot
```

**Explanation:**
Renames the two required CFG files to clearer names.

Before:

```text
.uncompressStream.dot
.BZ2_bzWrite.dot
```

After:

```text
uncompressStream_cfg.dot
BZ2_bzWrite_cfg.dot
```

---

## 12. Convert CFG DOT files to PDF

```bash
dot -Tpdf uncompressStream_cfg.dot -o uncompressStream_cfg.pdf
dot -Tpdf BZ2_bzWrite_cfg.dot -o BZ2_bzWrite_cfg.pdf
```

**Explanation:**
Converts the required CFG DOT files into PDF diagrams.

Outputs:

```text
uncompressStream_cfg.pdf
BZ2_bzWrite_cfg.pdf
```

---

## 13. Open the CFG PDFs

```bash
xdg-open uncompressStream_cfg.pdf
xdg-open BZ2_bzWrite_cfg.pdf
```

**Explanation:**
Opens the two required CFG diagrams.
