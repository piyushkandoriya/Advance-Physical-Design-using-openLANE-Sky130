# **Advance Physical Design using OpenLANE/Sky130**
# Contents 
 <div class="toc">
  <ul>
    <li><a href="#header-1">Day 1 - Inception of open-source EDA, OpenLANE and sky130 PDK</a></li>
	<ul>
        <li><a href="#header-1_1"> How to talk to computers</a></li>
      </ul>
      <ul>
        <li><a href="#header-1_2">Soc design and OpenLANE</a></li>
      </ul>
	<ul>
        <li><a href="#header-1_3">Get familiar to open-source EDA tools</a></li>
      </ul>
   </div>
  
<div class="toc">
  <ul>
    <li><a href="#header-2">Day 2 - Good floor planning considerations</a></li>
	<ul>
        <li><a href="#header-2_1"> Chip Floor planning consideration</a></li>
      </ul>
      <ul>
        <li><a href="#header-2_2">Library building and Placement</a></li>
      </ul>
	<ul>
        <li><a href="#header-2_3">Cell design and characterization flows</a></li>
      </ul>
	  <ul>
        <li><a href="#header-2_4">General timing characterization parameters</a></li>
      </ul>
</div>
  
  <div class="toc">
  <ul>
    <li><a href="#header-3">Day 3 - Design library cell using Magic Layout and ngspice characterization</a></li>
	<ul>
        <li><a href="#header-3_1"> Labs for CMOS inverter ngspice simulations</a></li>
      </ul>
      <ul>
        <li><a href="#header-3_2">Inception of layout ̂A CMOS faabrication process </a></li>
      </ul>
	<ul>
        <li><a href="#header-3_3">Sky130 Tech File Labs</a></li>
      </ul>
   </div>
	  
<div class="toc">
  <ul>
    <li><a href="#header-4">Day 4 - Pre-layout timing analysis and importance of good clock tree</a></li>
	<ul>
        <li><a href="#header-4_1">Timing modeling using delay tables</a></li>
      </ul>
      <ul>
        <li><a href="#header-4_2">Timing analysis with ideal clocks using openSTA</a></li>
      </ul>
	<ul>
        <li><a href="#header-4_3">Clock tree synthesis TritonCTS and signal integrity</a></li>
      </ul>
	  <ul>
        <li><a href="#header-4_4">Timing analysis with real clock using openSTA</a></li>
      </ul>
</div>
	
<div class="toc">
  <ul>
    <li><a href="#header-5">Day 5 -Final step for RTL2GDS using tritinRoute and openSTA</a></li>
	<ul>
        <li><a href="#header-5_1">Routing and design rule check (DRC)</a></li>
      </ul>
      <ul>
        <li><a href="#header-5_2">Power Distribution Network and routing</a></li>
      </ul>
	<ul>
        <li><a href="#header-5_3">TritonRoute Features</a></li>
      </ul>
</div>
	
<div class="toc">
  <ul>
    <li><a href="#header-6">References</a></li>
  </ul>
</div>

<div class="toc">
  <ul>
    <li><a href="#header-7">Acknowledgement</a></li>
  </ul>
</div>

# <h1 id="header-1">Day 1 -Inception of open-source EDA, OpenLANE and sky130 PDK</h1>	 
## <h1 id="header-1_1"> How to talk to computers?</h1>
To perform the intruction, which are given externaly in computers, some hardware is there which convert our external instruction into understandable language of computers. In this hardware, many things are there. For example FPGA board, Arduino board etc. are provides the medium for external inputs and outputs.

Taking the example of Aurdino board. Arduino consists of both a physical programmable circuit board (often referred to as a microcontroller) and a piece of software, or IDE (Integrated Development Environment) that runs on the computer, used to write and upload computer code to the physical board.
	
![compontents on arduno](https://user-images.githubusercontent.com/123488595/214484537-9bb5894c-dc9d-4ce9-8caf-913e92fd6b99.jpg)
Here in the board, the ATmega328 microcontroller is used. Basically microcontroller is made from package, inside the package chip is there.Chip is made by pads, core, die and foundary IP's.
### Package
The package is a case that surrounds the circuit material to protect it from corrosion or physical damage and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). here what we are seeing in the black color is the Package of the microcontroller.

<img width="449" alt="image" src="https://user-images.githubusercontent.com/123488595/214485578-ac54fa09-0d56-4f3a-9e68-5475933be6c0.png">
	
### Chip
Inside the Package, chip is available which contains, foundary IP's (for example, RISC-V Soc, PLL, ADC, DAC, SRAM, SPD), core (core contains AND, OR etc. all gates and digital logics), pads (through which input and output signals are communicates).

![51c0d009ce395feb33000000](https://user-images.githubusercontent.com/123488595/214487804-3e5596fc-2690-4649-bb2c-7074cec97f9d.jpg)
	
## What is RISC-V?
RISC-V, where five refers to the number of generations of RISC architecture that were developed at the University of California, Berkeley. RISC is an open standard instruction set architecture (ISA) based on established RISC principles. Unlike most other ISA designs, RISC-V is provided under open source licenses that do not require fees to use. A number of companies are offering or have announced RISC-V hardware, open source operating systems with RISC-V support are available, and the instruction set is supported in several popular software toolchains.

The instruction set is designed for a wide range of uses. The base instruction set has a fixed length of 32-bit naturally aligned instructions, and the ISA supports variable length extensions where each instruction can be any number of 16-bit parcels in length. The instruction set specification defines 32-bit and 64-bit address space variants. The specification includes a description of a 128-bit flat address space variant, as an extrapolation of 32 and 64 bit variants, but the 128-bit ISA remains "not frozen" intentionally, because there is yet so little practical experience with such large memory systems.
	
## How software communicate with Hardware?

Between apps or software (in our mobilephones and computers or laptops) and Hardware (in our mobilephones and computers or laptops), One full channel is there, which contains O.S. (operating system), compiler and assembler. This channel proccesses the inputs from the apps and gives the outputs to the Hardware. And according to the output of assembler, the hardware will perform.

<img width="206" alt="Screenshot 2023-01-25 115108" src="https://user-images.githubusercontent.com/123488595/214494625-6f94b210-f7fb-4d16-bf4b-c7de6239cd91.png">

when we gives the inputs from the software, inputs is in the C,C++,JAVA etc. languages. Hardware not understand this language. So, this inputs goes throuth the system software cahnnel, where O.S. handles the input/output operations, it allocates the memory to for the codes and low level systems functions are there in O.S. Then program goes to the compiler, where program will compile and generate the HDL program.This HDL program is basically the sets of Instructions which can hardware is able to understand. The instructions are basically depends on what kind of hardware we are using. For example if we are using Mips processor then instruction formaate will be according to the Mips processor, if Hardware is RISC-V then the instruction formate will be according to the RISC-V and similerly for Arm and intel processors also.
	
This sets of instrusctions is now given to the Assembler. here according to the instruction set, assembler will generate the Binary code (contains only 0s and 1s). this binary code can be understandable for hardware. And according to this code, hardware will gives the output. 
	
## how to design Digital ASIC?
To design Digital ASIC, few tools or things which are required from the day one.
These are
	<ul>
        <li><a>RTL Design</a></li>
      </ul>
      <ul>
        <li><a>EDA tools</a></li>
      </ul>
	<ul>
        <li><a> PDK data</a></li>
      </ul>
	
### what is RTL design?
In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.for this designs many open sorces are available. like, librecores.org, opencores.org, github.com, etc...
	
### What is EDA tools?
The term Electronic Design Automation (EDA) refers to the tools that are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. many open sorces tools are available like Qflow, OpenROAD, OpenLANE, etc...
	
### What is PDK Data?
PDK is process design kit. It is interface between FAB and design. This data is collections of files like,
	<ul>
        <li><a>process design rules: DRC, LVS, REX</a></li>
      </ul>
      <ul>
        <li><a>Digital standerd cell libreries</a></li>
      </ul>
	<ul>
        <li><a>i/o librerirs </a></li>
      </ul>
	<ul>
        <li><a> etc..... </a></li>
      </ul>
which are used to model a fabrication process for the EDA tools used to design an ICs.
for example, in 2020, google release the open source PDK for FOSS 130nm production with the skywater technology. But right now it is at cutting age of the 5 nm also. But in many applications, the advance node is not required, and the cost of advanced node is also high as compared to 130nm processors.
This 130nm processors are also fast processor.
for example,
	<ul>
		<li><a> intel: P4EE @3.46 GHz(Q4'o4)</a></li>
	</ul>
	<ul>
		<li><a>sky130_OSU (single cycle RV32i CPU)
			pipeline version can achieve more than 1 GHz clock</a></li>
	</ul>
	
## Simplified RTL to GDSII flow
<img width="264" alt="image" src="https://user-images.githubusercontent.com/123488595/214504937-264c46d8-f6c7-4b8e-8f8d-87b216f8a067.png">
### Synthesis
In the synthesis, the design RTL is translated to a circuit out from the SCL. The resultant circuit is describes in HDL	and usualy refered to the gate level netlist. the gate level netlist is functionaly equivelent to the RTL.
"standard Cells" have regular layouts.
	
### Floor planning and Power planning
The main objective here is that to plan silicon area and distribute the power to the whole circuit.
In the chip floor planning, the partition chip die between different system building blocks and place the i/o pads. In micro floor planning, we define the dimensions, pin locations, rows.
![20-Figure1 3-1](https://user-images.githubusercontent.com/123488595/214507829-d049bb33-d11b-427e-818c-85a8c43f2f4f.png)

In power planning, the power network is connstructed. tipically, the chip is power by multiple VDD and GND. so, total components are connected to power supply horizontaly and vertically by metal streps. here parallel structures are used to reduce the resistance.
To address the electromagnetization problem, power distribution network uses upper metal leyers, which are thicker than lower metal layers. Hence have less resistance.
![Picture1](https://user-images.githubusercontent.com/123488595/214508763-040d992c-e41b-40f1-b9d0-b1553888a8e1.png)

### Placement
In this process, we place the gate level netlist on the floor planning rows, alligned with the sites. cells should be placed very closed to eachother to reduce the interconnnect delay.
Usually placement is done in 2 steps:
<ul>
	<li><a>Global placement</a></li>
	</ul>
<ul>
	<li><a>Detailed placement</a></li>
	</ul>	

![image](https://user-images.githubusercontent.com/123488595/214510218-dd7cc40c-c627-47cb-ac9a-0fc2a484c08b.png)

#### Global placement
Global placement is very first stage of the placement where cells are placed inside the core area for the first time looking at the timing and congestion. Global Placement aims at generating a rough placement solution that may violate some placement constraints while maintaining a global view of the whole Netlist.
	
#### Detailed placement
In detailed placements, we determined the exact route and layers for each netlist. the objective of detailed placement is valid routing, minimize area and meet timing constrains. Additional objective is minimum via and less power.

### Clock tree synthesis (CTS)
Before routing the signals, we have to route the clock. In the process of clock synthesis, we have distribute the clock to the every sequential elements. for example flipflops, registers, ADC, DAC ete. basically clock netwroks looks likes a tree. where the clock source is roots and the clock elements are end leaves.
Synthesization should be done in a manner that with minimum skew and in a good shape.To minimize the clock skew by using the low-skew global routing resources for clock signals.Microsemi devices provide various types of global routing resources that significantly reduce skew.Usually a tree is a H tree, X tree etc.
![image](https://user-images.githubusercontent.com/123488595/214568639-23e7cc83-1f6a-4abd-b1d3-a8a251a07450.png)

### Routing
After routing the clock, the signal routing comes. Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined.
There are two types of nets in VLSI systems that need special attention in routing: 
<ul>
	<li><a>Clock nets</a></li>
	</ul>
<ul>
	<li><a>Power/Ground nets</a></li>
	</ul>	
The sky130 PDK defines the 6 routing leyers. the lowest leyer is called local interconnect layer (titanium nitride layer). Other five layers are alluminium layers.

![metal_stack-900x795](https://user-images.githubusercontent.com/123488595/214572611-63a2130c-0901-454e-9188-1eddacdd86ca.png)


In the proccess of routing, metal trackes forms a routing grids and these grids are huge. so, devide and conquer approach is use for routing.
The two types of routing is used:
<ul>
	<li><a>Global routing: Generates the routing guides</a></li>
	</ul>
<ul>
	<li><a>Detailed Routing: Uses the routing guides to implement the actual wiring</a></li>
	</ul>		

### Sign off
Once the routing is done, we can construct the final layout. This final layout will goes under the verification. Two types of verifications are there:
<ul>
	<li><a>Physical verification: Here design rule checking will done and it will check the final layout and owners layout</a></li>
	</ul>
<ul>
	<li><a>Timing Verification: Here Static Timing Analysis will done</a></li>
	</ul>	

## Introduction to OPENLANE:
OPENLANE is an automated RTL to GDSII flow that is composed of several tools such as OpenROAD, Yosys, Magic, Netgen, Fault, CVC SPEF-Extractor, CU-GR, Klayout and a number of scripts used for design exploration and optimization.
It is started as an Open-source flow for a true Open Source tape-out Experiment.
striVe is a family of open everything SoCs:
	<ul>
	<li><a>Open PDK</a></li>
	</ul>
<ul>
	<li><a>Open EDA</a></li>
	</ul>	
	<ul>
	<li><a>Open RTL</a></li>
	</ul>	
### striVe SoC Family
<img width="376" alt="image" src="https://user-images.githubusercontent.com/123488595/214582995-2ac885d9-c76a-4242-bb9c-b8994679af25.png">
The main goal of OPENLANE is to produce a clean GDSII with no human intervation (no-human-in-the-loop). 
here the meaning of clean is that:
<ul>
	<li><a>No LVS violations</a></li>
	</ul>
<ul>
	<li><a>No DRC Violations</a></li>
	</ul>	
	<ul>
	<li><a>No timing Violations</a></li>
	</ul>	
OPENLANE is tuned for skyWter130nm open PDK. it can be used to harden Macros and chips.there
