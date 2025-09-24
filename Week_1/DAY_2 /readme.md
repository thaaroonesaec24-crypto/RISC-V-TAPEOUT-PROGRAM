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
endmodule ~~~





