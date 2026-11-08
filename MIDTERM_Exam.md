# CSSA Midterm - Quick Revision

Short English notes for learning by rewriting, not just rereading.

**Scope** Topic 0, Topic 1, Topic 2a, CW1, 3a, 3b, 3c, 3d. The supplied Topic 2 file covers **2a only**, not detailed
two's complement or IEEE-754. These notes are a study aid, not an official exam specification.

**Study loop:** read one section, close it, rewrite the idea in your own words, solve an example, then compare. Keep
your own examples below each section when you upload this to GitHub.

<!-- pdf-page: Architecture and Von Neumann -->

## 1. Foundations and performance

**Architecture** is the programmer-visible contract: instructions, registers and addressing modes. **Organization**
is the implementation: pipeline, cache sizes and physical circuitry. Two CPU's can share an ISA but have different performance.

```text

Transistors -> Gates -> Microarchitectute -> ISA -> OS -> Applications

```
Each layer provides services upward and hides implementation details below it. The **ISA** is the hardware/software boundary.

```text

CPU time = Instruction Count * CPI / Clock Freqeuncy
Speedup = Old execution time / New execution time
Amdahl = 1 / [(1 - p) + p/n]

```

`p` is the parallel fraction; `n` is the processor count. For `p=0.9, n=4`, speedup is about **3.08**; the infinite-processor
limit is **10**.

**Example:** 800 million instructions, CPI 2.5, 1.6 GHz: `(800e6 * 2.5) / 1.6e9 = 1.25s`.
