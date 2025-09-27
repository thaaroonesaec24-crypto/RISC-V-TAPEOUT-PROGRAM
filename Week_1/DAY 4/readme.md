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
~~~
verilog
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




## Blocking vs Non-Blocking Assignments

### What Causes Mismatches?
 **Blocking (`=`)** and **non-blocking (`<=`)** assignments behave differently in simulation, which can sometimes lead to mismatches between **simulation results** and **actual synthesized hardware behavior** if used incorrectly.


### Blocking (`=`) vs Non-Blocking (`<=`) Assignments

| Feature           | Blocking (`=`)                                                                  | Non-Blocking (`<=`)                                                        |
| ----------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Execution order   | Executes statements **sequentially**, blocking the next until current completes | Executes statements **concurrently**, all RHS evaluated before LHS updated |
| Usage             | Typically used in **combinational logic** or testbenches                        | Used in **sequential logic** (inside clocked always blocks)                |
| Simulation result | Updates immediately, can cause unintended ordering effects                      | Updates after all RHS are evaluated, modeling parallel hardware behavior   |

### Why Mismatches Happen?

* Using blocking assignments (`=`) in sequential (clocked) blocks can cause simulation to show behavior different from hardware, as hardware updates all flip-flops in parallel on clock edges.
* Using non-blocking (`<=`) incorrectly in combinational logic may cause unintended delays or latches.

### Example of Mismatch

~~~
verilog
module mismatch_example(
    input clk,
    input d,
    output reg q1,
    output reg q2
);

// Incorrect use of blocking assignments in sequential logic
always @(posedge clk) begin
    q1 = d;     // Blocking assignment
    q2 = q1;    // q2 immediately gets the updated q1 in simulation
end

endmodule
~~~

**Simulation behavior:**

* `q1` gets `d`
* `q2` immediately gets the new value of `q1` (which is `d`), so `q2` follows `q1` in the same cycle.

**Synthesized hardware behavior:**

* Both `q1` and `q2` are flip-flops clocked together; `q2` will get the **previous** value of `q1` (before clock edge), not the updated one.



### Corrected Code Using Non-Blocking Assignments
~~~
verilog
module mismatch_fixed(
    input clk,
    input d,
    output reg q1,
    output reg q2
);

always @(posedge clk) begin
    q1 <= d;     // Non-blocking assignment
    q2 <= q1;    // q2 gets q1's value from previous cycle
end

endmodule
~~~

**Result:**

* Simulation matches synthesized hardware: `q2` always lags `q1` by one clock cycle.

---

## Labs on Gate-level Simulation
**ternary_operator_mux.v-> GLS**
~~~
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
~~~

 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2020-00-08.png)
-

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2020-08-32.png)
-

The commands to run GLS simulation is

~~~
iverilog -o ~/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
~~~

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2020-15-40.png)
-


### bad_mux.v GLS

~~~
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
~~~
The output wave after synthsis

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2020-52-25.png)

## Blocking Assignment Caveat

**verilog code for blocking caveat :**

~~~
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
~~~

**Simulation for blocking_caveat**

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2021-02-44.png)
-

**Dot file of blocking_caveat**

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2021-09-22.png)
-

**GLS of blocking_caveat**

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY%204/pictures/Screenshot%20from%202025-09-27%2021-16-57.png)
-
