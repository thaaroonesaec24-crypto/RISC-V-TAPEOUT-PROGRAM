# GLS,Blocking vs Non-Blocking and Synthesis Simulation Mismatch

## Gate-level Simulastion

 * Running the test-bench with netlist as design under test.
 * Netlsit is logically same as RTL code

### Why GLS ?

 * To Verify the logical correctness of the design after synthesis.
 * Gate-level simulation is crucial to ensure the design will work correctly in real silicon, not just in theory.
 ---
 
### GLS using IVERILOG

 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/image.png)

 ## SYnthesis Simulation Mismatch 
   A synthesis-simulation mismatch in digital design is when the behavior of an RTL (Register Transfer Level) description observed during simulation does not match the actual hardware behavior after synthesis,
   ### Why it happens?
    * Missing sensitive list.
    * Blocking VS Non-Blocking Assignments.
    * Non Standard Verilog coding.


## Missing Sensitivity List

### What is a Missing Sensitivity List?

In Verilog, the **sensitivity list** of an `always` block defines the signals that trigger its execution.
If signals that affect the output are **missing** from this list, the output won't update properly in simulation when those signals change.

This can lead to **incorrect simulation results** even if the synthesized hardware works correctly.

---

### Example: Mux with Missing Sensitivity List

```verilog
module mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(sel) // Incorrect sensitivity list: missing i0, i1
begin
    if (sel)
        y = i1;
    else
        y = i0;
end

endmodule
```

**Problem:**
The `always` block only triggers on changes to `sel`. Changes in `i0` or `i1` won’t update `y`, causing simulation mismatches.

### Corrected Version: Using `always @(*)`

```verilog
module mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*) // Automatically includes i0, i1, sel
begin
    if (sel)
        y = i1;
    else
        y = i0;
end

endmodule
~~~
