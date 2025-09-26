# INTRODUCTION TO TIMING LIBRARIES

 A .lib file (Liberty file) is a text-based technology library that defines the functionality, timing, power, and area of standard cells, and is used in synthesis, timing, and power analysis.
 ![tools check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/image.png)

## what is PVT?
Three key factors that affect how a circuit
 p -> process.

 v -> voltage.
 
 T -> Temperature.

 ## What is sky130_fd_sc_hd__tt_025C_1v80.lib?
 The file sky130_fd_sc_hd__tt_025C_1v80.lib is a Liberty timing library for the SkyWater 130nm (sky130) standard cell library.
| Term     | Parameter   | Value            |
|----------|-------------|------------------|
| **tt**   | Process     | Typical-Typical  |
| **1v80** | Voltage     | 1.8 V            |
| **025C** | Temperature | 25°C             |

# Hierarchical and Flat Synthesis
Here, we are gooing to see what is hierarchial and flat synthesis.

## Hierarchical Synthesis
Hierarchical Synthesis is a method where the design is broken into modules (blocks) and each block is synthesized separately before combining them into a top-level design.
 Example of Multiple module Files
~~~
// Module 1 
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule
// Module 2
module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule
// multiple module
module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
  sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
 ~~~

![tools check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-25%2007-20-31.png)

### Why nand gate is preferred?
For basic gates such as OR, hierarchical synthesis prefers NAND + NOT implementations because:
* NAND gates rely on stacked NMOS transistors, which are faster.
* Stacked NMOS devices offer better performance than stacked PMOS devices.
* PMOS transistors are typically 2–3× weaker than NMOS due to lower carrier mobility.
* A NOR gate requires stacked PMOS transistors, leading to higher resistance and slower operation.
* NAND gates provide better scalability as the number of inputs increases.

 ## Flat Synthesis
Flat Synthesis is a synthesis approach where the entire design hierarchy is flattened into a single-level netlist, and all logic is optimized together without preserving module boundaries.

~~~
flatten
~~~
This command is used to flatten the hierachial design.
~~~
module multiple_modules(a, b, c, y);
  input a;
  input b;
  input c;
  wire net1;
  wire \u1.a ;
  wire \u1.b ;
  wire \u1.y ;
  wire \u2.a ;
  wire \u2.b ;
  wire \u2.y ;
  output y;
  assign \u1.y  = \u1.b  & \u1.a ;
  assign \u2.y  = \u2.b  | \u2.a ;
  assign \u1.a  = a;
  assign \u1.b  = b;
  assign net1 = \u1.y ;
  assign \u2.a  = net1;
  assign \u2.b  = c;
  assign y = \u2.y ;
endmodule

~~~
![tools check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-25%2021-01-15.png)

## Hierachy VS Flatten
| Aspect              | Hierarchical Synthesis                          | Flat Synthesis                           |
|----------------------|------------------------------------------------|------------------------------------------|
| **Design Structure** | Keeps module boundaries (hierarchy preserved)  | Removes hierarchy → single-level netlist  |
| **Optimization**     | Optimized block by block (local optimizations) | Optimized globally across whole design    |
| **Runtime & Memory** | Lower, scales well for large designs           | Higher, may be slow for large designs     |
| **Reusability**      | Easy to reuse pre-synthesized IPs/blocks       | No direct reuse; everything is re-flattened |
| **Debugging**        | Easier (debug at module level)                 | Harder (loss of hierarchy)                |
| **Performance (QoR)**| May miss cross-module improvements             | Better QoR due to full visibility         |
| **Best Suited For**  | Complex, large designs with reusable IPs       | Small to medium designs needing max optimization |

# Sub Module level Synthesis
Each sub-module is synthesized independently, optimized, and then the synthesized sub-modules are integrated or composed together to form the complete design.
## synthesing sub module 1
~~~
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
~~~
![tools check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-25%2021-18-07.png)
## why sub module synthesis?

* **Improves Modularity:** Easier to design, debug, and maintain smaller, independent blocks.
* **Speeds Up Synthesis:** Synthesizing smaller sub-modules is faster than synthesizing a large monolithic design.
* **Enables Incremental Compilation:** Only re-synthesize changed sub-modules, saving time during iterations.
* **Supports Reusability:** Synthesized sub-modules can be reused across multiple projects without re-synthesis.
* **Allows Targeted Optimization:** Apply specific optimization strategies to different sub-modules based on their needs.
* **Facilitates Parallel Development:** Multiple teams or engineers can work simultaneously on different sub-modules.
* **Enhances Verification:** Easier to verify and validate smaller sub-modules individually before integration.

# Various Flip Flops codinig styles and optimization
| Flip-Flop Type     | Reset/Set Behavior  | Trigger Condition                   | Use Case                               |
| ------------------ | ------------------- | ----------------------------------- | -------------------------------------- |
| Asynchronous Reset | Immediate reset     | `rst` signal (independent of clock) | Fast initialization or emergency reset |
| Asynchronous Set   | Immediate set       | `set` signal (independent of clock) | Immediate forcing of state             |
| Synchronous Reset  | Reset on clock edge | `rst` evaluated on clock edge       | Controlled, glitch-free reset          |
| Synchronous Set    | Set on clock edge   | `set` evaluated on clock edge       | Controlled, synchronous state set      |


## Flip FLops coding styles and Synthesis
commands for synthesis 
~~~
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v 
synth -top dff_asyncres.v
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
~~~
### 1. Asynchronous Reset Flip-Flops
   
```verilog
   always @(posedge clk or posedge rst) begin
    if (rst)
        q <= 0;
    else
        q <= d;
end
```
![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-25%2022-12-09.png)

![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-26%2014-12-49.png)
### 2. Asynchronous Set Flip-Flop.
```verilog
always @(posedge clk or posedge set) begin
    if (set)
        q <= 1;
    else
        q <= d;
end
```
![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-26%2014-44-43.png)
![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-26%2014-59-25.png)
### 3. Synchronous Reset Flip-Flop.
```verilog
always @(posedge clk) begin
    if (rst)
        q <= 0;
    else
        q <= d;
end
```
![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-26%2015-53-49.png)
![tool check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-26%2015-43-35.png)
### 4. Synchronous Set Flip-Flop.
```verilog
always @(posedge clk) begin
    if (set)
        q <= 1;
    else
        q <= d;
end
```
![tool check]()
![tool check]()







