# Simulator
* RTL design is validated by simulating its behavior to ensure it adheres to the specifications.
* The simulation tool used in this program is Icarus Verilog (iverilog).

# Design 
* The design refers to the Verilog code or collection of Verilog modules.
* It implements the intended functionality to satisfy the required specifications.

# Testbench
* A testbench is used to provide test inputs (stimuli) to the design.
* It simulates the input signals and monitors the design’s output to ensure correct functionality.
* Note that a testbench does not have any primary input or output ports.

# Verilog-based Simulation Flow
The iverilog simulation process starts with writing the Design (RTL Verilog module) along with a Testbench that provides input stimuli to the design and monitors the outputs. These files are compiled together using the iverilog tool to create a simulation executable. Running this executable generates a Value Change Dump (VCD) file, which logs the signal changes over time. This VCD file can then be opened with gtkwave, a waveform viewer, enabling designers to visually analyze and debug the signal behavior to confirm the design’s correct operation.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/iverilog%20design%20flow.png)

