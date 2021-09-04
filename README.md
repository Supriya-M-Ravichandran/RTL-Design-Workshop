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
|#|TOPICS COVERED|
|:---:|:---:|
|1.|[INTRODUCTION TO VERILOG RTL DESIGN AND SYNTHESIS](#introduction-to-verilog-rtl-design-and-synthesis)|
|2.|[DESIGN AND TEST BENCH MODULES](#design-and-test-bench-modules)|
|3.|[SIMULATION FLOW](#simulation-flow)|
|4.|[ENVIRONMENT SETUP](#environment-setup)|
|5.|[MULTIPLEXER REVIEW](#multiplexer-review)|
|6.|[GOOD MUX IMPLEMENTATION USING IVERILOG](#good-mux-implementation-using-iverilog)|
|7.|[GTKWave ANALYSIS](#gtkwave-analysis)|
|8.|[ACESSING MODULE FILES](#accessing-module-files)|
|9.|[INTRODUCTION TO YOSYS AND LOGIC SYNTHESIS](#introduction-to-yosys-and-logic-synthesis)|
|10.|[DESIGN LOGIC SYNTHESIS](#design-logic-synthesis)|

### INTRODUCTION TO VERILOG RTL DESIGN AND SYNTHESIS

_Register Tranfer Level_ is a low-level abstraction to represent a design circuit. RTL design facilitates the designers by automating the design process. It converts the functionality of a design circuit written in Hardware Description Languages (HDL) into equivalent combinational and sequential circuit. 
Fundamentally, Design is the actual Verilog code of set of Verilog codes which has intended functionalities to meet the specifications. RTL design is also checked for adherence to specification by simulating the design. While a _Simulator_ is used to verify or check the design, _Testbench_ is the setup to apply stimulus to the design to check its functionality. 

### DESIGN AND TEST BENCH MODULES

![Screen Shot 2021-09-02 at 10 50 36 AM](https://user-images.githubusercontent.com/89927660/131876207-313df749-a7c5-46e8-9463-f7a2b41a6dc3.png)

>_**Note:** Design module may have one or more primary inputs and primary outputs. However, testbench will not have any primary input or output._  

### SIMULATION FLOW

![Screen Shot 2021-09-02 at 10 57 07 AM](https://user-images.githubusercontent.com/89927660/131877209-116b4362-b56d-46ed-9f42-c7981a027158.png)

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

### GOOD MUX IMPLEMENTATION USING IVERILOG

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
Synthesizer (Yosys) is the tool that helps to convert RTL to netlist. Netlist is the representation of the design in the form of standard cells (in the form of the cells present in the .lib). Design and netlist file should be one and the same. Logic synthesis is the optimiztion stage during the CAD process where the RTL code is being transformed into netlist. 

![Screen Shot 2021-09-02 at 11 10 58 AM](https://user-images.githubusercontent.com/89927660/131879381-e7f010a5-545d-49a8-ae76-6d60ed221732.png)

#### _But, How to Verify the Synthesis Output?_
The stimulus should be same as the output observed in the RTL simulation. The design was written in Verilog code and netlist is the standard cell representation of the code. The set of primary inputs and primary outputs will remain the same between the RTL design and synthesized netlist. It implies that, we can use the same testbench as RTL design.

![Screen Shot 2021-09-02 at 11 00 19 AM](https://user-images.githubusercontent.com/89927660/131877685-e2bba5af-59ac-4817-ab84-fb07556676da.png)

#### _Logic Synthesis_
RTL Design is the behavioral representation in HDL form for the required specification.

But How to map the code with the hardware circuit? Synthesis - RTL to gate level translation. The design is converted into gates and the interconnections are made between the gates. This is given out as a file named as netlist. We take the RTL design, combine with .lib and synthesis it to get the netlist file. 

>_**Note:** .lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._

#### _Cell Selection_
Fast and Slow cells comes with its own advantages and disadvantages when we consider the critical delays in combinational circuits. It is necessary to guide the synthesizer for selecting the cells that is optimum for implementing the logic circuits. This is called as _Constraints._ 

>_**Note:** Faster Cells lead to increased area and power, potentially leading to hold time violations. Slower Cells will result in slow circuits and may fail to meet the performance._

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

>_**Observation:** The Generated circuit thus has 2-input NAND, OR-AND-Inverted and NOT gates. The output of this circuit is i1sel + i0sel'_

**_Screenshot: Writing the Netlist_**

![Screen Shot 2021-09-02 at 12 27 54 AM](https://user-images.githubusercontent.com/89927660/131787026-c7ba62b7-a7d1-4cee-a23b-319e7af7c2f5.png)

-----------------------------------------------------------------------------------------------------------------------------------------------
# Day 2

### _Highlights:_
|#|TOPICS COVERED|
|:---:|:---:|
|1.|[UNDERSTANDING THE LIBRARY](#understanding-the-library)|
|2.|[CONTENTS OF THE LIBRARY FILE](#contents-of-the-library-file)|
|3.|[HIERARCHICAL SYNTHESIS](#hierarchical-synthesis)|
|4.|[FLAT SYNTHESIS](#flat-synthesis)|
|5.|[SUB MODULE LEVEL SYNTHESIS](#sub-module-level-synthesis)|
|6.|[FLIP FLOP OVERVIEW](#flip-flop-overview)|
|7.|[FLIP FLOP SIMULATION](#flip-flop-simulation)|
|8.|[FLIP FLOP SYNTHESIS](#flip-flop-synthesis)|


### UNDERSTANDING THE LIBRARY

For a design to work, there are three important parameters that determines how the Silicon works: Process (Variations due to Fabrications), Voltage (Changes in the behavior of the circuit) and Temperature (Sensitivity of semiconductors). Libraries are characterized to model these variations. 

```
//Steps Followed:
//Command to open the libary file
$ gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//To shut off the background colors/ syntax off:
: syn off
//To enable the line numbers
: se nu
```

>sky130_fd_sc_hd__tt_025C_1v80.lib

|PARAMETERS|MEANING|
|:---:|:---:|
|SKY130|Technology - CMOS|
|fd|Foundary - Skywater|
|sc|Standard Cell - Digital|
|hd|Density - High|
|tt|Process - Typical|
|025C|Temperature - Measure|
|1v80|Voltage - Measure|

**_Screenshot: .lib Sample File_**

![Screen Shot 2021-09-02 at 1 17 28 AM](https://user-images.githubusercontent.com/89927660/131792266-d0d8a1e1-4831-4bd8-a642-9448fc478b25.png)

### CONTENTS OF THE LIBRARY FILE

.lib is a bucket of all standard cells that are available (including every flavor of the cells). For all combinations of cells, it also contains the features: Area, Power, Timing and Pin details, Capacitance to mention a few. 

```
//Steps Followed:
//To view the Equivalent Verilog model (in order to understand the functionality of the cell)
//to open without power ports
:sp ../my_lib/verilog_model/sky_130_fd_sc_hd__a2111o.behavioral.v
```

**_Screenshot: .lib file containing the cell details for sky_130_fd_sc_hd__a2111o_1_**

![Screen Shot 2021-09-02 at 1 26 58 AM](https://user-images.githubusercontent.com/89927660/131793727-de36696a-2cd4-4286-910d-0fbb2d1998cc.png)

**_Screenshot: Equivalent Verilog Model for sky_130_fd_sc_hd__a2111o_1_**

![Screen Shot 2021-09-02 at 1 29 16 AM](https://user-images.githubusercontent.com/89927660/131793892-821d9e15-228c-42a0-8d00-64b6ff819de0.png)

>_**Note:** For this above 5-input gate, there will be 2^5 = 32 combinations for each feature value (e.g leakage power consumption for all 32 combinations)._

**_Screenshot: Various Flavours of AND Cell_**

![Screen Shot 2021-09-02 at 1 47 51 AM](https://user-images.githubusercontent.com/89927660/131796090-558e3d56-ea70-4d70-9dd9-8873bd353b43.png)

```
//Steps Followed:
//To open and cell 
/cell .*and
//To open variations of and cell in parallel
:vsp
```

>_**Note:** AND2_4 implies that it is a wider cell compared to the other two. Hence, it has more area and power specifications. It also has the least delay. On the other hand, AND2_0 - cell with least area and power has bigger delay._

### HIERARCHICAL SYNTHESIS

```
//Steps Followed:
//Opening the file used for this experiment
$ gvim multiple_modules.v
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog multiple_modules.v
//Synthesize Design
$ synth -top multiple_modules
//Generate Netlist
$ abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for multiple modules
$ show multiple_modules
//Writing the netlist in a crisp manner 
$ write_verilog -noattr multiple_modules_hier.v
$ !gvim multiple_modules_hier.v
```

**_Screenshot: Multiple_modules File_**

>_**Note:** There are 2 submodules in this file. The module multiple_modules instantiaties two other sub-modules._

**_Screenshot: Statistics of Multiple_modules File_**

![Screen Shot 2021-09-02 at 5 04 14 PM](https://user-images.githubusercontent.com/89927660/131922463-01b29515-c90d-4431-aa86-f10b15fa8d82.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-02 at 5 12 46 PM](https://user-images.githubusercontent.com/89927660/131923051-5d882430-fa4a-4b0d-8b70-0857c58f9b34.png)

>_**Observation:** The realization after show command is different from the expected Realization of the file. The instants of the sub-modules u1 and u2 are seen instead of the the AND and OR gates. This is called as Hierarchical Design where the hierarchies are preserved._

**_Screenshot: Expected Realization of Multiple_modules File_**

![Screen Shot 2021-09-02 at 9 28 33 PM](https://user-images.githubusercontent.com/89927660/131941557-4dcd0693-a89c-4cda-a5ee-3cd572b6a665.png)

**_Screenshot: Netlist file showing the sub-modules, preserving the hierarchy**

![Screen Shot 2021-09-02 at 5 27 46 PM](https://user-images.githubusercontent.com/89927660/131924297-ff9aed84-85ba-40dd-884f-7c49705b0e48.png)

>_**Observation:** There is an interesting note on how the OR gate is realized through De-Morgan's Law. The Synthesis tool has chosen NAND implementation. This is because, for a NAND gate in CMOS implementaion - there is a stacked NMOS structure. For realising OR gate - there is a stacked PMOS structure. Stacking PMOS will have negative effects (since they need good logical efforts due to poor mobility of holes). Hence there is a need for wide cell to put in the logical efforts._

### FLAT SYNTHESIS

```
//Steps Followed:
//To flatten the netlist
$ flatten
//Writing the netlist in a crisp manner and to view it
$ write_verilog -noattr multiple_modules_flat.v
$ !gvim multiple_modules_flat.v
```

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-02 at 6 14 16 PM](https://user-images.githubusercontent.com/89927660/131927662-d25c4d37-c0c1-41a7-ab14-9399840eb3ee.png)

**_Screenshot: Netlist file showing the flattened netlist, without any hierarchy**

![Screen Shot 2021-09-02 at 6 10 30 PM](https://user-images.githubusercontent.com/89927660/131927406-1410346f-77bd-4fe0-a589-ffe2bf2f1a53.png)

>_**Observation:** There is no hierarchy preserved here and the gates are directly instantiated under module multiple_module without being called under the sub-module._

### SUB MODULE LEVEL SYNTHESIS

#### _Need for Sub-module level Synthesis:_
Sub-module level synthesis is preferred when there are multiple instances of same module. Sythesizing the same module over several times may not be advantageous with respect to time. Instead, synthsis can be performed for one module, its netlist can be replicated and then stitched together in the top module. This is also used particulary in massive designs using divide and conquer method. 

```
//Steps Followed:
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog multiple_modules.v
//Synthesize Design - this controls which module to synthesize
$ synth -top sub_module1
//Generate Netlist
$ abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//Writing the netlist in a crisp manner 
$ write_verilog -noattr multiple_modules_hier.v
$ !gvim multiple_modules_hier.v
```

**_Screenshot: Statistics of Sub-module_**

![Screen Shot 2021-09-02 at 9 11 28 PM](https://user-images.githubusercontent.com/89927660/131940332-d8272cc3-affb-471d-9dd0-1ad951d86c22.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-02 at 9 12 18 PM](https://user-images.githubusercontent.com/89927660/131940277-9fc3e2fe-b185-4d91-9b2d-86e54c1d1768.png)

**_Screenshot: NetList File of Sub-module_**

![Screen Shot 2021-09-02 at 9 13 33 PM](https://user-images.githubusercontent.com/89927660/131940384-c0bf6a0a-a73c-4c99-95a4-84a7a654e774.png)

>_**Note:** The gates that are instantiated under submodule 1 can only bee seen._

### FLIP FLOP OVERVIEW

In a digital design, when an input signal changes state, the output changes after a propogation delay. All logic gates add some delay to singals. These delays cause expected and unwanted transitions in the output, called as _Glitches_ where the output value is momentarily different from the expected value. An increased delay in one path can cause glitch when those signals are combined at the output gate. In short, more combinational circuits lead to more glitchy outputs that will not settle down with the output value. 

>_Hence, there is a need to store the values called as flop elements. D Flip-flops (aka Data or Delay Flip Flops) are the widely storage elements used to restrict the glitches._
 
 
![image](https://user-images.githubusercontent.com/89927660/131943529-8b90a84b-d745-48c1-9e3d-3876c4050a5e.png)


Flop elements stores a single bit of data - with either of the states 0 and 1. They are placed inbetween the combinational circuits and the output of flop will change at the edge of clock. Though it's input is glitchy, the output is stable. That way, the successive combinational circuit will receive a stable intput and its output will also be predictable and settled down.  

#### _How to Code the Flip Flop:_
Every flop element needs an initial state, else the combinational circuit will evaluate to a garbage value. In order to achieve this, there are control pins in the flop namely: Set and Reset which can either be Synchronous or Asynchronous. 

#### _Asynchronous Reset/Set:_

![Screen Shot 2021-09-02 at 10 11 42 PM](https://user-images.githubusercontent.com/89927660/131946717-7cdd0aeb-369a-4ade-b6d2-b9b7fc51b13e.png)
![Screen Shot 2021-09-02 at 10 16 12 PM](https://user-images.githubusercontent.com/89927660/131947092-dd26c7ca-25c2-48d0-9841-d9e0ad5d5a66.png)


>_**Note:** Here, always block gets evaluated when there is a change in the clock or change in the set/reset.The circuit is sensitive to positive edge of the clock. Upon the signal going low/high depending on reset or set control, singal q line goes changes respectively. Hence, it does not wait for the positive edge of the clock and happens irrespective of the clock_.

#### _Synchronous Reset:_

![Screen Shot 2021-09-02 at 10 19 42 PM](https://user-images.githubusercontent.com/89927660/131946706-2222f473-89bc-4a2a-85df-3ffb0c01e77e.png)

>_**Note:** Here, the singal waits for the clock always and is always set to D Pin of flop. The D pin will wait for the positive edge of the clock and on the subsequent occurence, the output changes respectively. The sensitivity list only contains posedge clk_. 

#### _Both Synchronous and Asynchronous Reset:_

![Screen Shot 2021-09-02 at 10 28 38 PM](https://user-images.githubusercontent.com/89927660/131946998-d79712a6-2c72-44ae-95c2-596372b3365c.png)

>_**Note:** Care needs to be taken when using the Set and Reset control pins and they may lead to race conditions. The always block is evaluated for positive edge of clock and asynchronous reset. The execution of else if implies that the always block has been evaluated because of the positive edge of the clock_. 

### FLIP FLOP SIMULATION

```
#Steps Followed for analysing Asynchronous behavior:
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog dff_asyncres.v tb_dff_asyncres.v 
//List so as to ensure that it has been added to the simulator
$ ls
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_dff_asyncres.vcd
```

**_Screenshot: Waveform Behavior of DFF with Asynchronous Reset_**

![Screen Shot 2021-09-02 at 11 00 02 PM](https://user-images.githubusercontent.com/89927660/131948704-e40344b8-ec25-4f30-9191-cd183067d287.png)

>_**Observation:** The output doesnt wait for the clock (independent of positive edge of the clock)._

### FLIP FLOP SYNTHESIS

```
//Steps Followed:
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog dff_asyncres.v
//Synthesize Design - this controls which module to synthesize
$ synth -top dff_asyncres
//There will be a separate flop library under a standard library
//so we need to tell the design where to specifically pick up the DFF
//But here we point back to the same library and tool looks only for DFF instead of all cells
$ dfflibmap -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Generate Netlist
$ abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//Writing the netlist in a crisp manner 
$ write_verilog -noattr dff_asyncres_ff.v
$ !gvim dff_asyncres_ff.v
```

**_Screenshot: Statistics of D FLipflop with Asynchronous Reset_**

![Screen Shot 2021-09-02 at 11 24 53 PM](https://user-images.githubusercontent.com/89927660/131950632-d611af77-1d3d-4def-b840-569faa2b9036.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-02 at 11 26 17 PM](https://user-images.githubusercontent.com/89927660/131950638-c44f59ae-3801-4533-966c-58cea4964490.png)

**_Screenshot: NetList File of D FLipflop with Asynchronous Reset_**

![Screen Shot 2021-09-02 at 11 28 27 PM](https://user-images.githubusercontent.com/89927660/131950646-83b1784a-8cbf-4be2-96ca-1d812fe00391.png)

### OPTIMIZATION TECHNIQUES PART 1

```
//Steps Followed:
//modules used for this experiment are opened using the command
$ gvim mult_*.v -o
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog mult_2.v
//Synthesize Design - this controls which module to synthesize
$ synth -top mult2
//Generate Netlist
$ abc -liberty ../my_lib/lib/SKY130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//Writing the netlist in a crisp manner 
$ write_verilog -noattr dff_asyncres_ff.v
$ !gvim dff_asyncres_ff.v
```

>_**Note:** Mul2: It has a 3 bit input and generating a 4 bit output. The relationship for the output is twice the input a. Apparently, the output can be written as the input a itself appended with zeros. Ideally, there is no requirement for Hardware without needing a multiplier._

**_Screenshot: Statistics of D FLipflop with Asynchronous Reset_**

Note: No hardware requirements - No # of memories, memory bites, processes and cells. Number of cells inferred is 0.

**_Screenshot: abc command return due to absence of standard cell library**

>_**Observation:** Due to absence of the cell, the tool returns Not to call abc command as there is nothing to map._

**_Screenshot: Graphical Realization of the Logic_**

### OPTIMISATION TECHNIQUES PART 2

>_**Note:** Incase if the input a has 3 bits and generated output has 5 bits. The relationship for the output is always a constant (say 9) times the input a. The number 9 can be split as 8 + 1, which replaces the output equation to be 8 times input a added with 000. _

-----------------------------------------------------------------------------------------------------------------------------------------------
# Day 2

Logic Optimisation
There are two types of digital logic circuits - combinational and sequential logic circuits. Combinational circuits are collection of basic logic gates, where the output depends only on the current inputs and do not require any clocks. They result in a simple circuit capable of implementing complex logic using logic gates only. Sequential circuits are collection of memory elements calls as flip-flops. The circuit's output depends on current input as well as the past intputs. Due to the presence of flip-flops, the output requires clock inputs. Hence, they result in a complex circuit capable of implmenting complex logic using memory.  

Why combi logic optimisation? 









