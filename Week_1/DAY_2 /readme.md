# INTRODUCTION TO TIMING LIBRARIES

 A .lib file (Liberty file) is a text-based technology library that defines the functionality, timing, power, and area of standard cells, and is used in synthesis, timing, and power analysis.

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




