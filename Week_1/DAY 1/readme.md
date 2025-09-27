
# Table of contents
## Table of Contents
- [1. Introduction to iverilog](#1-introduction-to-iverilog)
- [2. Environment Setup](#2-environment-setup)
- [3. Simulation Process](#3-simulation-process)
- [4. Introduction To Yosys](#4-introduction-to-yosys)
- [5. Yosys Synthesis flow](#5-yosys-synthesis-flow)

## 1. INTRODUCTION TO IVERILOG
#### Simulator
* RTL design is validated by simulating its behavior to ensure it adheres to the specifications.
* The simulation tool used in this program is Icarus Verilog (iverilog).

#### Design 
* The design refers to the Verilog code or collection of Verilog modules.
* It implements the intended functionality to satisfy the required specifications.

#### Testbench
* A testbench is used to provide test inputs (stimuli) to the design.
* It simulates the input signals and monitors the design’s output to ensure correct functionality.
* Note that a testbench does not have any primary input or output ports.
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/TEST%20BENCH.png)

#### Verilog-based Simulation Flow
* Write the Design (RTL Verilog module) and a Testbench to provide input stimuli and monitor outputs.
* Compile both files using the iverilog tool to generate a simulation executable.
* Run the executable to produce a Value Change Dump (VCD) file recording signal transitions.
* Open the VCD file with gtkwave to visually analyze and debug signal behavior for correctness.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/iverilog%20design%20flow.png)

--- 

## 2. ENVIRONMENT SETUP
* Create a new folder named VSD
  
* Open the terminal inside the VSD folder and navigate into it to verify that all files are correctly stored.
  ~~~
  cd VSD
* Clone the workshop repository
   ~~~
   git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/environment.png)

---

## 3. SIMULATION PROCESS
* Providing the RTL design and its corresponding testbench as inputs to the Iverilog simulator.
  
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/comand%20for%20the%20iverilog.png)
  
* Generating the Value Change Dump (VCD) file and visualizing it using GTKWave.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-23%2021-31-39.png)

* Displaying the simulation waveform using GTKWave for visual analysis.

  ![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/gtk%20window.png)

* Execute the following command to view both the tb_good_mux.v -o good_mux.v code.
  ~~~
  gvim tb_good_mux.v -o good_mux.v
  ~~~

## 4. INTRODUCTION TO YOSYS
 #### What is yosys?
  * Yosys is a tool used for converting the RTL Drsign to Netlist.

 ![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%202025-09-23%20233236.png)

#### Verification of Synthesis
  * Testbench used is same as the RTL testbench.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%202025-09-23%20231603.png)

* If the output is same as RTL output the netlist file is correct.

## 5. Yosys Synthesis flow

  This workflow illustrates performing RTL-to-gate-level synthesis using the Yosys open synthesis suite along with the sky130_fd_sc_hd standard cell library.
  
#### Step 1 : Inovoke Yosys
~~~
 yosys
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/yosys%20invoke.png)
#### Step 2 : Read Liberty File
  You are loading the Liberty file for the Sky130 standard cell library, which is essential for mapping the synthesized netlist to technology-specific cells.
~~~
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/readliberty.png)
#### Step 3 : Read RTL Verilog
You are loading the Verilog file containing your RTL design, and if it is read successfully, you should see the message "Successfully finished verilog frontend."
~~~
read_verilog good_mux.v
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/read%20verilog.png)
#### Step 4 : Synthesize the top-level module 
You are synthesizing the top module, transforming the RTL design into a gate-level netlist that is independent of the target technology.
~~~
synth -top good_mux
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/synth.png)
#### Step 5 : Map synthesized RTL to standard cells 
You are converting the synthesized netlist into a technology-dependent netlist by mapping it to the standard cells specified in the Liberty file.
~~~
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/abc.png)
#### Step 6 : View synthesized netlist as a schematic
You can display the synthesized netlist as a schematic diagram, which visually represents the logic implemented by the synthesis tool.
~~~
show
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/show.png)
#### step 7 : Write the synthesized gate-level netlist 
You are exporting the synthesized gate-level netlist to a Verilog file while omitting attributes to keep the code clean and more readable.
~~~
 write_verilog -noattr good_mux_netlist.v
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/write%20verilog.png)
##### To see the netlist file use 
~~~
!gvim good_latch_netlist.v
~~~
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/gvim%20mux.png)




### Summary

This document covers the fundamental steps and tools involved in RTL design simulation and synthesis for digital circuits, primarily focusing on Verilog, Icarus Verilog (iverilog), and Yosys synthesis.

1. **Introduction to Icarus Verilog**:
   Explains the simulation process where RTL designs and testbenches are compiled and executed to verify design correctness. It introduces the concept of testbenches and how signal waveforms are analyzed using GTKWave.

2. **Environment Setup**:
   Guides through creating a working directory, cloning the required repository, and setting up the necessary files and environment to start simulations and synthesis.

3. **Simulation Process**:
   Describes compiling RTL and testbench files with iverilog, generating VCD files to capture signal changes, and using GTKWave for waveform visualization to debug and validate designs.

4. **Introduction to Yosys**:
   Introduces Yosys as an RTL-to-gate-level synthesis tool, explains how synthesized netlists are verified using existing testbenches by comparing outputs.

5. **Yosys Synthesis Flow**:
   Provides a step-by-step guide to using Yosys for synthesis including:

   * Loading the standard cell Liberty file for technology mapping.
   * Reading the RTL Verilog design.
   * Synthesizing the top-level module to a technology-independent netlist.
   * Mapping the netlist to technology-specific standard cells.
   * Visualizing the synthesized netlist.
   * Exporting the final gate-level netlist to a Verilog file.

This workflow helps digital designers validate and convert high-level RTL code into optimized gate-level netlists suitable for ASIC or FPGA implementation.



