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

**Moore's law:** a historical observation about transistor-count growth, not a guarantee that clock speed or application
performance doubles. Technology progressed from tubes to transistors, ICs and microprocessors. RISC is not automatically 
faster than CISC: compare IC, CPI and clock together.

## 2. VON Neumann and the instruction cycle

**Stored program:** Instructions and data occupy the same memory and are both bit patterns. Context determines how the
CPU interprets a cell.

| Register | Remember its job | 
| --- | --- | 
| PC | Address of the next instruction | 
| IR / CIR | Current instruction |
| MAR | Address being read or written | 
| MDR | Data moving to or from memory |
| ACC | Arithmetic and logic working result |

**Address bus:** Where? **Data bus:** What value? **Control bus:** Read, write and status signals.

```text
Fetch: MAR <- PC; MDR <- Memory [MAR]; IR <- MDR
Decode -> Execute -> Write back -> Repeat
```
The PC advances by the **instruction lenght**, not necessarily one byte. A jump loads a new PC value. 
`MVI A,O5H; MVI B,08H; ADD B; STA 2000H` leaves `A=0 DH=13` AND `Memory[2000H]=0DH`.

**Bottleneck:** Code fetch and data access compete for a shared path. Harvard separates instruction/data
memories and paths; modifies Harvard may use one instruction stages; it is a different concept.

**Write it back:** Why are PC and IR different? Why can a lower-GHz CPU finish sooner?

<!-- pdf-page: Numbers and CW1 -->

## 3. Number Systems - Topic 2a

```text
Value = sum(digit * base ^ position)
Integer positions: ..., 2, 1, 0
Fraction positions: -1, -2, ...
```
Every digit must be smaller than its base. `128` is invalid in octal. An unsigned `n` -bit word has `2^n`
patterns and a range of `0 ... 2^n-1`. A byte is 8 bits; a nibble is 4. Word width depends on the architecture.

| Direction | Method | 
| --- | --- | 
| Any base -> decimal | Add the weighted digit values |
| Decimal integer -> base r | Repeated division by r; read remainders bottom-up |
| Decimal fraction -> base r | Repeated multiplication by r; read integer digits top-down | 
| Binary <-> octal | Group 3 bits from the radix point outward |
| Binary <-> hex | Group 4 bits from the radix point outward | 

Pad incomplete groups only at the outer ends: left of an integer, right of a fraction.

```text
152.25 decimal = 10011000.01 binary = 230.2 octal = 98.4 hex
10110.001 binary = 16 + 4 + 2 1/8 = 22.125 decimal
1101 0110 1010 binary = D6A hex
110 101 101 010 binary = 6552 octal = 3434 decimal
100 decimal = 202 base 7 = 10201 base 3
```
**Why does 0.1 repeat in binary?** Reduce the fraction first: `1/10`. A fraction terminates in base `r`
only if every prime factor of its reduced denominator is also a factor `r`. Binary has only factor 2;
denominator 10 also contains 5.

## 4. CW1 - vnmsim and verification

Do not mix vnmsim and MVI instruction set above. Vnmsim uses one accumulator, numeric jump targets and
**JMZ** as its conditional test.

| Intruction | Meaning | 
| --- | --- |
| LOD X / LOD #1 | Load a cell / immediate value into ACC |
|
