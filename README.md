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
	
## <h1 id="header-1_2">How to design Digital ASIC?</h1>
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
OPENLANE is tuned for skyWter130nm open PDK. it can be used to harden Macros and chips.there is two mode of operation
<ul>
	<li><a>Autonomus : it is the push botton flow. with the push botton , it is a some time base design and due to this push botton, we get final GDSII </a></li>
	</ul>
<ul>
	<li><a>interactive : here we can run comamds and steps one by one.</a></li>
	</ul>	

It has  large number of design examples(43 designs with their best configurations).

## OpenLANE ASIC Flow (Detailed)
<img width="386" alt="Screenshot 2023-01-25 195527" src="https://user-images.githubusercontent.com/123488595/214588975-3737d250-704b-45d6-aa0b-b0ceb6639030.png">

The design exploration utility is also used for regression testing(CI).
we run OpenLANE on ~ 70 designs and compare the results to the best known ones. 
### DFT(Design for Test)
it perform scan inserption, automatic test pattern generation, Test patterns compaction, Fault coverage, Fault simulation. 
<img width="439" alt="image" src="https://user-images.githubusercontent.com/123488595/214592208-63cb6c82-1241-413f-b7f9-3d94c1641d6a.png">

After that physical implementation is done by OpenROAD app. physical implementation involves the several steps: 
	<ul>
	<li><a>Floor/Power Planning</a></li>
	</ul>
<ul>
	<li><a>End Decoupling Capacitors and Tap cells insertion</a></li>
	</ul>	
	<ul>
	<li><a>Placements: Global and Detailed</a></li>
	</ul>	<ul>
	<li><a>Post Placement Optimization</a></li>
	</ul>
<ul>
	<li><a>Clock Tree synthesis (CTS)</a></li>
	</ul>	
	<ul>
	<li><a>Routing: Global and Detailed</a></li>
	</ul>	
Every time the netlist is modified.(CTS modifies the netlist and Post Placements optimization also modifies the netlist).so for that verification must be performed. The LCE(yosys) is used to formally confirm that the function did not change after modifying the netlist.  
### Dealing with antenna rules Violation:	
when a metal wire segment is fabricated, it can act as antenna.as an antenna, it collect charges which can demaged  the transister gates during the fabrication.
<img width="242" alt="image" src="https://user-images.githubusercontent.com/123488595/214597006-5c0808dc-e94e-4f52-96da-c90e191a7d20.png">
To address this issue, we have to limit the lenght of the wire. usually this is the job of the router. If router fails to do this, then there are two solutions:
	<ul>
	<li><a>Bridging attaches a higher layer intermediary</a></li>
	</ul>
<img width="203" alt="image" src="https://user-images.githubusercontent.com/123488595/214597897-29ee947f-6ab5-4df6-bc81-2944f77895f4.png">

<ul>
	<li><a>Add antenna diode cell to leak away charges.(Antenna diodes are provided by the SCL)</a></li>
	</ul>	
	
<img width="77" alt="image" src="https://user-images.githubusercontent.com/123488595/214598376-902b092c-d03c-4178-abc2-2ed0d4621bb8.png">
With OpenLANE, we took a preventive approach. here we add fake antenna diode next to every cell input after placement. Then run the Antenna checker on the routed layout. If the checker reports a violation on cell input pin, replace the fake diode cell by a real one.
<img width="272" alt="image" src="https://user-images.githubusercontent.com/123488595/214599628-8c0f2729-2e43-4025-b263-34b6c502654c.png">
	
### Static Timing analysis(STA)
It involves the interconnect RC Extraction(DEF2SPEF) from the routed layout, followed by STA on OpenSTA(OpenROAD) tool. resulting report will shows the timing violations if any violations is there.
	
### Physical Verification (DRC and LVS)
Magic is used for design Rules checking and SPICE Extraction from Layout.
Magic and Netgen are used for LVS.

## <h1 id="header-1_3">Open source EDA tools</h1>
### OpenLANE Directory Structure in detail 
#### cd command
cd means change directory. this command will help us to go inside the directory.
#### ls command
ls means listing the directory. It is used to find the list of total details of directory.
#### ls --ltr
This command will help to list the details in cronological order.
	
Here we are working in Sky130_fd_sc_hd PDK varient. where, "sky130" is process name or node name."fd" is a foundary name (skyWater foundary)."sc" means standerd cell librery files and the last one "hd" stands for high density(basically one type of varient). 

Sky130_fd_sc_hd varient contains many technology files like verilog, spice, techlef, meglef,mag,gds,cdl,lib,lef,etc. (techlef file contains the layer information).

### Design Preparation Step
when we enter in the OpenLANE, we have to use flow.tcl because as a name says, it will goes with the flow using the script. And by using interactive switch, we will do step by step process. without interactive switch, it will run complete flow from RTL to GDSII.
Now OpenLANE is open and we can see that prompt will change now.
<img width="992" alt="image" src="https://user-images.githubusercontent.com/123488595/214751196-c0bf70a0-b862-4452-89a2-2af81a89e86b.png">

Now we have to input all the packages which required to run the flow.
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/214752282-34a58730-3c57-44d0-b95d-f738a0e6b9a7.png">
Now, here we are ready to execute the command.
	
Now, if we are going into  the design folder in openlane, there are nearly 30-40 designs are already builted. Out of them we can open any of the design. for example, here we are opening the picorv32a.v design.
In  this design we can see many files are available. i.e., scr, config.tcl, etc. This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214754184-bfa2b6d1-4535-4cba-a1d8-adc19fb97077.png">
Here we can see that the time period is set to the 5.00 nsec. but is we see in the openlane sky130_fd_sc_hd folder, the period is set about 24 nsec. so it is not override to the main file. If it override then give first priority to the main folder.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214755677-28582a34-c102-435e-92e6-87ef0c14542c.png">

Now, in openlane, we are going to  run the synthesis, but before synthesis, we have to prepare design setup stage.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214756580-52cc86ed-874d-4025-bcaa-37cc0629de9a.png">

so, here it is shown that preparation is completed.

###Review after design preparation and run synthesis
After completing the preparation, in the picorv32a file, the run terictory is created. Inside the folder, Today's date is created. so in this terictory some folders are available which is required for openlane.
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/214791870-44553cb1-cee9-4048-ae8e-f9b4e4a6b018.png">

In the temp file, merged.lef file is available which was created in preparation time. if we open this merged.lef file, we get all the wire or layer level and cell level information. 
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214793001-9d40a092-8477-40e4-b544-67d5f2a39549.png">
<img width="996" alt="image" src="https://user-images.githubusercontent.com/123488595/214793517-13d079a0-24c8-4552-b85c-e939a17283f9.png">

While, in the result folder is empty because till we have not run anything and in the report folder all the folders are there about synthesis, placement, floorplanning,cts,routing,magic,lvs.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214795641-2c5f3f0e-6e59-4b7a-b962-8be331bb7e35.png">

now here also one config.tcl file is available similar like design folder. But this config.tcl file contains all default parameter taken by the run.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214796646-4539b897-1ff5-49f4-9c21-3a85ad31af6b.png">

when we make some change in the origional configuration and then we run, for example if we make a change in core utilization in the floorplanning and then we run the floorplanning, at this time in the congig.tcl file, the core utility will change and by cross checking it we can check that the modification is reflected in the exicution or not.

Now, cmds.log file takes all the record of the commands, what we have fab.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214798208-4cc5f491-c8d2-4a95-a254-3c0e1647991f.png">
	
Now coming to the openlane, we are going to run the synthesis. It will take some 3-4 mnts to run the synthesis and finally synthesis will complited.
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/214799290-906f704e-5fe3-4cac-989a-9d809acc1809.png">

From the data of synthesis, total number of counter D_flip-flops is 1613. and the number of cells is 14876.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214804529-510e50db-0153-4024-9ba2-f4b523f4d1e0.png">
	
So, the flop ratio = (number of flip flops)/(number of total cell).
So, the flop ratio is 10.84%.

Before run, we saw that the result folder is empty. but now, after running the synthesis, we can see that all the mapping have been done by ABC.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214806172-4e595c85-a0ac-4b4a-9a85-408806be9434.png">
	
And in the report, we can see when the actual synthesis has done. and the actual statistics synthesis report is showing below, which is same as what we have seen before.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214807555-eab9d596-c396-4c4f-a28d-4773f3e08bdc.png">


# <h2 id="header-2">Day 2 -Good floorplanning Vs Bad floorplanning and introduction to library cells</h2>	 
## <h2 id="header-2_1"> Chip floorplanning considerations</h2>
### Utilization factor and aspect ratio
#### 1)defining the width and height of core and Die
![core-to-ioclearence](https://user-images.githubusercontent.com/123488595/214812877-3a95861f-095f-4ae3-aaeb-1a1cf744ea71.jpeg)
let's begin with netlist( Netlist describes the connectivity of an electronic designs). Considering a netlist with 2 flops and 2 gates.
<img width="266" alt="image" src="https://user-images.githubusercontent.com/123488595/214813921-eaf8a832-f81a-407f-9872-9c26418504f0.png">

lets giving the height and width of standerd cell is 1 unit. So, area of standerd cell is 1 square unit. And assuming the same area for the flip flops also.
	
Now lets calculate the area occupied by the netlist on a silicon wafer by arranging them together. The total area occupied by netlist in silicon wafer is 4 square units.
	
<img width="118" alt="image" src="https://user-images.githubusercontent.com/123488595/214814862-ed648466-d3e7-439d-9bbd-5e6a9e35dba9.png">

#### what is the core and die?
A 'core' is the section of the chip where the fundamental logic of the design is placed.
	
A 'Die', which is consist of core, is small semiconductor material specimen on which the fundamental circuits is fabricated.
![image](https://user-images.githubusercontent.com/123488595/214816013-c1bdb7f5-5e79-4c26-a944-d6de35671c47.png)
	
If we put the 'arranged netlist' in the core, then whole core is occupied by netlist. that means utilization of core is 100%.
	
<img width="122" alt="image" src="https://user-images.githubusercontent.com/123488595/214824163-afcf3f11-4a79-4159-b39d-6fa4676bfeca.png">

#### Utilization factor
it is defined as, 'the area occupied by netlist' devided by the 'total area of core'. 
	
so, in above case, the utilization factor is 1.
	
practically, we don't go with 100% uti;ization. practically utilization factor is about 50%-60% to add other extra cells (like buffer) in the core after netlist is placed.
	
#### Aspect Ratio
it is defined as (height)/ (width). here in above example the aspect ratio is 1.
	
### Concept of pre-placed cells
#### 2) Define locations of preplaced cells
to define the preplaced cells, let's take a combinational logic i.e., mux, multiplier, clock devider, etc. and assuming that the output of the circuit is huge and assuming that the circuit has some 100k gates. so let devides in two blocks of 50k gates.

<img width="366" alt="image" src="https://user-images.githubusercontent.com/123488595/214839792-c8f43ba0-18ca-443b-a75e-790406bc5f05.png">

This both blocks are implimented separatly. Now, extending the input and output pins from the both blocks. now, let's detached these box. we will black box the boxes and detached them. After black boxing, the upper portion is invisible from the top or invisible to the one , who is looking into the main netlist. now we make separate them out.
	
<img width="324" alt="image" src="https://user-images.githubusercontent.com/123488595/214841034-cdf45413-3402-4fa4-b2f3-864f2bb5a3d3.png">

This blocks are implemented in netlist once and then we can reuse it multiple time. Similarly, there are other modules or IPs also readily available.i.e.,memory, clock gating cell, comparater, mux. these all are part of the top level netlist.They recieve some signals, they perform some functions, they deliver the outputs but the functionality of the cell is implemented only once. That what we called as preplaced cells.
	
The arrangement of these ip's in a chip is refferd as floorplanning.These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "preplacement cells".These cells are place din such fashion that, the placement and routing tool not touch the location of the cell.

![image](https://user-images.githubusercontent.com/123488595/214843837-bbdba3e6-5783-4a8c-bb34-39881337adca.png)

Let consider memory as a preplaced cells as a block 'a', block 'b' and block 'c'. Memory of any device is implemented once and reused multiple times. these preplaced cells are located as per the design bacckground. the location of the cells are never touched.

### De-coupling capacitors
#### 3) surround pre-placed cells with Decoupling capacitors

Let consider some circuit, which is part of the blocks which were defined above. When some gate (let consider AND gate) switched from 0 to 1 ot 1 to 0, considered amount of the switching current required because of small capacitance is available there. this capacitor should be completely charged to represent logic 1 and completly discharged to represent logic 0. the required amount of the charge is suplied from the Vdd and absorb from the Vss. during supplying the current, wire has some drop of voltage due to resistence and inductunce of the physical wire.

<img width="301" alt="image" src="https://user-images.githubusercontent.com/123488595/214847108-8f9747bf-1e44-49f7-9d70-6bba5594f1ef.png">

So, due to this if ideal logic 1 = 1 volt then here practically it can be less then 1 volt i.e., 0.97 volts (Vdd'). So, for any signal to be considered as Logic '0' and '1' in the NM low and NM high range. It is danger case.
	
<img width="334" alt="image" src="https://user-images.githubusercontent.com/123488595/214848285-1f8b322e-ecfc-4941-813f-2f65f4c4149e.png">

To solve this problem,, we have to put De-coupling capacitor in parallel with the circuit. Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replacenish the charge into Cd.
	
<img width="296" alt="image" src="https://user-images.githubusercontent.com/123488595/214848866-96646aa8-2073-4467-9428-b014761add21.png">
	
<img width="278" alt="image" src="https://user-images.githubusercontent.com/123488595/214849500-510229d7-5859-43eb-aed7-936b758f395b.png">

### Power Planning
#### 4) How to do power planning?
Up to here, we have taken care of local communication. Now let consider that local circuitory as a black box and it can be repeat multiple times and power supply is also connected to all of them as shown below,

<img width="291" alt="image" src="https://user-images.githubusercontent.com/123488595/214851756-38f569d9-46ee-4150-813b-f9fd79e4c5e9.png">

Now 16 bit bus has to retain the same signal from driver to the load. so it should get the sufficient power from the supply. But at this bus, there is no de-coupling capacitor is available because it is not physible to put capacitor at all over the place. now, power supply is far away from the bus, that is why some drop between them will definalty occurs.
	
Let consider this 16 bit bus connected to inverter. So, all capacitor are initially charged will get discharged and vice-versa due to inverter.
	
<img width="322" alt="image" src="https://user-images.githubusercontent.com/123488595/214853264-d023b43c-3e2b-401b-a699-c3d6bcdf6199.png">

But the problem is occurs due to all capacitor is connected to the single ground. This will cause a bump in 'ground' tap point during discharging.
	
<img width="306" alt="image" src="https://user-images.githubusercontent.com/123488595/214854259-e538ca11-c396-4fb3-8c72-28c2d5f59d2e.png">

Same problem will occurse during the charging time also. at that time lowering of voltage occurse at 'Vdd' tap point.
	
<img width="320" alt="image" src="https://user-images.githubusercontent.com/123488595/214854772-0daf74b3-bfae-4fa9-9130-ae81840576e7.png">

The solution of the problem is use multiple power supply. So, every block will take charge from neartest power supply and similarly dump the charge to the nearer ground. this type of power supply is called mesh.

<img width="284" alt="image" src="https://user-images.githubusercontent.com/123488595/214855314-308e092e-a3d1-4ef4-97fa-9508f01605e7.png">

And the power planning is shown below,

<img width="362" alt="image" src="https://user-images.githubusercontent.com/123488595/214856244-a322a374-660c-454b-b252-8d40fa1fd8c7.png">

### Pin placement and Logic floor planning considerations
#### 5) Pin Placement
Lets take below designs for example that needs to implemented.
	
<img width="302" alt="image" src="https://user-images.githubusercontent.com/123488595/214858342-5e11cfc4-e283-4a0b-9def-1d2001ac11fe.png">

Both the sections has different inputs like Din1 and Din2 and operated from different clocks like clock 1 and clock 2. along with that we have preplaced cells as well. So, basically now we have 4 input ports and 3 output ports.

let's have one more design that needs to be implemented. this types of circuits are very much helpful to understand the timing analysis of inter clocks. 

now complete design becomes like given below which has 6 input ports and 5 output ports. it is called the 'Netlist'.

<img width="256" alt="image" src="https://user-images.githubusercontent.com/123488595/214859546-e01e96f8-f8ae-4d32-b850-e866cc884e95.png">
	
Let's put this netlist in the core which we have designed before and let's try to fill this empty area between core and die with the pin information. The frontend team who decides the netlist input and output and the backend team who done the pin placements. So according to the pin placements, we have to locate the preplaced blocks nearer to the inputs of the preplaced blocks.
	
<img width="361" alt="image" src="https://user-images.githubusercontent.com/123488595/214863991-eb0bd738-d4e8-43c9-9975-b89d1ea3d1da.png">

Here one thing that we noticed is that clock-in and clock-out pins are bigger in size as compared to input and output pins. reason behind this is that, input clocks are conntinuously provides the signal to the every elements of the chip and output clock should out the signal as fast as possible. So, we need least resistance path for the clocks inputs and clocks outputs. So, bigger the size, lower the resistance.
	
One more thing is need to take care about is that, this pin placement area is blocked for routing and cell placements. so we nned to do logical  cell placement blockage. this blockage is shoown in above image in between pins.
	
So, floor plan is ready for Placement and Routing step.

### Steps to run floorplan using OpenLANE
#### 6)Placement and routing 
Before run the floorplanning, we required some switches for the floorplanning. these we can get from the configuration from openlane.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214902095-52b164f2-1a0a-4a98-b8fd-c281a7f216be.png">

Here we can see that the core utilization ratio is 50% (bydefault) and aspect ratio is 1 (bydefault). similarly other information is also given. But it is not neccessory to take these values. we need to change these value as per the given requirments also. 
	
Here FP_PDN files are set the power distribution network. These switches are set in the floorplane stage bydefault in OpenLANE.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214906731-4768f143-e9ea-4b24-82dd-2c9c972c9649.png">

Here, (FP_IO MODE) 1, 0 means pin positioning is random but it is on equal distance.

In the OpenLANE lower priority is given to system default (floorplanning.tcl), the next priority is given to config.tcl and then priority is given to PDK varient.tcl (sky130A_sky130_fd_sc_hd_congig.tcl).

Now we see, with this settings how floorplan run.
	
### Reviewing floorplan files and steps to view floorplan
In the run folder, we can see the connfig.tcl file. this file contains all the configuration that are taken by the flow. if we open the config.tcl file, then we can see that which are the parameters are accepted in the current flow. 
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214913959-ae328082-9f18-42fb-b405-6ccefeb95ae7.png">

here we can see that, the core utilization is 35%, aspect ratio is 1 and core margin is taken as 0. while in default the core utilization is 50%. this is the issue. because this design is override the system. but it is the taken from PDK varient.tcl file. so priority vise it is true. 
	
To watch how floorplane looks, we have to go in the results. in the result, one def( design exchange formate) file is available. if we open this file, we can see all information about die area (0 0) (660685 671405), unit distance in micron (1000). it means 1 micron means 1000 databased units. so 660685 and 671405 are databased units. and if we devide this by 1000 then we can get the dimensions of chips in micrometer. 

<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/214918258-f3bc54e7-644d-491b-ae74-737f510fc3da.png">

so, the width of chip is 660.685 micrometer and height of the chip is 671.405 micrometer.

To see the actual layout after the flow, we have to open the magic file by adding the command  magic -T Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../temp/merged.lef def read picorv32a.floorplan.
 
And then after pressing the enter, Magic file will open.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214920878-ecb2c0a6-4c7c-4d9c-9d48-cb8de2ddc8b3.png">

### Reviewing floorplan layot with magic.
