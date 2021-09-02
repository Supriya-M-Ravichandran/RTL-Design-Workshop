# RTL DESIGN USING VERILOG WITH SKY130 TECHNOLOGY
![68747470733a2f2f7777772e766c736973797374656d64657369676e2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323032312f30352f566572696c6f672d666c7965722e706e67](https://user-images.githubusercontent.com/89927660/131698504-1010e82e-58c8-4474-bf88-191c120f36ad.png)

## About the Workshop
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

## Workshop Credits
> VSD - Kunal Ghosh and Team: https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/
----------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 1
## 1. Introduction to Verilog RTL Design and Synthesis

_Register Tranfer Level_ is a low level abstraction representation of a design a circuit. RTL design facilitates the designers by automating the process of converting the functionality of a digital circuit written in Hardware Description Languages to its equivalent combinational and sequential circuit with the help of Synthesis tools. Design is the actual Verilog code of set of Verilog codes which has the intended functionality to meet the required specifications - or implementation of the specification. RTL design is checked for adherence to specification by simulating the design. _Simulator_ is used to verify or check the design, in this Workshop, the simulator tool used is iverilog. _Testbench_ is the setup to apply stimulus which are also called as test vectors to the design to check its functionality. 

### 2. Understanding Design and Test Bench Modules

to insert block diagram 

>_**Note:** Design module may have one or more primary inputs and primary outputs. However, testbench will not have any primary input or output._  

### 3. iVerilog Based Simulation Flow

to insert block diagram

>_**Note:** Simulator continuously checks for changes in the input. If there is an input change, the output is evaluated; else the simulator will never evaluate the output._

### 4. Setting up the Environment and Cloning the Repositories from GitHub
>**Steps Followed:** _I have created a VLSI directory, successively two GitHub clonings were performed which are 1. https://github.com/kunalg123/vsdflow
and 2. https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop _

![Screen Shot 2021-09-01 at 5 40 21 PM](https://user-images.githubusercontent.com/89927660/131754679-41ae4441-1c3b-40ba-9876-fc5d2d655c91.png)

sky130RTLDesignAndSynthesisWorkshop Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes. 

### 5. Good Multiplexer Implementation using iVerilog and GTKWave!
#### _What's a Mux? _

### 6. Implementation using iVerilog
iVerilog invoking command should have both source and testbench Verilog code. Once it was run, it created a.out file which dumped the vcd file. GTKwave viewer was utilized to open the vcd dump file and analyze the waveform. 

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

![Screen Shot 2021-09-01 at 5 47 49 PM](https://user-images.githubusercontent.com/89927660/131755360-c66415a2-5135-456e-82ba-1b7c4a6c3a07.png)

### 7. GTKWaveform Viewer Results to verify the Design Functionality
![Screen Shot 2021-09-01 at 3 49 04 AM](https://user-images.githubusercontent.com/89927660/131778505-5929cc06-72a2-4d20-ae16-4c4cc72e0730.png)

### 8. GTKWaveform Viewer Results to verify the Design Functionality
Additionally, the verilog and test bench modules can be accessed using the following command: 
```
gvim tb_good_mux.v -o good_mux.v 
```
![image](https://user-images.githubusercontent.com/89927660/131779232-ca559d03-c7a4-4607-81bb-eedc9c0c1eca.png)

### 9. Introduction to Yosys and Logic Synthesis
Synthesizer is the tool that helps to convert RTL to netlist. Netlist is the representation of the design in the form of standard cells (in the form of the cells present in the .lib). Design and netlist file should be one and the same. 

#### _But, How to Verify the Synthesis Output?_
The stimulus should be same as the output observed in the RTL simulation. The design was written in Verilog code and netlist is the standard cell representation of the code. The set of primary inputs and primary outputs will remain the same between the RTL design and synthesized netlist. It implies that, we can use the same testbench as RTL design. 

#### _Logic Synthesis_
RTL Design is the behavioral representation in HDL form for the required specification.

But How to map the code with the hardware circuit? Synthesis - RTL to gate level translation. The design is converted into gates and the interconnections are made between the gates. This is given out as a file named as netlist. We take the RTL design, combine with .lib and synthesis it to get the netlist file. 

>_Note: .lib file is a collection of logical modules which includes all basic logic gates. It may also contain different flavors of the same gate (2 input AND, 3 input AND – slow, medium and fast version)._

#### _Cell Selection _
Fast and Slow cells comes with its own advantages and disadvantages when we consider the critical delays in combinational circuits. It is necessary to guide the synthesizer for selecting the cells that is optimum for implementing the logic circuits. This is called as _Constraints._ 

>_Note: Faster Cells lead to increased area and power, potentially leading to hold time violations. Slower Cells will result in slow circuits and may fail to meet the performance._

### 10. Synthesizing the Design 
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
```

**_Screenshot: steps till Design Synthesis_**

![Screen Shot 2021-09-01 at 4 58 07 AM](https://user-images.githubusercontent.com/89927660/131780523-4e1e1cae-1e08-45b7-828e-996eeb6e2ed0.png)

**_Screenshot: Inference from Synthesis and Execution of netlist generation using abc command_**

![Screen Shot 2021-09-01 at 5 00 32 AM](https://user-images.githubusercontent.com/89927660/131780607-a8cb8499-c715-4f6b-a777-d95b4e3f1d0d.png)

**_Screenshot: Inference from abc command_**

![Screen Shot 2021-09-01 at 5 08 05 AM](https://user-images.githubusercontent.com/89927660/131780717-6824e63d-1897-4700-bd36-32b10758410a.png)

**_Screenshot: Graphical Representation of the Logic using show command_**

![Screen Shot 2021-09-01 at 5 10 22 AM](https://user-images.githubusercontent.com/89927660/131780762-e8cf32bc-b156-4ac9-a692-2f09fc2c243e.png)






 







































----------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 2
## Timing Libs, Hierarchial Vs Flat Synthesis and Efficient Flop Coding Styles
