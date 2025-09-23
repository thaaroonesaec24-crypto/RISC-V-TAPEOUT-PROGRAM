#DAY ONE
## INTRODUCTION TO IVERILOG
### Simulator
* RTL design is validated by simulating its behavior to ensure it adheres to the specifications.
* The simulation tool used in this program is Icarus Verilog (iverilog).

### Design 
* The design refers to the Verilog code or collection of Verilog modules.
* It implements the intended functionality to satisfy the required specifications.

### Testbench
* A testbench is used to provide test inputs (stimuli) to the design.
* It simulates the input signals and monitors the design’s output to ensure correct functionality.
* Note that a testbench does not have any primary input or output ports.
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/TEST%20BENCH.png)

### Verilog-based Simulation Flow
* Write the Design (RTL Verilog module) and a Testbench to provide input stimuli and monitor outputs.
* Compile both files using the iverilog tool to generate a simulation executable.
* Run the executable to produce a Value Change Dump (VCD) file recording signal transitions.
* Open the VCD file with gtkwave to visually analyze and debug signal behavior for correctness.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/iverilog%20design%20flow.png)

--- 

## ENVIRONMENT SETUP
* Create a new folder named VSD
  
* Open the terminal inside the VSD folder and navigate into it to verify that all files are correctly stored.
  ~~~
  cd VSD
* Clone the workshop repository
   ~~~
   git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/environment.png)

## SIMULATION PROCESS
* Providing the RTL design and its corresponding testbench as inputs to the Iverilog simulator.
  
![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/comand%20for%20the%20iverilog.png)
  
* Generating the Value Change Dump (VCD) file and visualizing it using GTKWave.

![Tools Check](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/Pictures/Screenshot%20from%202025-09-23%2021-31-39.png)

* Displaying the simulation waveform using GTKWave for visual analysis.

  ![Tools Check]()

  




 

