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
## Introduction to Verilog RTL Design and Synthesis

_Register Tranfer Level_ is a low level abstraction representation of a design a circuit. RTL design facilitates the designers by automating the process of converting the functionality of a digital circuit written in Hardware Description Languages to its equivalent combinational and sequential circuit with the help of Synthesis tools. Design is the actual Verilog code of set of Verilog codes which has the intended functionality to meet the required specifications - or implementation of the specification. RTL design is checked for adherence to specification by simulating the design. _Simulator_ is used to verify or check the design, in this Workshop, the simulator tool used is iverilog. _Testbench_ is the setup to apply stimulus which are also called as test vectors to the design to check its functionality. 

### Understanding the Design and Test Bench Modules

to insert block diagram 

>Note: Design module may have one or more primary inputs and primary outputs. However, testbench will not have any primary input or output.  

### iVerilog Based Simulation Flow

to insert block diagram

>Note: Simulator continuously checks for changes in the input. If there is an input change, the output is evaluated; else the simulator will never evaluate the output.

### Setting up the Environment and Cloning the Repositories from GitHub

![Screen Shot 2021-09-01 at 5 40 21 PM](https://user-images.githubusercontent.com/89927660/131754679-41ae4441-1c3b-40ba-9876-fc5d2d655c91.png)

> Steps Followed: I have created a VLSI directory, successively two GitHub clonings were performed which are 1. https://github.com/kunalg123/vsdflow
and 2. https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop 

sky130RTLDesignAndSynthesisWorkshop Directory has: My_Lib - which contains all the necessary library files; where lib has the standard cell libraries to be used in synthesis and verilog_model with all standard cell verilog models for the standard cells present in the lib. Ther verilog_files folder contains all the experiments for lab sessions including both verilog code and test bench codes. 

### Good Multiplexer Implementation using iVerilog and GTKWave!

![Screen Shot 2021-09-01 at 5 47 49 PM](https://user-images.githubusercontent.com/89927660/131755360-c66415a2-5135-456e-82ba-1b7c4a6c3a07.png)

```
//Load the design in iVerilog by giving the verilog and testbench file names
$ iverilog good_mux.v tb_good_mux.v 
//List so as to ensure that it has been added to the simulator
$ ls
//To dump the VCD file
$ ./a.out
//To load the VCD file in GTKwaveform
$ gtkwave tb_good_mux.vcd
```

 







































----------------------------------------------------------------------------------------------------------------------------------------------------------------
# Day 2
## Timing Libs, Hierarchial Vs Flat Synthesis and Efficient Flop Coding Styles
