# Combinational and Sequential optimisation


## combination logic optimisation
combinational optimization is about simplifying and improving combinational circuits for better area, speed, and power efficiency,which is critical for building efficient chips.

**why combinational logic optimisation?**


Squeeze and simplify logic to get the most optimized design in terms of area and power.

### Constant Propagation


**Constant propagation** is an optimization technique where known constant values (0 or 1) in the inputs or signals of a combinational logic circuit are substituted directly to simplify the circuit’s logic.


#### How it works

If an input signal is constant, we can replace it with that constant value in the logic expression, which often reduces the complexity of the circuit.


#### Example

Consider the logic expression:

$$
Y = \overline{(A \cdot B + C)}
$$

If the input $A = 0$, then:

$$
Y = \overline{(0 \cdot B + C)} = \overline{C} = C'
$$

This simplification happens because $A \cdot B = 0$ when $A = 0$, so the expression reduces to $C'$.

#### Benefits

* Simplifies logic expressions
* Reduces number of gates and transistors used
* Decreases power consumption and delay
* Helps improve overall circuit efficiency
---
## Boolean Logic Optimisation

**Boolean logic optimization** is the process of simplifying Boolean expressions or logic circuits to make them more efficient. The goal is to reduce the number of logic gates, minimize circuit complexity, improve speed, and lower power consumption without changing the output behavior.


### Why optimize Boolean logic?

* To **reduce hardware cost** by using fewer gates.
* To **improve circuit speed** by minimizing the number of logic levels.
* To **reduce power consumption**.
* To **simplify design and verification**.


### How is it done?

* Using algebraic simplifications (like Boolean identities).
* Applying Karnaugh maps (K-maps).
* Using algorithmic methods like the Quine-McCluskey algorithm.
* Using logic synthesis tools in CAD software.

### Example

Original expression:

$$
F = A \cdot B + A \cdot \overline{B}
$$

Using Boolean laws:

$$
F = A \cdot (B + \overline{B}) = A \cdot 1 = A
$$

The expression is simplified from two terms to just one.

---

## Sequential logic Optimisation

**Sequential Logic Optimization** is the process of improving sequential circuits by minimizing the number of states, simplifying the state transitions, and reducing the hardware required for memory elements and combinational logic, all while preserving the original functionality of the circuit.

### Types of Sequential Logic Optimization
 * sequential constant propagation
 * State diagram
 * Retiming
 * Sequential logic cloning

### Sequential constant  propagation
Great questions! You're touching on several important **sequential logic optimization techniques** used in digital design and synthesis. Let’s break down each one with clear definitions:

---

### 1.  **Sequential Constant Propagation**

**Definition:**
Sequential constant propagation is the process of identifying and replacing **signals or registers that hold constant values over time** (like always 0 or always 1) with their constant values throughout the circuit.

**Purpose:**

* Reduces unnecessary logic and registers
* Simplifies control logic
* May enable further optimizations (like state reduction)

**Example:**
If a flip-flop always outputs `0`, all logic driven by it can treat it as a constant, allowing gate-level simplification.

---

### 2.  **Retiming**

**Definition:**
**Retiming** is an optimization technique where flip-flops are **moved across combinational logic** to balance delays and improve timing (i.e., reduce the critical path), without changing the circuit’s input/output behavior.

**Purpose:**

* Increases maximum clock speed
* Balances timing across the design
* Reduces logic depth between flip-flops


### 3.  **State Optimization (FSM Optimization)**

**Definition:**
**State optimization** involves minimizing the number of states or transitions in a **finite state machine (FSM)** while keeping the same behavior.

**Types include:**

* **State minimization:** Merge equivalent or redundant states
* **State encoding optimization:** Choose optimal binary encodings for states to simplify logic

**Benefits:**

* Fewer flip-flops
* Simpler next-state and output logic
* Reduced area and power


### 4.  **Sequential Logic Cloning**

**Definition:**
**Sequential logic cloning** is a technique where certain registers or logic blocks are **duplicated (cloned)** to reduce fanout or improve timing by distributing the load more evenly.

**Purpose:**

* Helps manage large fanout nets (e.g., one flip-flop driving too many gates)
* Improves timing closure in large designs
* Sometimes used in pipelined or replicated architectures

**Example:**
One register `R` drives 100 gates. Instead, clone `R` into `R1` and `R2`, each driving 50 gates, reducing the load per driver.


| Technique                           | Purpose                                      | Result                      |
| ----------------------------------- | -------------------------------------------- | --------------------------- |
| **Sequential Constant Propagation** | Replace registers with known constant values | Simplified logic            |
| **Retiming**                        | Move flip-flops to optimize timing           | Faster clock speeds         |
| **State Optimization**              | Minimize number of states or encoding        | Smaller FSMs                |
| **Sequential Logic Cloning**        | Duplicate logic to manage load/fanout        | Better timing & scalability |



## LAB 1:

The files we are using in the labs are 

![Tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2012-57-37.png)
--

The command used for logic optimization is 
~~~
opt_clean -purge
~~~

We used the opt_check.v file for this lab

![tools](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2013-14-53.png)
--

#### Explanation:

- assign y = a ? b : 0; means:
- If a is true, y is assigned the value of b.
- If a is false, y is 0.

Use the following commands to opt and syth the opt_check.v file 
~~~
read_liberty -lib ~/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
dfflibmap -liberty ~/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ~/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
~~~
The optimisied .dot file shows that the opt_check.v is just a AND gate 
![Tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2013-11-20.png)
--

### LAB 2:
 The file used is opt_check2.v
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2013-27-54.png)
 --
 
 Acts as a multiplexer:
- y = 1 if a is true.
- y = b if a is false.
  
 This code is just a **or** between a and b. 
 
 so, the optimized version is just a or between a and b
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2013-32-23.png)
 --

 For file opt_check3.v
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2014-57-37.png)
 --

Three inputs (a, b, c), output y.

Nested ternary logic:

If a = 1, y = c.

If a = 0, y = !c.

Logic simplifies to:

y = a ? c : !c

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-27%2015-03-18.png)
