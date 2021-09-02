# RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY
![68747470733a2f2f7777772e766c736973797374656d64657369676e2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323032312f30352f566572696c6f672d666c7965722e706e67](https://user-images.githubusercontent.com/89927660/131698504-1010e82e-58c8-4474-bf88-191c120f36ad.png)

### About the Workshop
A cloud based workshop on verilog coding guidelines, resulting in predictable logic in silicon. The workshop insights include:
* Digital logic design using Verilog HDL 
* Functionality Validation of the design using Functional Simulation
* Creating Test Benches to validate the functionality of the RTL design 
* Logic synthesis of the Functional RTL Code
* Gate Level Simulation of the Synthesized Netlist

## Table of Contents
|DAY COUNT|TOPICS COVERED|
|:---:|:---:|
|[Day 1](#day-1)| Introduction to Verilog RTL Design and Synthesis|
|[Day 2](#day-2)| Timing Libs, Hierarchial Vs Flat Synthesis and Efficient Flop Coding Styles|
|[Day 3](#day-3)| Combinational and Sequential Optimizations|
|[Day 4](#day-4)| GLS, blocking vs non-blocking and Synthesis-Simulation mismatch|
|[Day 5](#day-5)| Optimization in synthesis|

#### Workshop Credits
> VSD - Kunal Ghosh and Team: https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/
----------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 1 

### _Highlights:_
|#|TOPIC COVERED|
|:---:|:---:|
|1.|[INTRODUCTION TO VERILOG RTL DESIGN AND SYNTHESIS](#introduction-to-verilog-rtl-design-and-synthesis)|
|2.|[DESIGN AND TEST BENCH MODULES](#design-and-test-bench-modules)|
|3.|[SIMULATION FLOW](#simulation-flow)|
|4.|[ENVIRONMENT SETUP](#environment-setup)|
|5.|[MULTIPLEXER REVIEW](#multiplexer-review)|
|6.|[IMPLEMENTATION OF A GOOD MUX USING IVERILOG](#implementation-of-a-good-mux-using-iverilog)|
|7.|[GTKWave ANALYSIS](#gtkwave-analysis)|
|8.|[ACESSING MODULE FILES](#accessing-module-files)|
|9.|[INTRODUCTION TO YOSYS AND LOGIC SYNTHESIS](#introduction-to-yosys-and-logic-synthesis)|
|10.|[DESIGN LOGIC SYNTHESIS](#design-logic-synthesis)|

### INTRODUCTION TO VERILOG RTL DESIGN AND SYNTHESIS

_Register Tranfer Level_ is a low-level abstraction representation of a design circuit. RTL design facilitates the designers by automating the design process. It converts the functionality of a design circuit written in Hardware Description Languages (HDL) to its equivalent combinational and sequential circuit. 
Fundamentally, Design is the actual Verilog code of set of Verilog codes which has intended functionalities to meet the specifications. RTL design is also checked for adherence to specification by simulating the design. While a _Simulator_ is used to verify or check the design, _Testbench_ is the setup to apply stimulus to the design to check its functionality. 

### DESIGN AND TEST BENCH MODULES

to insert block diagram 

>_**Note:** Design module may have one or more primary inputs and primary outputs. However, testbench will not have any primary input or output._  

### SIMULATION FLOW

to insert block diagram

>_**Note:** Simulator continuously checks for changes in the input. If there is an input change, the output is evaluated; else the simulator will never evaluate the output._

### ENVIRONMENT SETUP

```
#Steps Followed:
//create a directory
$ mkdir VLSI 
//Git Clone vsdflow. 
//Reference: https://github.com/kunalg123/vsdflow
$ git clone https://github.com/kunalg123/vsdflow.git
//Git Clone sky130RTLDesignAndSynthesisWorkshop. 
//Reference: https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

![Screen Shot 2021-09-01 at 5 40 21 PM](https://user-images.githubusercontent.com/89927660/131754679-41ae4441-1c3b-40ba-9876-fc5d2d655c91.png)

> _**Note:** sky130RTLDesignAndSynthesisWorkshop Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes._ 

### MULTIPLEXER REVIEW

### IMPLEMENTATION OF A GOOD MUX USING IVERILOG

```
#Steps Followed:
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog good_mux.v tb_good_mux.v 
//List so as to ensure that it has been added to the simulator
$ ls
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_good_mux.vcd
```
>_**Note:** The invoking command for iVerilog should have both source and testbench Verilog files. Once this command is run, it creates a.out file which dumps the vcd file. GTKwave viewer is used to open the vcd dump file and analyze the waveform._

![Screen Shot 2021-09-01 at 5 47 49 PM](https://user-images.githubusercontent.com/89927660/131755360-c66415a2-5135-456e-82ba-1b7c4a6c3a07.png)

### GTKWave ANALYSIS

![Screen Shot 2021-09-01 at 3 49 04 AM](https://user-images.githubusercontent.com/89927660/131778505-5929cc06-72a2-4d20-ae16-4c4cc72e0730.png)

>_**Note:** GTKWave is the waveform analyzer and is the primary tool used for visualization and thereby to check the design functionality._

### ACCESSING MODULE FILES

Additionally, the verilog and test bench modules can be accessed using the following command: 
```
$ gvim tb_good_mux.v -o good_mux.v 
```
![Screen Shot 2021-09-02 at 12 23 28 AM](https://user-images.githubusercontent.com/89927660/131786618-c6d4663f-5375-48dc-aa16-46b70e6797da.png)

>_**Note:** Vim is a highly configurable text editor built to enable efficient text editing. gvim brings all the functionality, power, and features of Vim while adding the convenience and intuitive nature of a GUI environment._

### INTRODUCTION TO YOSYS AND LOGIC SYNTHESIS
Synthesizer is the tool that helps to convert RTL to netlist. Netlist is the representation of the design in the form of standard cells (in the form of the cells present in the .lib). Design and netlist file should be one and the same. 

#### _But, How to Verify the Synthesis Output?_
The stimulus should be same as the output observed in the RTL simulation. The design was written in Verilog code and netlist is the standard cell representation of the code. The set of primary inputs and primary outputs will remain the same between the RTL design and synthesized netlist. It implies that, we can use the same testbench as RTL design. 

#### _Logic Synthesis_
RTL Design is the behavioral representation in HDL form for the required specification.

But How to map the code with the hardware circuit? Synthesis - RTL to gate level translation. The design is converted into gates and the interconnections are made between the gates. This is given out as a file named as netlist. We take the RTL design, combine with .lib and synthesis it to get the netlist file. 

>_Note: .lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._

#### _Cell Selection_
Fast and Slow cells comes with its own advantages and disadvantages when we consider the critical delays in combinational circuits. It is necessary to guide the synthesizer for selecting the cells that is optimum for implementing the logic circuits. This is called as _Constraints._ 

>_Note: Faster Cells lead to increased area and power, potentially leading to hold time violations. Slower Cells will result in slow circuits and may fail to meet the performance._

### DESIGN LOGIC SYNTHESIS 

```
#Steps Followed:
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog good_mux.v
//Synthesize Design
$ synth -top good_mux
//Generate Netlist
$ abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic
$ show
//Writing the netlist in a crisp manner 
$ write_verilog -noattr good_mux_netlist.v
```

**_Screenshot: Steps for Design Synthesis_**

![Screen Shot 2021-09-02 at 12 16 52 AM](https://user-images.githubusercontent.com/89927660/131785975-bbc0c874-8b81-4f29-b892-3b279c7dbc6c.png)

**_Screenshot: Inference from Synthesis and Execution of netlist generation_**

![Screen Shot 2021-09-02 at 12 16 16 AM](https://user-images.githubusercontent.com/89927660/131785899-90b9720f-cd4f-489e-910e-dc9003137ef3.png)

**_Screenshot: Inference from abc command - Synthesized Netlist_**

![Screen Shot 2021-09-02 at 12 15 00 AM](https://user-images.githubusercontent.com/89927660/131785819-64cecf46-fef0-476d-af9c-25db68c1505a.png)

**_Screenshot: Graphical Representation of the Logic using show command_**

![Screen Shot 2021-09-02 at 12 14 23 AM](https://user-images.githubusercontent.com/89927660/131785750-c8f15d7f-eee1-4997-b825-52c677ff8df3.png)

>_**Observation:** The Generated circuit thus has 2input NAND gate, Or And Inverted and Inverter. The output of this circuit is i1sel + i0sel'_

**_Screenshot: Writing the Netlist_**

![Screen Shot 2021-09-02 at 12 27 54 AM](https://user-images.githubusercontent.com/89927660/131787026-c7ba62b7-a7d1-4cee-a23b-319e7af7c2f5.png)

-----------------------------------------------------------------------------------------------------------------------------------------------
# Day 2

### _Highlights:_
|#|TOPIC COVERED|
|:---:|:---:|
|1.|[UNDERSTANDING THE LIBRARY](#understanding-the-library)|

### UNDERSTANDING THE LIBRARY

Command to open the library used in this workshop: sky130_fd_sc_hd__tt_025C_1v80.lib
```
$ gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//to shut off the background colors/ syntax off:
: syn off
```

For a design to work, there are three important parameters that determines how the Silicon works: Process (Variations due to Fabrications), Voltage (Changes in the behavior of the circuit) and Temperature (Sensitivity of semiconductors). Libraries are characterized to model these variations. 

>sky130_fd_sc_hd__tt_025C_1v80.lib

|PARAMETER|MEANING|
|:---:|:---:|
|SKY130|Technology - CMOS|
|fd|Foundary - Skywater|
|sc|Standard Cell - Digital|
|hd|Density - High|
|tt|Process - Typical|
|025C|Temperature - Measure|
|1v80|Voltage - Measure|

.lib is a bucket of all standard cells that are available. The file also provides for every flavor of cells. 







