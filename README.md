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

![Screen Shot 2021-09-04 at 4 50 00 AM](https://user-images.githubusercontent.com/89927660/132090267-473a8ae1-ec52-4979-9e60-5cc0b05a0e6e.png)

#### _Mux - Working:_
The multiplexer (MUX) is a combinational logic circuit which is designed to switch one of the several input lines through to a single common output. The input A acts to control which input (either I0 or I1) gets passed to the output Q. A good mux - when the data select input A is at logic 0, input I1 passes its data to the output, while I0 is blocked. When the input is at logic 1, input I0 passes its data to the output, while I1 is blocked. Output Expression is given as Q = A'I1 + A I0.

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
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__t_025C_1v80.lib
//Read Design
$ read_verilog good_mux.v
//Synthesize Design
$ synth -top good_mux
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
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
|9.|[OPTIMIZATION TECHNIQUES](#optimization-techniques)|

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
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog multiple_modules.v
//Synthesize Design
$ synth -top multiple_modules
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__t_025C_1v80.lib
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

![Screen Shot 2021-09-04 at 4 32 57 AM](https://user-images.githubusercontent.com/89927660/132089951-73857fd9-aba8-40ac-b477-b316c8df5dc4.png)

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
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog multiple_modules.v
//Synthesize Design - this controls which module to synthesize
$ synth -top sub_module1
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
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

>_**Observation:** The output does not wait for the clock (independent of positive edge of the clock)._

### FLIP FLOP SYNTHESIS

```
//Steps Followed:
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog dff_asyncres.v
//Synthesize Design - this controls which module to synthesize
$ synth -top dff_asyncres
//There will be a separate flop library under a standard library
//so we need to tell the design where to specifically pick up the DFF
//But here we point back to the same library and tool looks only for DFF instead of all cells
$ dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
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

### OPTIMIZATION TECHNIQUES

```
//Steps Followed:
//modules used for this experiment are opened using the command
$ gvim mult_*.v -o
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog mult_2.v
//Synthesize Design - this controls which module to synthesize
$ synth -top mul2
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//Writing the netlist in a crisp manner 
$ write_verilog -noattr mult_2.v
$ !gvim mult_2.v
```

#### _CASE 1: mult_2.v_

**_Screenshot: Expected logic from RTL file_**

![Screen Shot 2021-09-04 at 3 55 55 AM](https://user-images.githubusercontent.com/89927660/132088944-b199fd81-2a54-4184-8273-3ed808c052cd.png)

>_**Note:** Mul2: It has a 3 bit input and generating a 4 bit output. The relationship for the output is twice the input a. Apparently, the output can be written as the input a itself appended with zeros. Ideally, there is no requirement for Hardware without needing a multiplier._

**_Screenshot: Statistics of D FLipflop with Asynchronous Reset_**

![Screen Shot 2021-09-04 at 4 04 02 AM](https://user-images.githubusercontent.com/89927660/132089094-37a9160e-2860-43f0-a36e-44a5cd2128d6.png)

>_**Note:** No hardware requirements - No # of memories, memory bites, processes and cells. Number of cells inferred is 0._

**_Screenshot: abc command return due to absence of standard cell library**

![Screen Shot 2021-09-04 at 4 05 32 AM](https://user-images.githubusercontent.com/89927660/132089134-4dac96e9-7e26-4f54-827a-871e5a71cd28.png)

>_**Observation:** Due to absence of the cell, the tool returns Not to call abc command as there is nothing to map._

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 4 06 16 AM](https://user-images.githubusercontent.com/89927660/132089155-291995bd-2b6d-4bd5-9b17-0a377d4c011a.png)

>_**Note:** The ouput here is just a appended with a zero._

**_Screenshot: NetList File of Sub-module_**

![Screen Shot 2021-09-04 at 4 27 15 AM](https://user-images.githubusercontent.com/89927660/132089753-8e6cd95d-0d92-4b70-86f0-bea362fc5c73.png)

#### _CASE 2: mult_8.v_

**_Screenshot: Expected logic from RTL file_**

![Screen Shot 2021-09-04 at 4 29 20 AM](https://user-images.githubusercontent.com/89927660/132089797-01b21135-e1b0-44e0-96fc-13151650882a.png)

>_**Note:** Incase if the input a has 3 bits and generated output has 5 bits. The relationship for the output y is always a constant (say 9) times the input a. The number 9a can be split as (8 + 1)a. a(1) - a mapped to a 3-bit number. a(8) - a followed by 000 which gives a 6-bit number._

![Screen Shot 2021-09-04 at 4 19 20 AM](https://user-images.githubusercontent.com/89927660/132089537-ea9225f5-00ed-462f-b61e-208a3ae8d25e.png)

**_Screenshot: Statistics_**

![Screen Shot 2021-09-04 at 4 09 34 AM](https://user-images.githubusercontent.com/89927660/132089581-678a6ffc-7a0c-4622-b463-977f9696c3da.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 4 21 45 AM](https://user-images.githubusercontent.com/89927660/132089629-c1380335-d442-4db1-86fa-3288b69615fd.png)

>_**Note:** There is no hardware requirement._

**_Screenshot: NetList File of Sub-module_**

![Screen Shot 2021-09-04 at 4 25 24 AM](https://user-images.githubusercontent.com/89927660/132089762-47842545-dc80-49f9-bbea-23719af81335.png)

-----------------------------------------------------------------------------------------------------------------------------------------------
# Day 3

### _Highlights:_
|#|TOPICS COVERED|
|:---:|:---:|
|1.|[LOGIC CIRCUITS OVERVIEW](#logic-circuits-overview)|
|2.|[COMBINATIONAL LOGIC OPTIMIZATION](#combinational-logic-optimization)|
|3.|[SEQUENTIAL LOGIC OPTIMIZATION](#sequential-logic-optimization)|
|4.|[SEQUENTIAL UNUSED OUTPUT OPTIMIZATION](#sequential-unused-output-optimization)|

### LOGIC CIRCUITS OVERVIEW

There are two types of digital logic circuits - _combinational and sequential logic circuits_. **_Combinational circuits_** are collection of basic logic gates, where the output depends only on the current inputs and do not require any clocks. They result in a simple circuit capable of implementing complex logic using logic gates only. **_Sequential circuits_** are collection of memory elements calls as flip-flops. The circuit's output depends on current input as well as the past intputs. Due to the presence of flip-flops, the output requires clock inputs. Hence, they result in a complex circuit capable of implmenting complex logic using memory.  

#### _Combinational Logic Optimization Techniques_

Logic optimization is a part of logic synthesis to find an equivalent representation of the specified logic circuit under one or more specified constraints. We perform in order to squeeze the logic and get the most optimized design which can lead to area and power savings. This can be achieved through Constant Propoation Method (Ex: Direct Optimisation) or through Boolean Logic Optimization (ex: K-Map or Quine-McCluskey Methods). In Constant Propogation, most optimized logic is obtained by propogating the value of one input is to the next stage and all the way to the output. In Boolean Logic Optimization, synthesis tools reduce complex logic equations to simplied version using boolean algebra/K-map reductions. 

#### _Sequential Logic Optimization Techniques_

One of the most basic Optimization Techniques for sequential circuits is the constant propogation method. At times of logic design when D input is tied Low, in order to optimize the sequential logic, the Q pin of flop should always have a constant value. There are also advanced techniques to obtain a most condensed state machine: 1) State Optimization where the unused states are being optimized. 2) Cloning way of logic is done during physical aware synthesis (where if two flops are sitting far off - there might be a large routing delay. To prevent this, a flip flop with huge positive slack can be cloned and the timing can be met). 3) Re-timing - the combinational logic is partitioned effectively to reduce the delay, thereby increasing the frequency of operation. Hence, the performance of the circuit is improved using this technique. 

### COMBINATIONAL LOGIC OPTIMIZATION

```
//Steps Followed for each of the optimization problems:
//to view all optimization files
$ ls *opt_check*
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog opt_check.v
//Synthesize Design - this controls which module to synthesize
$ synth -top opt_check
//To perform constant propogation optimization
$ opt_clean -purge
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```

![Screen Shot 2021-09-04 at 12 22 02 AM](https://user-images.githubusercontent.com/89927660/132083551-5c53c20d-38a9-4615-87ec-c9c01a249d44.png)

#### _CASE 1: opt_check.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 12 27 04 AM](https://user-images.githubusercontent.com/89927660/132083682-50c7cb6a-9ed9-4fda-b429-70d31a77b7d8.png)

>_**Note:** The value of y depends on a, y = ab._

**_Screenshot: Command for performing combinational optimization using constant propogation method_**

![Screen Shot 2021-09-04 at 12 06 13 AM](https://user-images.githubusercontent.com/89927660/132083253-7852498e-5ecc-48fb-890f-942b46670588.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 12 06 55 AM](https://user-images.githubusercontent.com/89927660/132083272-7384978a-0c82-4303-81c8-efbfbde3dcee.png)

>_**Note:** The optimized graphical realization thus shows a 2-input AND gate being implemented. _

#### _CASE 2: opt_check2.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 12 27 35 AM](https://user-images.githubusercontent.com/89927660/132083672-6d4aa97e-f413-4d74-adae-bdce75cda645.png)

>_**Note:** The value of y depends on a, y = a+b._

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 12 17 39 AM](https://user-images.githubusercontent.com/89927660/132083462-9b0e643c-b1d7-4666-91f2-bc2afe9d1e32.png)

>_**Note:** The optimized graphical realization thus shows 2-input OR gate being implemented. Although OR gate can be realized using NOR, it can lead to having stacked PMOS configuration which is not a design recommendation. So the OR gate is realized using NAND and NOT gates (which has stacked NMOS configuration)._

#### _CASE 3: opt_check3.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 12 28 00 AM](https://user-images.githubusercontent.com/89927660/132083691-bb2eccbb-85b6-4c48-b27d-0c1a3f11dbfa.png)

>_**Note:** The value of y depends on a, y = abc._

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 12 34 27 AM](https://user-images.githubusercontent.com/89927660/132083820-25411f5d-744c-4843-9081-1ae379a98d74.png)

>_**Note:** The optimized graphical realization thus shows 3-input AND gate being implemented._

#### _CASE 4: opt_check4.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 2 03 29 AM](https://user-images.githubusercontent.com/89927660/132085957-fac3ef34-7c99-4108-9908-fd97935cd439.png)

>_**Note:** The value of y depends on a, y = a'c + ac._

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 12 54 49 AM](https://user-images.githubusercontent.com/89927660/132084352-ee9c60ae-747f-4b9d-967d-a002219c20c7.png)

>_**Note:** The optimized graphical realization thus shows A XNOR C gate being implemented._

#### _CASE 5: multiple_module_opt.v_

>_**Note:** Due to the presence of multiple modules, the netlist was flattened before optimizing the logic circuit._

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 2 05 23 AM](https://user-images.githubusercontent.com/89927660/132086005-8d5fecef-68a4-4e3b-b5e6-4ee4bc50af4b.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 1 05 44 AM](https://user-images.githubusercontent.com/89927660/132084707-c7338477-72e5-4d1b-82cb-1ba80040689a.png)

>_**Note:** The optimized graphical realization thus shows a 2-input AND into first input of 2-input OR gate being implemented._

#### _CASE 6: multiple_module_opt2.v_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 1 17 02 AM](https://user-images.githubusercontent.com/89927660/132084984-8cbe4ff2-a4b1-466e-93dc-f2d787a90a58.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 1 18 21 AM](https://user-images.githubusercontent.com/89927660/132084990-4d97dcdd-d3da-45ea-92c5-df26149f7f30.png)

### SEQUENTIAL LOGIC OPTIMIZATION

```
//Steps Followed for each of the optimization problems:
//To view all optimization files
$ ls *df*const*
//To open multiple files 
$ dff_const1.v -o dff_const2.v
//Performing Simulation
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog dff_const1.v tb_dff_const1.v 
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_dff_const1.vcd
//Performing Synthesis
//Invoke Yosys 
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Read Design
$ read_verilog dff_const1.v
//Synthesize Design - this controls which module to synthesize
$ synth -top dff_const1
//There will be a separate flop library under a standard library
//so we need to tell the design where to specifically pick up the DFF
//But here we point back to the same library and tool looks only for DFF instead of all cells
$ dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```

#### _CASE 1: dff_const1.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 1 28 13 AM](https://user-images.githubusercontent.com/89927660/132085199-17e1722e-fbe1-435c-8aed-3261dfe6f7c3.png)

>_**Note:** Although Reset goes low, Q will wait for the clock to go high in order to become high. The flop will be inferred in this design._

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 1 32 08 AM](https://user-images.githubusercontent.com/89927660/132085290-0f5c8c03-5465-4be2-ace6-0d103637a2e9.png)

**_Screenshot: Statistics showing a flop inferred_**

![Screen Shot 2021-09-04 at 4 31 22 AM](https://user-images.githubusercontent.com/89927660/132089896-f5ff572c-e4b2-4266-988e-79997dc01e99.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 1 58 56 AM](https://user-images.githubusercontent.com/89927660/132085850-78172e3b-bb1f-4e31-8c1c-d48f6361e1c1.png)

>_**Note:** The optimized graphical realization thus shows the flop inferred. Also, the design code has active high reset and the standard cell library has active low reset - so, there is a presence of inverter for the reset._

#### _CASE 2: dff_const2.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 1 28 38 AM](https://user-images.githubusercontent.com/89927660/132085202-cc08a4f3-0787-48f0-abe6-217700b74c31.png)

>_**Note:** Q is constant with value of 1_

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 1 33 58 AM](https://user-images.githubusercontent.com/89927660/132085291-d3c57b95-c2a7-4123-b166-bfa9f76927f7.png)

**_Screenshot: Statistics showing no flop inferred_**

![Screen Shot 2021-09-04 at 2 09 04 AM](https://user-images.githubusercontent.com/89927660/132086147-d64d6779-620d-4b63-ac49-14c8bc40755b.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 2 01 09 AM](https://user-images.githubusercontent.com/89927660/132085890-a2113331-ed15-4ab8-bbbc-9f3cd603897d.png)

>_**Note:** The optimized graphical realization thus does not have any flop inferred and is a constant value of 1, irrespective of reset or clock signals._

#### _CASE 3: dff_const3.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 2 13 14 AM](https://user-images.githubusercontent.com/89927660/132086209-8291c021-eb09-4a30-9d18-654d61264854.png)

>_**Note:** There are 2 flops, reset if condition defines Q else condition defines D signal. Q1 waits for the next positive edge of the clock with reset is applied. The successive flop will sample the value of 0 due to TCK delay effect in the preceeding flop. The output Q will always be high except for a one clock cycle. Both flops are expected to be present and will not be optimized._

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 2 27 30 AM](https://user-images.githubusercontent.com/89927660/132086603-fa6e7356-d968-434a-873a-3c29627174c5.png)

**_Screenshot: Statistics showing both flops being inferred_**

![Screen Shot 2021-09-04 at 2 31 19 AM](https://user-images.githubusercontent.com/89927660/132086794-38e331a5-0f72-4333-8d3b-957e14b2891b.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 2 35 12 AM](https://user-images.githubusercontent.com/89927660/132086882-0eb8b850-4eb6-418b-8465-f5736574e96f.png)

>_**Note:** The optimized graphical realization thus shows both flops being inferred. It can also be seen that the first flop is reset and second is set._

#### _CASE 4: dff_const4.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 4 31 00 AM](https://user-images.githubusercontent.com/89927660/132089903-94c9504f-7e6d-4b10-8989-05310fa72791.png)

>_**Note:** Since both the flops have constant 1 in the output lines and thus they are expected to be optimized. The resulting netlist will not have any flops inferred. ._

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 2 41 30 AM](https://user-images.githubusercontent.com/89927660/132087140-bce6b4d5-b9ba-429f-a5e2-d0c2d4379fe3.png)

**_Screenshot: Statistics showing no flops being inferred_**

![Screen Shot 2021-09-04 at 2 46 50 AM](https://user-images.githubusercontent.com/89927660/132087252-261a2389-5fce-4e02-9611-e1bf33afc620.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 2 48 40 AM](https://user-images.githubusercontent.com/89927660/132087304-464689aa-e783-4f71-88ee-8a9c338bf17d.png)

>_**Note:** The optimized graphical realization thus does not have flops inferred and is a constant value of 1, irrespective of reset or clock signals._

#### _CASE 5: dff_const5.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 4 31 51 AM](https://user-images.githubusercontent.com/89927660/132089891-9ccbcce4-9e88-4e8c-83b2-62e96dedeea1.png)

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 2 44 22 AM](https://user-images.githubusercontent.com/89927660/132087147-6e2d3c90-3ffc-4e4e-9663-51d84b990a43.png)

**_Screenshot: Statistics showing both same flop being inferred twice_**

![Screen Shot 2021-09-04 at 2 54 43 AM](https://user-images.githubusercontent.com/89927660/132087411-95bbba8b-593c-4b85-a81d-680cfcded3e3.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 2 55 39 AM](https://user-images.githubusercontent.com/89927660/132087414-fe214115-81c5-42c5-a030-7031f94bc678.png)

>_**Note:** The optimized graphical realization have the same flop inferred twice._

### SEQUENTIAL UNUSED OUTPUT OPTIMIZATION 

```
//Steps Followed for each of the unused output optimization problems:
//opening the file
$ gvim counter_opt.v
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog opt_check.v
//Synthesize Design - this controls which module to synthesize
$ synth -top opt_check
//To perform constant propogation optimization
$ opt_clean -purge
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd_-tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
```

#### _CASE 1: counter_opt.v_

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 3 06 31 AM](https://user-images.githubusercontent.com/89927660/132087686-2c95772e-1fb7-4fa2-9677-d66149f594da.png)

![Screen Shot 2021-09-04 at 3 14 13 AM](https://user-images.githubusercontent.com/89927660/132087854-23ec6319-5b57-4a15-8453-ece548643537.png)

>_**Note:** If there is a reset, the counter is intialised to 0, else it is incremented - performing like an upcounter. Since it is a 3 bit signal, the counter rolls back after 7. However, the final output q is sensing only the count [0], so the bit is toggling in every clock cycle (000, 001, 010 ...111). The other two outputs are unused and does not create any output dependency. Hence, these unused outpus need not be present in the design._

**_Screenshot: Statistics showing only one flop inferred instead of 3 flops sinces it is a 3 bit counter_**

![Screen Shot 2021-09-04 at 4 32 14 AM](https://user-images.githubusercontent.com/89927660/132089882-32ddc9a5-fe19-4bae-a8de-1dd342f53450.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 3 19 22 AM](https://user-images.githubusercontent.com/89927660/132088002-39377ee7-f1d8-4abf-be43-3a5189e67fbd.png)

>_**Note:** The optimized graphical realization output Q (count0) being fed to NOT gate so as to perform the toggle function. The other outputs which has no dependency on the primary out is optimized off._

#### _CASE 2: counter_opt2.v_

```
//Steps Followed:
//Copying the code to a new file
$ cp counter_opt.v counter_opt2.v
$ gvim counter_opt2.v
//Changes made in the verilog code, i for insert mode: 
- assign q = [count2:0] == 3'b100;
```

**_Screenshot: Expected logic from verilog file_**

![Screen Shot 2021-09-04 at 3 36 58 AM](https://user-images.githubusercontent.com/89927660/132088508-f79a00ad-cd2c-451b-9521-ded1a1568e41.png)

>_**Note:** In this case, all three bits of the counter is used and hence 3 flops are expected in the optimized netlist._

**_Screenshot: Statistics showing all three flops inferred_**

![Screen Shot 2021-09-04 at 3 37 53 AM](https://user-images.githubusercontent.com/89927660/132088576-83ccc9e4-01c8-4ba4-8cfe-a3b19b5d7ed9.png)

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 3 38 54 AM](https://user-images.githubusercontent.com/89927660/132088587-4cf00ae0-5a41-45d7-b238-7f42d2e50ffd.png)

>_**Note:** All three flops can be seen. There is a need for incremental logic, so the logic other than flops represent the adder circuit. The expression at the output is q = counter2.counter1'.counter0'. Therefore, the outputs having no direct role on the primary output will only be optimized away._

-----------------------------------------------------------------------------------------------------------------------------------------------
# Day 4

### _Highlights:_
|#|TOPICS COVERED|
|:---:|:---:|
|1.|[GATE LEVEL SIMULATION](#gate-level-simulation)|
|2.|[SYNTHESIS SIMULATION MISMATCH](#synthesis-simulation-mismatch)|
|3.|[EXPERIMENTS WITH GLS](#experiments-with-GLS)|
|4.|[MISSING SENSITIVITY LIST](#missing-sensitivity-list)|
|5.|[CAVEATS IN BLOCKING ASSIGNMENTS](#caveats-in-blocking-assignments)|

### GATE LEVEL SIMULATION

#### _What is GLS?_

Previously, the functionality of the design was given stimulus inputs and the output was verified to meet the specifications through a test bench module. The RTL design was considered as the DUT (Design Under Test). In Gate Level Simulation, the Netlist is considered as the Design Under Test.  Netlist is logically same as the RTL code that was converted to Standard Cell Gates. Hence, same test bench will align with the design. 

#### _Advantages of GLS:_

* To logically verify the correctness of the design after Synthesis.
* During the RTL Simulation, timing was not accounted. But for practical applications, there is a need to ensure the timing of the design to be met. 

![Screen Shot 2021-09-04 at 12 54 53 PM](https://user-images.githubusercontent.com/89927660/132105880-e47f3d07-d920-4d0a-ab03-1b3c7b3da68d.png)

```
//consider a netlist
and uand (.a(a),.b(b))
or uor (.a(a),.b(b))
//There is a need to define the meaning of and and or
//Thus we need netlist, testbench and verilog models of the standard cells
```

>_**Note:** Netlist consists of all standard cells instantiated and it's meaning is conveyed to the iVerilog using Gate Level Verilog Models. Gate Level Verilog Models can be functional or timing aware. If the gate level models are delay annotated, then GLS can be performed for timing validation also in addition to functional validation._

### SYNTHESIS SIMULATION MISMATCH

If netlist is a true reciprocation of RTL, what is the need to validate the functionality of netlist? There may be synthesis and simulation mismatch due to the following reasons: 
1. Absence of Sensitivity List
2. Blocking Vs Non Blocking Assignments
3. Non Standard Verilog Coding

#### _Absence of Sensitivity List:_

```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (sel)
begin
   if (sel)
            y = i1;
   else 
            y = i0;          
end
endmodule
```

The output of Simulator changes only when the input changes. The output is not evaluated when there is no activity. In the above 2x1 mux code, when select is changing (when select is 1), the output is 1 when input is 1 else the output is 0. The always block evaluates only when there is a transition change in select pin, and is not sensitive (output does not reflect) to changes in the inputs 0 and 1. 

|Time|Logic|
|:---:|:---:|
|During Simulation|Logic acts as a Latch/Double edged Flop|
|During Synthesis|Logic acts as a Mux|

**_Hence there is a Synthesis Simulation mismatch due to missing sensitivity list. This is because the synthesizer will not take sensitivity list into account and always looks for the functionality of the code._**

#### _Corrected code for missing sensitivity list:_

```
module mux(
input i0,input i1
input sel,
output reg y
);
always @ (*)
begin
   if (sel)
            y = i1;
   else 
            y = i0;        
end
endmodule
```

>_**Note:** Thus the mismatch is corrected by having always @ (*) where the always block is evaluated when any signal changes. So, any changes in inputs will also be seen in the output._

#### _Blocking Vs Non Blocking Statments in Verilog:_

#### _CAVEAT 1:_

The error always occurs when inside an always block. **_Blocking Statements_** executes the statements in the order it is written. The first statement is always evaluated before second statement (like a C program). **_Non-Blocking Statements_** executes the statements in parallel. All the right hand side assignments will be evaluated before assigning to the left hand side. 

|Assignment|Statement|
|:---:|:---:|
|=|Blocking Statment|
|<=|Non-Blocking Statment|

```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q = q0;
        q0 = d;    
end
endmodule
```

![Screen Shot 2021-09-04 at 2 46 14 PM](https://user-images.githubusercontent.com/89927660/132106366-2faa1e5a-738b-4782-a550-4fe8dfc4aef2.png)

The code is aimed to create a shift register when two flops as shown above. The assignments inside the code represent the blocking statements. q0 and q are assigned to 1 bit 0s - so asynchronous reset connection happens. However, in the later parts, q0 is assigned to q and then d gets assigned to q0. If suppose, there is a change in the code

```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 = 1'b0;
        q = 1'b0;
end
else
        q0 = d;
        q = q0;    
end
endmodule
```

In this case, d is assigned to q0 and then q0 is assigned to q. So, by the time the second statment gets executed, q0 has the value of d. This will lead to implementation of only one flop. Previously, q has the value of q0 and q0 has the value of d - which lead to implementation of 2 storage elements. 

#### _Usage of non-blocking statements:_

```
module code (input clk,input reset,
input d,
output reg q);
always @ (posedge clk,posedge reset)
begin
if(reset)
begin
        q0 <= 1'b0;
        q <= 1'b0;
end
else
        q0 <= d;
        q <= q0;   
end
endmodule
```

**_Therefore the order does not matter here as RHS gets evaluated first and then assignment takes place. Presence of two flops irrespective of the order. Always use non blocking statements for writing sequential circuits._**

#### _CAVEAT 2: Causing Synthesis Simulation Mismatch_

```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        y = q0 & c;
        q0 = a|b ;    
end 
endmodule
```

The code is aimed to create a function of y = (A+B).C. In the above code, when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So y gets evaluated first (q0.C), where the q0 results corresponds to the previous iteration's result. The q0 value gets updated only in the second statement. 

|Time|Logic|
|:---:|:---:|
|During Simulation|Logic mimcs a delay or flop|
|During Synthesis|Logic will not have a flop|

When the order of the statements is changed: In this case, a OR b is evaluated first and the latest value is used for calculating y. 

```
module code (input a,b,c
output reg y);
reg q0;
always @ (*)
begin
        q0 = a|b ;
        y = q0 & c;
end 
endmodule
```

**_Therefore there is a paramount importance to run the GLS on the netlist and match the specifications, to ensure there is no simulation synthesis mismatch._**

### EXPERIMENTS WITH GLS

```
//Steps Followed: 
//opening the file
$ gvim ternary_operator_mux.v
//PERFORMING SIMULATION
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_ternary_operator_mux.vcd
//PERFORMING SYNTHESIS
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog ternary_operator_mux.v
//Synthesize Design - this controls which module to synthesize
$ synth -top ternary_operator_mux
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//Write Verilog netlist
$ write_verilog ternary_operator_mux_net.v
//PERFORMING GLS
//Opening Verilog Models, Netlist and Test Bench
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/
sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_ternary_operator_mux.vcd
```

#### _CASE 1: ternary_operator.v_

>_**Note:** Mux function is written using a ternary operator. Ternary operator takes 3 operands with the format. 

```
<Condition>?<True>:<False>
```

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 2 22 39 PM](https://user-images.githubusercontent.com/89927660/132105872-aacbb89e-5a9d-42b9-8868-fe46a5a0ddc5.png)

**_Screenshot: Verifying the Observation using Simulation_**

![Screen Shot 2021-09-04 at 2 23 25 PM](https://user-images.githubusercontent.com/89927660/132105866-0c6427fb-b651-4c4f-8c42-19966ddaf0b2.png)

>_**Observation:** Function of a 2x1 mux._

**_Screenshot: Statistics showing a flop inferred_**

![Screen Shot 2021-09-04 at 2 24 38 PM](https://user-images.githubusercontent.com/89927660/132105861-045b6e33-2997-4a9c-9540-0ac327628366.png)

>_**Observation:** A mux has been inferred._

**_Screenshot: Graphical Realization of the Logic_**

![Screen Shot 2021-09-04 at 2 25 19 PM](https://user-images.githubusercontent.com/89927660/132105855-909fbf9b-7abb-4756-8f7a-b9569797f75f.png)

>_**Note:** NAND gate with i1 and sel, inverted io and Or to And invert gate, to which the inputs are sel and inverted i0. The output y is given by the expression = sel'.i0 + sel.i1_

**_Screenshot: Commands to perform Gate Level Simulation_**

![Screen Shot 2021-09-04 at 2 51 13 PM](https://user-images.githubusercontent.com/89927660/132106425-f87711d7-acfd-4292-b5b5-cb488aa86dd7.png)

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-04 at 2 44 12 PM](https://user-images.githubusercontent.com/89927660/132106301-9ef0cd42-25b6-42f2-a4fa-656725632265.png)

>_**Observation:** Confirms the functionality of 2x1 mux._

### MISSING SENSITIVITY LIST

#### _CASE 2: bad_mux.v showing mismatch due to missing sensitivity list_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 2 52 14 PM](https://user-images.githubusercontent.com/89927660/132106441-722fcb2e-fb52-4bfa-b761-bf5b9de3817c.png)

>_**Expected Behavior:** The sensitivity list contains only select input. So during Simulation, the logic acts as a latch and during synthesis, it acts as a mux._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 2 58 38 PM](https://user-images.githubusercontent.com/89927660/132106569-047919a9-4eb7-44db-9615-f591e909ad9f.png)

>_**Observation:** When select is low, it follows i0, and there is no activity happening in select line - so the output remains low. When the select is high, it follows i1, and again there is no activiting in the select line. Thus it acts as a flop, retaining its value._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 3 04 07 PM](https://user-images.githubusercontent.com/89927660/132106716-e0b15b92-337f-4e09-a534-c920a676ec16.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 3 06 07 PM](https://user-images.githubusercontent.com/89927660/132106767-401bf904-f8b4-46d1-a0fd-28124d5523a5.png)

>_**Note:** There is a mux inferred during synthesis of the logic._

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-04 at 3 12 13 PM](https://user-images.githubusercontent.com/89927660/132106949-edceae4c-0222-481f-9f09-04b04a36ed11.png)

>_**Observation:** Confirms the functionality of 2x1 mux after synthesis where when the select is low, activity of input 0 is reflected on y. Similarly, when the select is hight, activity of input 1 is reflected on y. Hence there is a synthesis simulation mismatch due to missing sensitivity list._

### CAVEATS IN BLOCKING ASSIGNMENTS

#### _CASE 3: blocking_caveat.v showing mismatch due to blocking assignments_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 3 18 30 PM](https://user-images.githubusercontent.com/89927660/132107088-418508ce-be19-4e72-9111-19c59d10c87c.png)

>_**Expected Behavior:** In the above code, when the code enters always block, due to the presence of blocking statements, they get evaulated in order. So d gets evaluated first (x.c), where the x results corresponds to the previous iteration's result (a|b). The d value gets updated only in the second statement. The output expression is given as d = (a+b).c_

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 3 24 43 PM](https://user-images.githubusercontent.com/89927660/132107190-01276363-6441-4828-964f-8a90c1fe92d4.png)

>_**Observation:** d = (a+b).c, if the inputs a,b = 0; then a+b = 0. The output d = 0. But, we observe the output d = 1 because it looks at the past value where a+b was 1._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 3 27 07 PM](https://user-images.githubusercontent.com/89927660/132107231-35d383aa-8af6-4926-b050-483e3c96a552.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 3 27 52 PM](https://user-images.githubusercontent.com/89927660/132107244-2db5d6bd-ebd7-4873-a289-74b16fd0e19e.png)

>_**Note:** The synthesized design has or 2 and gate to realize the output._

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-04 at 3 31 35 PM](https://user-images.githubusercontent.com/89927660/132107321-51c03706-4818-4d15-a9a8-b0eb623d14b5.png)

>_**Observation:** The value of output d is 0 after simulation and 1 after synthesis for the same set of input values. Hence there is a synthesis simulation mismatch due to blocking assignments._

-----------------------------------------------------------------------------------------------------------------------------------------------

# Day 5

### _Highlights:_
|#|TOPICS COVERED|
|:---:|:---:|
|1.|[IF STATEMENTS](#if-statements)|
|2.|[CASE STATEMENTS](#case-statements)|
|3.|[INCOMPLETE IF STATEMENTS](#incomplete-if-statements)|
|4.|[INCOMPLETE CASE STATEMENTS](#incomplete-case-statements)|
|5.|[STATEMENTS USING FOR](#statements-using-for)|
|6.|[STATEMENTS USING GENERATE](#statements-using-generate)|

### IF STATEMENTS

#### _Why IF Statements?_

The if statement is a conditional statement which uses boolean conditions to determine which blocks of verilog code to execute. If always translates into Multiplexer. It is used for priority Logic and are always used inside always block.The variable should be assigned as a register.

#### _Syntax for IF Statement_

```
if<cond>
begin
.....
.....
end
else
begin
.....
.....
end
```
#### _Syntax for IF-ELSE-IF Statement_

```
if<cond1>
begin
.....
 executes cb1
.....
end
else if<cond2>
begin
.....
  executes cb2
.....
end
else if<cond3>
begin
.....
  executes cb3
.....
end
else
begin
.....
  executes cb4
.....
end
```

#### _Hardware Implementation_
 
![Screen Shot 2021-09-04 at 4 30 46 PM](https://user-images.githubusercontent.com/89927660/132108531-9f88b486-75b8-42be-a141-fc3abac109e4.png)
 
>_**Note:** Condition 1 gets the highest Prority, If the condition1 is met - other conditions are not evaluated. LegN gets evaluated only if all the conditions precedding fail to meet._

#### _Cautions with using IF Statements_

**_Inferred latches_** can serve as a 'warning sign' that the logic design might not be implemented as intended. They represent a bad coding style, which happens because of incomplete if statements/crucial statements missing in the design. For ex: if a else statement is missing in the logic code, the hardware has not been informed on the decision, and hence it will _latch_ and will tried retain the value. This type of design should be completely avoided unless intended to meet the design functionality (ex: Counter Design). 

![Screen Shot 2021-09-04 at 4 45 49 PM](https://user-images.githubusercontent.com/89927660/132108796-8cfdeb80-4f84-4cfe-8b2a-f061fbe139d5.png)

>_**Note:** Combinational circuits cannot have an inferred latch._

### CASE STATEMENTS

#### _CASE Statements_
The hardware implementation is a Multiplexer. Similar to IF Statements, Case statements are also used inside always block and the variable should be a register variable. 

#### _Syntax_

```
reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
		      .
	endcase	
end
```

#### _Caveats in CASE Statements_

1. Case statements are dangerous when there is an incomplete Case Statement Structure may lead to inferred latches. To avoid inferred latches, code Case with default conditions. When the conditions are not met, the code executes default condition.

```
reg y
always @ (*)
begin
	case(sel)
		2'b00:begin
		      ....
		      end
		2'b01:begin
		      ....
		      end
		      .
		      .
	default:begin
         ....
		       end
	endcase	
end
```

2. Partial Assignments in Case statements - not specifying the values. This will also create inferred latches. To avoid inferred latches, assign all the inputs in all the segments of the case statement. 

#### _Comparison between If - Else If - Else If - Else Vs Case_

|If - Else If - Else If - Else Structure|Case Structure|
|:---:|:---:|
|Undergoes concept of priority.|No priority of segments in case structure|
|Only one segment of the code will execute as it follows top-bottom approach sequentially.|May lead to Unpredictable outputs in bad case structures as there may be more than one segment executing the code. Thus, we should not have overlapping case statements. 

```
//Steps Followed for all the experiments: 
//opening the file
$ ls *incomp*
$ gvim *incomp* -o
//PERFORMING SIMULATION
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog incomp_if.v tb_incomp_if.v
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_incomp_if.vcd
//PERFORMING SYNTHESIS
//Invoke Yosys
$ yosys
//Read library 
$ read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Read Design
$ read_verilog incomp_if.v
//Synthesize Design - this controls which module to synthesize
$ synth -top incomp_if
//Generate Netlist
$ abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
//Realizing Graphical Version of Logic for single modules
$ show 
//To write the netlist
$ write_verilog -noattr incomp_if_net.v
//PERFORMING GLS
//Opening Verilog Models, Netlist and Test Bench
$ iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/
sky130_fd_sc_hd.v incomp_if_net.v tb_incomp_if.v
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_incomp_if.vcd
```
### INCOMPLETE IF STATEMENTS

#### _CASE 1: incomplete if statements_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 5 17 41 PM](https://user-images.githubusercontent.com/89927660/132109267-6c694036-5f29-4106-9c02-afecf6586919.png)

>_**Expected Behavior:** Else case is missing so there will be a D latch._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 5 24 06 PM](https://user-images.githubusercontent.com/89927660/132109365-90c03f00-31d3-4bfb-b67b-10d9de1c30d1.png)

>_**Observation:** When i0 (select line) is low, the output latches to a constant value. Presence of inferred latches due to incomplete if structure._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 5 31 33 PM](https://user-images.githubusercontent.com/89927660/132109477-1624c61a-a508-4786-b2c0-29c4c90aa375.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 5 32 58 PM](https://user-images.githubusercontent.com/89927660/132109498-1c438b69-15a0-4c35-b43d-1e41d46a44a3.png)

>_**Note:** The synthesized design has a D Latch inferred due to incomplete if structure (missing else statement)._

#### _CASE 2: incomplete if statements_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 5 35 38 PM](https://user-images.githubusercontent.com/89927660/132109551-97fec38b-d1ea-4196-bf3b-b8f0acd7cd84.png)

>_**Expected Behavior:** Else case is missing so there will be a latch._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 5 42 15 PM](https://user-images.githubusercontent.com/89927660/132109682-c1230a08-5131-4178-ad34-57abe0c4fa31.png)

>_**Observation:** When i0 is high, the output follows i1. When i0 is low, the output latches to a constant value (when both i0 and i2 are 0). Presence of inferred latches due to incomplete if structure._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 5 43 16 PM](https://user-images.githubusercontent.com/89927660/132109711-b1387435-c889-457d-971c-9749058b5f16.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 5 43 54 PM](https://user-images.githubusercontent.com/89927660/132109713-86418751-819c-4666-adc9-faae7fa03988.png)

>_**Note:** The synthesized design has a D Latch inferred due to incomplete if structure (missing else statement)._

### INCOMPLETE CASE STATEMENTS

#### _CASE 1: incomplete case statements_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 9 37 44 PM](https://user-images.githubusercontent.com/89927660/132113320-a73dbbda-58c7-4bb5-acbd-19a45f600226.png)

>_**Expected Behavior:** There is an incomplete case structure, so a latch is expected._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 9 46 23 PM](https://user-images.githubusercontent.com/89927660/132113329-74d9023b-9b40-4622-a6c2-e63931d4a496.png)

>_**Observation:** When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the ouput latches to the previously available value._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 9 54 29 PM](https://user-images.githubusercontent.com/89927660/132113365-45d871b2-bf70-41c9-814f-52a639dcde66.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 9 55 08 PM](https://user-images.githubusercontent.com/89927660/132113368-c8dde325-4b17-4b72-b5a5-c00691944690.png)

>_**Note:** The synthesized design has a D Latch inferred due to incomplete case structure (missing output definition for 2 of the select statements)._

#### _CASE 2: incomplete case statements with default_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 10 01 40 PM](https://user-images.githubusercontent.com/89927660/132113527-ffd675e0-90f5-4c68-a83e-346dd7d899ac.png)

>_**Expected Behavior:** There is an incomplete case structure but with a default condition, so a latch is not expected._

**_Screenshot: Simulation Output_**
![Screen Shot 2021-09-04 at 10 03 58 PM](https://user-images.githubusercontent.com/89927660/132113528-0860166f-359a-4e84-8eb9-cf88887349e0.png)

>_**Observation:** When select signal is 00, the output follows i0 and is i1 when the select value is 01. Since the output is undefined for 10 and 11 values, the presence of default sets the output to i2 when the select line is 10 or 11. The ouput will not latch and be a proper combinational circuit. 

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 10 04 47 PM](https://user-images.githubusercontent.com/89927660/132113530-1c111485-4604-45a5-a2e0-db2164e71d40.png)

**_Screenshot: Synthesis Output_**
![Screen Shot 2021-09-04 at 10 05 16 PM](https://user-images.githubusercontent.com/89927660/132113531-aa0c5fec-8590-46e0-8add-d5fe444e4190.png)

>_**Note:** The synthesized design has combinational logic without latch due to the presence of default case statement._

#### _CASE 3: partial case statement_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-04 at 10 17 14 PM](https://user-images.githubusercontent.com/89927660/132113918-f56c0602-67be-4385-90af-19f2a42208b8.png)

>_**Expected Behavior:** There is a partial case structure with output of x undefined for one of the select values, so a latch is not expected._

>_**Observation:** The mux for output y will not have a latch, while there will be a latch for mux with output x as one of the conditions is not defined_

|Select|Output y|Output x|
|:---:|:---:|:---:|
|00|i0|i2|
|01|i1|Latch|
|10|i2|i1|
|11|i2|i1|

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 10 26 18 PM](https://user-images.githubusercontent.com/89927660/132113992-7d21193c-0c3a-498c-91dd-ef6008e399ef.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 10 26 47 PM](https://user-images.githubusercontent.com/89927660/132113995-a1ab773b-d335-40e6-9fb2-e838195eb24a.png)

>_**Note:** The synthesized design has a latch due to partial case statement for output x. Though we write default condition, there can be inferred latches._

#### _CASE 4: overlapping case statement_

**_Screenshot: Verilog file_**

[Screen Shot 2021-09-04 at 11 12 34 PM](https://user-images.githubusercontent.com/89927660/132115407-c4412126-8e23-4247-b022-c53b7692285a.png)

>_**Expected Behavior:** Although the case structure is not complete, there is overlapping of output when the select input is 10 or 11 and ? represented that the bit can be wither 0 or 1. Thus, the simulator may be confused._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-04 at 11 34 45 PM](https://user-images.githubusercontent.com/89927660/132115404-89e9d456-7539-472a-b997-68d0ba580ed1.png)!

>_**Observation:** Here, when the select input is 11, the output value is latched to a value._

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-04 at 11 25 49 PM](https://user-images.githubusercontent.com/89927660/132115256-ef57bde4-aeb4-4972-a386-1192cf52b3c4.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-04 at 11 26 54 PM](https://user-images.githubusercontent.com/89927660/132115352-f3c7db8b-7a1c-4aa4-b5ac-52e12d7d60db.png)

>_**Note:** It can be inferred that there is no Latch in the synthesized netlist as the case structure is complete (no presence of inferred latches)_

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-04 at 11 31 42 PM](https://user-images.githubusercontent.com/89927660/132115350-36d494b8-3ac6-4a07-9b2e-542727765459.png)

>_**Observation:** There is no latch observed in the output. The synthesizer tool does not get confused. Hence there is a Synthesis Simulation Mismatch due to overlapping of legs in the code. Care must be taken to address the legs individually without any overlap (mutually exlusive code)_

### STATEMENTS USING FOR

#### _Understanding the Usage of For and Generate Statements:_

|FOR STATEMENTS|GENERATE STATEMENTS|
|:---:|:---:|
|These statements are used inside the always block|These statements are used outsde the always block|
|Used for evaluating expressions|Used for instantiating/replicating Hardwares|

#### _CASE 1: using generate if statement_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-05 at 8 49 51 AM](https://user-images.githubusercontent.com/89927660/132129900-ce94545c-870a-4ff4-9383-75d3f05d68ff.png)

>_**Expected Behavior:** The structure is complete and expected to behave as a 4x1 multiplexer_

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-05 at 8 52 42 AM](https://user-images.githubusercontent.com/89927660/132129937-56e67283-0b21-4441-b211-413a139f4740.png)

>_**Observation:** It is a 4x1 multiplexer behavior which is given as mentioned below:

|Select|Output y|
|:---:|:---:|
|00|follows i0|
|01|follows i1|
|10|follows i2|
|11|follows i3|

#### _CASE 2: demux using case statement.v_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-05 at 11 41 16 PM](https://user-images.githubusercontent.com/89927660/132161659-389ddef4-3c6b-47f7-b572-b79743d0872d.png)

>_**Expected Behavior:** All the outputs are initialised to 0, to avoid inferring laches. Depending on the select line, the input is allocated to one of the outputs._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-05 at 12 07 29 PM](https://user-images.githubusercontent.com/89927660/132135459-7816c574-db47-4496-a538-14d212d3cbd6.png)

>_**Observation:** It is a 1x8 multiplexer behavior which is given as mentioned below:

|Select|i|
|:---:|:---:|
|000|follows o0|
|001|follows o1|
|010|follows o2|
|011|follows o3|
|100|follows o4|
|101|follows o5|
|110|follows o6|
|111|follows o7|

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-05 at 12 08 43 PM](https://user-images.githubusercontent.com/89927660/132135558-079f066c-a9c8-4141-ac1c-4d9bd1d120fa.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-05 at 12 09 13 PM](https://user-images.githubusercontent.com/89927660/132135563-9b89aaa8-3448-44d1-87f0-f793c7acc527.png)

>_**Note:** It can be inferred that there is no Latch in the synthesized netlist as the case structure is complete (no presence of inferred latches)_

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-05 at 12 12 40 PM](https://user-images.githubusercontent.com/89927660/132135595-c7c29683-fd3f-4cd4-9e50-b4f4b1ff1e4a.png)

>_**Observation:** The observed waveform in simulation and synthesis matches and conforms code functionality._

#### _CASE 3: demux using generate if statement.v_

>_**Note:** The experiment of using demux with generate if statement functions same as of demux with case statement. However, the advantage of using generate if statements is the number of lines in the code which almost remains the same even if the input lines in demultiplexer increases._

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-05 at 12 24 01 PM](https://user-images.githubusercontent.com/89927660/132135896-e4efeba9-b86c-4f4e-96c4-d20151d97652.png)

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-05 at 12 26 38 PM](https://user-images.githubusercontent.com/89927660/132135966-93f134be-122d-48a2-a2ba-45e6b81965ab.png)

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-05 at 12 35 50 PM](https://user-images.githubusercontent.com/89927660/132136219-14057e13-2c62-40eb-9309-599ffcb75c86.png)

**_Screenshot: Synthesis Output_**

![Screen Shot 2021-09-05 at 12 36 22 PM](https://user-images.githubusercontent.com/89927660/132136222-52bcbf8b-8ef6-45e1-834f-abd57bcb2f28.png)

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-05 at 12 38 40 PM](https://user-images.githubusercontent.com/89927660/132136226-5415ab92-a10b-4f89-9ca8-2c018de483b0.png)

>_**Observation:** The observed waveform in simulation and synthesis matches and conforms code functionality._

### STATEMENTS USING GENERATE

#### Experiment on Ripple Carry Adder_

>_**Note:** Instantiating the full adder in a loop to replicate the hardware_

**_Screenshot: Verilog file_**

![Screen Shot 2021-09-05 at 1 26 25 PM](https://user-images.githubusercontent.com/89927660/132161283-c5fbc45c-fe1b-40e5-bc41-beda3761bc5a.png)

>_**Note:** The output is always n+1 bits if both the inputs ate n bits. Since we are instantiating a full adder present in separate file, there is a need to tell the definition of full adder. It can also be seen that there is no always block used. The variable is genvar instead of integer._

**_Screenshot: Simulation Output_**

![Screen Shot 2021-09-05 at 12 56 30 PM](https://user-images.githubusercontent.com/89927660/132137158-7554ce78-5a46-4ea3-aa8f-a876ddaf129d.png)

**_Screenshot: Synthesis Statistics Report_**

![Screen Shot 2021-09-05 at 1 11 18 PM](https://user-images.githubusercontent.com/89927660/132137159-50399943-457b-4341-a514-6a50eb6d0253.png)

**_Screenshot: Synthesis Output - rca_**

>_**Note:** read_verilog for rca is performed before read_fa._

![Screen Shot 2021-09-05 at 1 14 36 PM](https://user-images.githubusercontent.com/89927660/132137364-8bbe0f9e-059f-4684-8849-00154c90dcb6.png)

**_Screenshot: Synthesis Output - fa_**

![Screen Shot 2021-09-05 at 1 14 09 PM](https://user-images.githubusercontent.com/89927660/132137361-7702ede2-1de9-43a9-a8c4-7d523673cbb3.png)

**_Screenshot: Commands for performing GLS**

![Screen Shot 2021-09-05 at 1 17 12 PM](https://user-images.githubusercontent.com/89927660/132137382-8d8b38d5-65ed-4c6d-af6b-ffbe54d81710.png)

**_Screenshot: GLS Output_**

![Screen Shot 2021-09-05 at 1 21 36 PM](https://user-images.githubusercontent.com/89927660/132137370-6835d03a-9cb4-409a-8031-68424d9f1adb.png)

>_**Observation:** The observed waveform in simulation and synthesis matches and conforms code functionality._
