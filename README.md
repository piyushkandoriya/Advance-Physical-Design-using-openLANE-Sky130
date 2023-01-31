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
    <li><a href="#header-6">All commands to run the openlane flow</a></li>
  </ul>
</div>

	
<div class="toc">
  <ul>
    <li><a href="#header-7">References</a></li>
  </ul>
</div>

<div class="toc">
  <ul>
    <li><a href="#header-8">Acknowledgement</a></li>
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

Now, in openlane, we are going to  run the synthesis, but before synthesis, we have to prepare design setup stage. for that command is " prep -design picorv32a".
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214756580-52cc86ed-874d-4025-bcaa-37cc0629de9a.png">

so, here it is shown that preparation is completed.

### Review after design preparation and run synthesis
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

To see the actual layout after the flow, we have to open the magic file by adding the command magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def 
 
And then after pressing the enter, Magic file will open. here we can see the layout.

<img width="722" alt="image" src="https://user-images.githubusercontent.com/123488595/214922787-863a11a5-a0c6-46b1-aa78-0d1ee50ed8b6.png">

### Reviewing floorplan layot with magic.
In the layout we can see that, input output pins are at equal distance.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/214925799-51b74403-022f-45a3-bef1-af63bb9f4180.png">

after selecting (To select object, first click on the object and then press 's' from keyboard. the object will hight lited. to zoom in the object, click on the object and then press 'z' and for zoom out press 'sft+z') one input pin, if we want to check the location or to know at on which layer it is available, we have to open tkcon window and type "what". it will shows all the details about that perticular pin.
	
<img width="256" alt="image" src="https://user-images.githubusercontent.com/123488595/214927377-fb000f07-9c01-42a7-9752-75e41a064aca.png">
<img width="738" alt="image" src="https://user-images.githubusercontent.com/123488595/214927471-148b9473-2b6c-417c-8dad-a08ce3a23d33.png">

so, it show that the pin is in the metal 3.similarly doing for the vertical pins, we find that this pin is at metal 2.

<img width="397" alt="image" src="https://user-images.githubusercontent.com/123488595/214928363-e058f29b-8a86-4a97-a50f-a68a2b263981.png">

Along with the side rows,the Decap cells are arranged at the border of the side rows.
	
<img width="121" alt="image" src="https://user-images.githubusercontent.com/123488595/214929406-c05aba68-2dbe-4e53-8a99-7339c642b730.png">

Another cells also placed here, which is a tap cells. these cells are meant to avoide the latch-up problems in the CMOS devices. it connect N-well to the Vdd and substrate to the Ground. these tap cells are placed at diagonally equal distance.

<img width="335" alt="image" src="https://user-images.githubusercontent.com/123488595/214930507-60abe484-3a08-426f-a609-2203d412d37b.png">

In the floorplane, standerd cells are not placed but here standerd cells are available in the left side of the floorplan. we can see few boxes are there.

<img width="362" alt="image" src="https://user-images.githubusercontent.com/123488595/214931774-2097438f-183e-4358-9954-d1d0c858d5c7.png">

here we can see that first standerd cells is for buffer 1. similarly other cells are for buffer 2, AND gate etc.


## <h2 id="header-2_2"> Library building and Placement</h2>
### Netlist binding and initial place design
#### 1) bind netlist with physical cells
Taking netlist as what we taken before,

<img width="256" alt="image" src="https://user-images.githubusercontent.com/123488595/214859546-e01e96f8-f8ae-4d32-b850-e866cc884e95.png">

Here, we can see that every gate or flip-flops have a shape to understand the functionality of the element. But practically, this cells are square or rectangular boxes which has internally some logic to perform. So, here we are taking all the elements from netlist and giving them a perfact height and width with perticular dimention. These all cells together are called 'self'. And this self are stored in the lybrary. Library have all the innformation about all the blocks, like height, width , time delay, conditional innformation, etc. library also have a option for the similar cells (with same functionality) like this with different height and width. According to our space available at floorplanning we can choose out of them.

<img width="404" alt="image" src="https://user-images.githubusercontent.com/123488595/215013265-5593bead-4617-4b43-b571-a09dec2e2a7f.png">

After giving size and shape to each and every box, next step is to take the boxes or element from library and placed in the floorplan. This is called placements of the cells.According to the design of the netlist, we have to put physical blocks in the floorplan which we have design before.Put all the blocks according to the input and output of that perticular blocks.
	
<img width="420" alt="image" src="https://user-images.githubusercontent.com/123488595/215014213-bf21cb29-075a-48f5-adfb-49334ab70e41.png">

up to  here we have done stage one and stage two placement. Now we will going for stage 3 and 4. here we have to place FF1 of stage 3 nearer to the Din3 and FF2 of stage 3 nearer to the Dout3. But Din3 and Dout3 are at somme distance from eachother. same thing is there for FF1 and FF2 of stage 4. Let's we placed these all element in such manner that all elements are closed to it's input and output pins.
	
<img width="421" alt="image" src="https://user-images.githubusercontent.com/123488595/215015116-5ccae340-8e21-4b7b-b158-547fa9434995.png">

But, the distance of FF1 of Stage 4 and Din4 is still far them others. By optimizing the placement, we can solve this problem.

### Optimize placement using estimated wire lenght and capacitance
#### 2) Optimize Placement
	
As we seen that the distance from Din2 to FF1 of stage 2 is higher. so if we connect the wire between them then resistance and capacitance of the wire comes in to the picture. and due to this the signal integrity can not maintain.
	
To maintain the integrity of the signal out from Din2 to FF1, we have to put some repeaters like buffers on between Din2 and FF1. But it will cause of loss of area.

In the stage 1, there is no need of any repeater to transmit the signal. But in stage 2, due to high distance, the lenth of wire is high and signal is not transmitted in perticular range. so we required repeater.

<img width="418" alt="image" src="https://user-images.githubusercontent.com/123488595/215016936-d2ec5589-23d9-4384-8bfd-4cf89ef696f5.png">

### Final Optimization
Similar as stage 2, in Stage 3 also we required the buffer between gate 2 and FF2.

<img width="280" alt="image" src="https://user-images.githubusercontent.com/123488595/215017463-923fccd6-3655-41e0-979e-1eb954a02fb6.png">

Stage 4 is bit tricky as compared to other stages.
<img width="288" alt="image" src="https://user-images.githubusercontent.com/123488595/215017652-db5355ec-6225-4cc5-b39f-b2744af5bcb9.png">

Now we have to check that, what we have done is correct or not. For that we need to do Timing analysis by considering the ideal clocks and according to the data of analysis, we will understand that, the placement is correct or not.

### Need for libraries and characterization.
#### Library charactorization and modelling
In whole IC design, we have to go through synthesis, floor/power planning, placement, routing , STA. In all this steps one thing remain common, which is "Gates or Cells". That is where the library charactorization becomes very important.

### Congestion aware placement using RePLAce
Right now we are not constrain about timing, but constrain about the congestion. so, we are making the congrstion is less.
	
The placement is donne in two stages. Global and detailed. In global placement, legalization is not happened but after detailed placement legalization will be done.

When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires. 
	
Now opening the Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below,
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215032086-358f617b-f98a-4b27-b2b2-1b357fd32bdf.png">

If we zooom into this, we find the buffers, gates, flip flops in this.
	
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/215032429-60b3d2cf-1e20-4108-96cd-aa58a26c66d0.png">

## <h2 id="header-2_3"> Cell design and characterization flows</h2>
### Inputs for cell design flow
#### Cell design flow
As we know that standerd cells are placed in the library. And in the library many other cells are available which have same functionality but the size is  different. As size is different the parameters like hVt, Ivt also different for each standerd cells.

If we take one of the standerd cells called inverter, it has their own design flow by this it can be understandable to EDA tool.
	
The cell design flow is devided into three part.
<ul>
	<li><a> Inputs</a></li>
	</ul>
<ul>
	<li><a> Design steps</a></li>
	</ul>
<ul>
	<li><a> Outputs</a></li>
	</ul>
	
<img width="394" alt="image" src="https://user-images.githubusercontent.com/123488595/215036081-d28d9e5d-7a79-4783-9e64-536854625428.png">

#### 1)inputs 
inputs required for cell design is PDKs, DRC and LVS rules SPICE models, library and user defined specs.
	
### Circuit design steps
#### 2)design steps
steps are circuit design, layout design, characterization
	
#### 3)Outputs
The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).
	
### Layout design steps
<ul>
	<li><a> implement function</a></li>
	</ul>

<img width="87" alt="image" src="https://user-images.githubusercontent.com/123488595/215039759-6e586324-32ef-4b40-8637-ecc1638254a1.png">

<ul>
	<li><a> Derive the NMOS and PMOS network graph</a></li>
	</ul>
<img width="193" alt="image" src="https://user-images.githubusercontent.com/123488595/215039824-c3b83fc5-b07e-4236-818a-36d38293feac.png">
<ul>
	<li><a> Obtain the Euler's path</a></li>
	</ul>
<img width="253" alt="image" src="https://user-images.githubusercontent.com/123488595/215040185-e52d3b61-4804-4022-b720-53a2bd19a601.png">
<ul>
	<li><a> Stick diagram</a></li>
	</ul>
<img width="98" alt="image" src="https://user-images.githubusercontent.com/123488595/215040590-8c3d1c3b-9565-4291-98c7-fa16033e1427.png">
<ul>
	<li><a>Convert stick diagram into proper layout</a></li>
	</ul>
<img width="137" alt="image" src="https://user-images.githubusercontent.com/123488595/215040862-bd6c960f-cecd-4206-a4d7-9e4a5606a704.png">

After layout design, we have to ecxtract the layout and characterize it.
In characterization step, we can get the information about Timing, Noice,power.libs and function.
	
### Characterization Flow
As of now, from the circuit design and layout design, we have final layout of buffer cell. where two buffers are connected in series with each other.
	
<img width="131" alt="image" src="https://user-images.githubusercontent.com/123488595/215042474-c46416f4-7d0d-4a4d-8359-4d24144a2773.png">

Now steps of flow is:
<ul>
	<li><a>Read the model file</a></li>
	</ul>
<ul>
	<li><a>read the extracted spice netlist</a></li>
	</ul>
<ul>
	<li><a>reconize the behavior of buffer</a></li>
	</ul>
<ul>
	<li><a>Read the subcircuit of the inverter</a></li>
	</ul>
<ul>
	<li><a>Ateched the neccessory power source</a></li>
	</ul>
<ul>
	<li><a>Apply the stimulus</a></li>
	</ul>
<ul>
	<li><a>Provide the necessory of output capacitance</a></li>
	</ul>
<ul>
	<li><a>Provide the necessory simulatin command </a></li>
	</ul>

This all steps we have to give in "GUNA" software. and this software will give the timing, noise, power.libs and functions.

## <h2 id="header-2_4"> General Timing characterization parameters</h2>
### Timing threshold defination
<img width="182" alt="image" src="https://user-images.githubusercontent.com/123488595/215048381-19739890-636b-4bed-a09d-f9216480abfb.png">
Let we take the waveform from the output of the first buffer and it will  be input of the second buffer and taking output of the second buffer also.

<img width="191" alt="image" src="https://user-images.githubusercontent.com/123488595/215048979-74e91dfe-b67e-42fc-8845-c70834af6314.png">

<ul>
	<li><a> slew_low_rise_thr</a></li>
	</ul>
here low means nearer to the ground, and rise tresold means we want to measer the slope of the increasing graph. typical value of slew low rise thr is around 20-30%.

<img width="300" alt="image" src="https://user-images.githubusercontent.com/123488595/215049488-7ddcb037-80d0-4e73-8f68-b28590d60e06.png">

<ul>
	<li><a> slew_high_rise_thr</a></li>
	</ul>
same as above, high means nearer to high value.
<img width="292" alt="image" src="https://user-images.githubusercontent.com/123488595/215049970-f216caee-409c-4574-b24c-58510f3d89fb.png">

whenever we want to calculate the slew, take the point at 20% from the low and take the point at 20% from the high. according to these point, take the time data and the time difference between them will helps to calculate the slew.

<ul>
	<li><a> slew_low_fall_thr</a></li>
	</ul>
<img width="294" alt="image" src="https://user-images.githubusercontent.com/123488595/215050698-648aadb1-27bc-43e6-abce-1fc4c2190006.png">

<ul>
	<li><a> slew_high_fall_thr</a></li>
	</ul>
<img width="298" alt="image" src="https://user-images.githubusercontent.com/123488595/215050796-b15d839d-8730-4e6c-af7b-23fe448e2013.png">

NOw, taking the waveform of input stimulus which is input of the first buffer and with that taking output of the first buffer.

Similar as a slew, thresolds are for delay also available. for that same as slew, we have to take some rise and fall points from the waveforms. this tresolds are almost 50%.

<ul>
	<li><a> in_rise_thr</a></li>
	</ul>
<img width="311" alt="image" src="https://user-images.githubusercontent.com/123488595/215051338-72102b38-51b1-4d97-9472-632e0f2c4fed.png">

<ul>
	<li><a> in_fall_thr</a></li>
	</ul>
<img width="291" alt="image" src="https://user-images.githubusercontent.com/123488595/215052122-9f78d4d0-2b4d-4162-b11c-a32b8bbec35f.png">

<ul>
	<li><a> out_rise_thr</a></li>
	</ul>
<img width="297" alt="image" src="https://user-images.githubusercontent.com/123488595/215052265-fdfbf6f5-91dd-42c6-ba31-6941450539a7.png">

<ul>
	<li><a> out_fall_thr</a></li>
	</ul>
<img width="295" alt="image" src="https://user-images.githubusercontent.com/123488595/215052377-70e01f13-1a61-49a3-9b36-8ff7ae7246ed.png">

So, according to rise and fall theresold we can find the rise delay and fall delay of the buffer.
	
<img width="110" alt="image" src="https://user-images.githubusercontent.com/123488595/215056233-6b5329b2-dd9a-4159-ab95-3354f83f7adf.png">


### Propogation delay and Transition time
#### propogation delay
let's take the same setup for understand the propogation delay.
Time delay = Time(out_*_thr)-time(in_*_thr).

Let's take waveform on which we can apply above formula.
	
<img width="362" alt="image" src="https://user-images.githubusercontent.com/123488595/215054264-65f24a74-d1eb-42f3-a1ba-ed3b13e79781.png">

In any case if thresold point move at the top, at that time we get negative delay because output comes before input. so reason behind the negative delay is the poor choice of the tresold points. which is not acceptable. so, choosing the thresold point is very important.
	
<img width="365" alt="image" src="https://user-images.githubusercontent.com/123488595/215054514-f6de9845-2a66-422b-b7e8-7fff32e47cb8.png">

Taking another example where the wire delay is very high because of high distance between two elements. so, here by choosing correct thresold point then also we get the negative delay.

<img width="366" alt="image" src="https://user-images.githubusercontent.com/123488595/215055564-4dfad413-5dcf-47b0-89ca-bfa81a55163f.png">

#### Transition time
transition time = time(slew_high_rise_thr)- time(slew_low_rise_thr)

or

transition time = time(slew_high_fall_thr)- time(slew_low_fall_thr)

let's take waveform for understand the slew calculation.
	
<img width="378" alt="image" src="https://user-images.githubusercontent.com/123488595/215056482-d42a9a99-69e6-44cc-b34c-f7934e03f95c.png">

	
# <h3 id="header-3">Day 3 -Design library cells using Magic Layout and ngspice characterization</h3>	 
## <h3 id="header-3_1"> Labs for CMOS inverter ngspice simulations</h3>
### SPICE deck creation for CMOS innverter
#### IO placer revison
Till now, we have done floor planning and run placement also. But if we want to change the floorplanning, for example, in our floor planning, pins are at equal distance and if we want to change it then we can also make it by "Set" command.

For that first we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.
	
Then check the changes in the pins location through magic -T. 
	
<img width="410" alt="image" src="https://user-images.githubusercontent.com/123488595/215105996-bea3fa72-0f26-4510-b646-e81b9fa5c235.png">

So, here we can see that there are no pins in the upper half side. all pins are in the lower half of the core.
	
### SPICE deck creation for CMOS inverter
#### VTC- SPICE simulations.
Before entering into the simulation, we have to creat the spice deck. The spice deck is nothing but netlist. so, we have to creat the spice deck for CMOS.
<ul>
	<li><a>Component connectivity</a></li>
	</ul>
	
<img width="143" alt="image" src="https://user-images.githubusercontent.com/123488595/215108987-e86fc8db-4dc0-43e9-829d-b6f7e74ef429.png">

<ul>
	<li><a>Define the components values</a></li>
	</ul>
	
<img width="173" alt="image" src="https://user-images.githubusercontent.com/123488595/215109110-7e889154-84dd-4160-9f0b-6e358162394f.png">

<ul>
	<li><a>Identyfy the nodes</a></li>
	</ul>
	
<img width="232" alt="image" src="https://user-images.githubusercontent.com/123488595/215109227-89c761c3-169c-45c4-b43a-d030c714e406.png">
<ul>
	<li><a>Name 'Nodes'</a></li>
	</ul>
	
<img width="181" alt="image" src="https://user-images.githubusercontent.com/123488595/215109418-16ba1203-64fa-4934-85d2-cc437c6e75ab.png">

Now, let's strat the writing the SPICE deck

<img width="197" alt="image" src="https://user-images.githubusercontent.com/123488595/215111052-63e1162e-aa89-419d-a648-317064d488c4.png">

Here, in the syntex, It is like Name of the mosfet, drain , gate, substrait , source. so, the meaning of the syntex is that, name of the mosfet is M1, Drain is connected to OUT node, Gate is connected to IN node, Substrait is connected to Vdd and the Source is connected to Vdd node. PMOS says the type of mosfet and the Width and lenth of channel is defined. similarly, For M2, syntex were written.

### SPICE simulation lab for CMOS
Tile we discribe the connectivity information about CMOS inverter only. Now we have to discribe connectivity information about other components also like source, capacitor etc. so, lets look into the other components.
	
First we discribe the load capacitor and then about the Vdd and Vin.

<img width="76" alt="image" src="https://user-images.githubusercontent.com/123488595/215141012-10b78ed8-a530-4e64-baaf-788bf6e3376d.png">

Now, we have to give simulation command. which is about swiping the Vin from 0 to 2.5 with the steps of 0.05. Because we want Vout while changing the Vin.

<img width="134" alt="image" src="https://user-images.githubusercontent.com/123488595/215142224-2e6c197b-3bdd-4d0b-8b72-7e60f1d54fc9.png">

The final step is to discribe the Model file.Model file contains all the details about PMOS and NMOS. from this file only we get the information about PMOS and NMOS.

<img width="194" alt="image" src="https://user-images.githubusercontent.com/123488595/215142962-f3fabd61-32fb-47a0-96d8-79e2c676e325.png">

So, the total program is given below,

<img width="206" alt="image" src="https://user-images.githubusercontent.com/123488595/215143090-58abcfb4-a7f4-45ac-b836-fb98fd6d84bc.png">

Now, doing the simulation and get the graph like this,

<img width="233" alt="image" src="https://user-images.githubusercontent.com/123488595/215184372-825fb2f1-1c27-4dde-965a-3c05aad7a438.png">

Now, doing other simulation in which we change the PMOS width to 3 times of NMOS width. and after diong the simulation, we get the graph like this,
	
<img width="242" alt="image" src="https://user-images.githubusercontent.com/123488595/215184798-5c94dcbf-76d1-4650-aa90-1fa7d08ae0b1.png">

The difference between this two graph is that in the second graph the transfer charactoristic is lies in the ecxact middle of the graph where in the first graph it is lies left from the middle of the graph.
	
### Switching Thresold Vm
These both model of different width has their own application. By comparing this both waveform, we can see that the shape of the both waveform is same irrespective of the voltage level.
	
This thing tell us that when Vin is at low, output is at high and when Vin is at high, the output is at low. so the charactoristic is maintain at all kind of CMOS with different size of NMOS or PMOS. That is why CMOS logic is very widely used in the design of the gates.

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.
	
<img width="311" alt="image" src="https://user-images.githubusercontent.com/123488595/215187444-2372bb51-c7ad-477b-a1b3-720b0e05273b.png">

In this figure, we can see that at Vm~0.9v, Vin=Vout. This point is very critical point for the CMOS because at this point there is chance that both PMOS and NMOS are turned on. If both are turned on then there is chances of leakage current(Means current flow direcly from power to ground).
	
By comparing this both the graph we can understang the concept of switching thresold voltage.
	
<img width="379" alt="image" src="https://user-images.githubusercontent.com/123488595/215189055-41cc046d-ece5-4152-b993-74161a01cf58.png">

<img width="157" alt="image" src="https://user-images.githubusercontent.com/123488595/215189213-60ab895b-7d0e-46bc-9504-871f8c7d08c1.png">

### Lab steps to git clone vsdstdcelldesign

To get the clone, copy the clone address from reporetery and paste in openlane terminal after the command "git clone". this will create the folder called "vsdstdcelldesign" in openlane directory.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215191871-c413fd56-c0ff-4519-a375-9c2bf2baae92.png">

now, if we open the openlane directory, we find the vsdstdcelldesing folder in the openlane directory.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215192335-6fbfbd33-5cf4-44c4-b402-a839cbaa8ce6.png">

Now if we goes in the vsdstdcelldesign folder and open it, we get the .mag file,libs file etc.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215192846-38ddf587-4c87-4e31-a159-372efe8c5223.png">

now, let's open the .mag file and see that which layers are used to build the inverter. But before opening the mag file, we need tech file. so we will copy this file from this given below address,

<img width="544" alt="image" src="https://user-images.githubusercontent.com/123488595/215194300-6c23a4fb-bb78-484c-b87f-c2fde8a73b83.png">

And do copy by "cp" command to the location which is given below,
	
<img width="944" alt="image" src="https://user-images.githubusercontent.com/123488595/215195032-dff99099-ab03-48a4-b531-910a064c7e72.png">

Now, we can see that this file is copied in the vsdstdcelldesign folder.

<img width="958" alt="image" src="https://user-images.githubusercontent.com/123488595/215195394-1620db66-70ea-46c3-a74a-d3b56621c370.png">

Now, here to see the layout in magic, we don't need to write the whole address because we copy the tech file here.

Now, appying the magic comand like this,
	
<img width="950" alt="image" src="https://user-images.githubusercontent.com/123488595/215197031-1f1defa5-2d3e-4c4b-ba4f-b179349d7593.png">

Now, we can see the layout of CMOS inverter in the magic like this,
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215197708-068e7137-9a58-420a-bdc3-d144e0157e74.png">

## <h3 id="header-3_2"> Inception of layout CMOS fabrication process</h3>
### create active region
#### 1) selecting a substrate
we select P-type silicon substrate with high resistivity(5~50 ohms) with moderate dopping and orientation is (100).
	
#### 2) creating active region for transister
First, we create the isolation layer by depositing the Sio2 layer (~40nm) on the substrate.

<img width="272" alt="image" src="https://user-images.githubusercontent.com/123488595/215252142-c2147410-9f36-40d3-88e4-1526eb1cdee9.png">

Now, we are depisiting the Si3N4 layer (~80 nm) on the Sio2 layer.

<img width="274" alt="image" src="https://user-images.githubusercontent.com/123488595/215252210-7b3ff105-40a2-434d-a390-6fe15f5e218d.png">

Now we do patterning by using depositing photoresist and using Mask 1 through UV light.

<img width="275" alt="image" src="https://user-images.githubusercontent.com/123488595/215252310-a6fdff19-7012-4568-929e-f50238d70ae3.png">

Now, first we remove the mask and doing etching of Si3N4 layer on the exposed area.
	
<img width="266" alt="image" src="https://user-images.githubusercontent.com/123488595/215252424-861d5b11-5a8c-412c-b703-5d60c216f88c.png">

Now, next step is to remove photoresist by chamical reaction, because now to Si3N4 layer itslef behaves like good protecting layer for Sio2 layer. now, if we do LOCOS (local oxidation of silicon) process, the exposed sio2 part will gown and bird break also form. This grown sio2 will provide the perfect isolation between two PMOS and NMOS.
	
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215252733-28b2b5c6-1b49-4c4d-8775-efc41960637f.png">

Next step is to etchout the Si3N4 layer by hot phosphoric acid.
	
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215252781-33240e0c-00c6-414f-9273-4e2122bd26e3.png">

### Formation of N-well and P-well
#### 3) N-well and P-well formation
we can not form P-well and N-well at a same time. we have to protect a region while forming one of the region by photoresist. And then using mask 2 and UV light, we will do patterning of photoresist to form P-well.
	
<img width="268" alt="image" src="https://user-images.githubusercontent.com/123488595/215252984-cf790063-7157-4323-b9d8-afddb6bbab37.png">

Now, the area where we want to form the P-well is exposed. now we remove the mask and by applying the ion implantaton method (~200kev)to form P-well using Boron. But still it is P implant. After performing the high temparature anneling, it will become P-well.

<img width="267" alt="image" src="https://user-images.githubusercontent.com/123488595/215253131-9fefda80-ae5c-476d-a46e-de4f9075117e.png">

We wiil do a similar process to form N-well by using mask 3 and using Phosphorus ions.
	
<img width="269" alt="image" src="https://user-images.githubusercontent.com/123488595/215253198-7b012a1e-f628-4e29-bf7d-53e066096459.png">

till now depth of wells are not define. so, by putting into the high temparature furnace (drive-in diffusion), we will define the depth of wells.
	
<img width="305" alt="image" src="https://user-images.githubusercontent.com/123488595/215253381-112f305f-d877-4e81-a3ba-57d4ef89272e.png">

### Formation of gate terminal
#### 4)Gate formation
Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only we can control the thresold voltage. doping concentration and oxide capacitance will control the thresold voltage.
	
so, first we are maintain the doping concentration here. for that we use mask 4 and again doing the ion implantation of boron ion at lower energy (~60kev).

<img width="275" alt="image" src="https://user-images.githubusercontent.com/123488595/215258532-fee8ed20-056e-4213-a808-598342b05c3a.png">

same process we will repeat for N-well also by using mask 5 and Arsenic ion.
	
<img width="283" alt="image" src="https://user-images.githubusercontent.com/123488595/215258621-22be43d0-f29d-45a5-9b75-2d417401b97f.png">

Next step is that we have to fix the oxide layer. but before that we have to remove the oxide layer because this layer is got dammeged because of the privious processes. so,first we remove the layer using HF solution and again re-grown the high quality oxide layer with same thickness. 
	
The final step is the deposition of polysilicon layer over oxide layer with more impurities for low resistance gate terminal.Then etched out this polysilicon layer by using mask 6 and photoresist.
	
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215259001-0f7d377d-359c-4ea7-a5fe-328077234647.png">

After etching, remove the photoresist and gate terminal looks like,
	
<img width="275" alt="image" src="https://user-images.githubusercontent.com/123488595/215259091-8329e9a5-ca5a-4d99-a526-0cb5255fd797.png">
	
### Lightly doped drain (LDD) formation
#### 5) LDD formation
Here, we actully want P+,P-,N doping profile in the PMOS and N+,N-,P doping profile for NMOS. Reason for that is
	
<ul>
	<li><a>Hot electron effect</a></li>
	</ul>
<ul>
	<li><a>short channel effect</a></li>
	</ul>

For the formation of LDD, we again do ion implantation in P-well by using mask 7 and here we use phosphoros as a ion for light doping.
	
<img width="269" alt="image" src="https://user-images.githubusercontent.com/123488595/215259658-bad6e8a9-1376-44af-bbfc-cc76869c0af8.png">

Same process we will repeat for N-well. there we use mask 8 and BOron Ion.

<img width="272" alt="image" src="https://user-images.githubusercontent.com/123488595/215259728-61c72b29-5e80-4347-97fb-8dde3eedd8f6.png">

Now, by creating the spacers, we can protect the actual structre remain constant of P-implantt and N-implant. For that we deposite a thick Sio2 or Si3N4 layer over the gate tereminal.
	
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215259837-99dbafd7-e916-40dd-8d05-7abe5f504343.png">

Now, we do Plasma anisotropic etching. By that side-wall spacers are formed.
		
<img width="268" alt="image" src="https://user-images.githubusercontent.com/123488595/215259914-013b5c37-c10e-46ea-b03f-a5ffd13ace00.png">

### Source and drain formation
#### 6)source-drain formation
Next step is deposite the very thin screen oxide layer to avoid the effect of channeling.
		
<img width="338" alt="image" src="https://user-images.githubusercontent.com/123488595/215260114-ee1fc803-7715-4ab3-b8b7-040b3c46ccae.png">

Now to form the drain and source, again we do the ion implantation of arsenic at 75kev to create the N+ implant by using mask 9 in the P-well to form PMOS.

<img width="265" alt="image" src="https://user-images.githubusercontent.com/123488595/215260225-0e1cc276-d23f-42dc-aef7-d4826d8bd4d4.png">

Same process we will repeat for NMOS by using the mask 10 and boron ion in the N-well at 50kev to creat P- implant.
		
<img width="269" alt="image" src="https://user-images.githubusercontent.com/123488595/215260301-53191988-fef9-472a-85df-5451bc172acf.png">

Now we put this Half made CMOS into the high temparature (1000 degree)anneling. So P+ implant and N+ implant now become the source and drain.
	
<img width="308" alt="image" src="https://user-images.githubusercontent.com/123488595/215260433-d13d9436-2a5d-48b9-857f-53e96fd15884.png">
		
### Local interconnect formation
#### 7)steps tp form contacts and local interconnects
First step is remove the thin screen oxide layer by etching. Then deposite the titanium (Ti) using sputtering. here Ti is used because Ti has very low resistivity.
		
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215260746-97163977-6efa-49d1-b72d-9023a393c1ec.png">

Next step is to create the reaction between Ti layer and source, gate, drain of CMOS. For that wafer is heated at about 650-700 degree temparature in N2 ambient for about 60 seconds. and after reaction, we can see the titanium siliside over the wafer. One more reaction is heppend there between Ti and N. and it results the TIN which is used for local communication.
		
<img width="271" alt="image" src="https://user-images.githubusercontent.com/123488595/215260927-f4b9c620-8ea0-4c25-b12e-dfc860054a42.png">

Now by using mask 11 and photoresist, we will etched out the TIN and make perticular contacts. TIN is etched out by using RCA cleaning.
		
<img width="275" alt="image" src="https://user-images.githubusercontent.com/123488595/215261246-d5f30607-f0f0-4b37-ac09-c3dee2131026.png">
		
Now, local interconnects are formed after etching and removing the photoresist.

<img width="270" alt="image" src="https://user-images.githubusercontent.com/123488595/215261365-24051f17-5b93-4279-8906-089d0cc87eff.png">

### Higher level metal formation
#### 8)Higher level metal formation
These steps are very semilar like previous steps. First thing that we are noticing is that the surface is non planner. it is not good to use this type of non planner serface for matel interconnects because of the problems regarding the metal disconinuty. so, we have to plannerize the surface by depositing the thick layer of sio2 with some impurity to make less resistive layer. and then we used CMP (chemical mechanical polishing) technique to plannerise the surface.
		
<img width="269" alt="image" src="https://user-images.githubusercontent.com/123488595/215261636-404703fe-d816-4e15-b7ca-a26734eb3a80.png">

Now using mask 12 and photorsist we etched the sio2 layer to diposite the metal in it.

<img width="269" alt="image" src="https://user-images.githubusercontent.com/123488595/215261687-a3dd767d-f098-40ae-ba77-d0478d62b5ae.png">

Now remove the photoresist and seposite the thin later of TIN (~10nm) over the wafer. Because TiN is act as very good adession layer for sio2 and also act as a barrier between bottom layer and top layer of metal interconnects.
		
<img width="289" alt="image" src="https://user-images.githubusercontent.com/123488595/215261728-34ef24e2-b118-4847-9340-7fb18f4fe351.png">

Next step is to deposite the blanket tungsten (W) layer over the wafer. and then do the CMP here to plannerize the surface.
		
<img width="272" alt="image" src="https://user-images.githubusercontent.com/123488595/215261916-fd979354-1f0a-401a-bb2e-caa1bc47500c.png">

This W is act as a contact holes and this holes needs to connect to the Higher metal layer. so we will deposite the Al (aluminium) layer.
		
<img width="266" alt="image" src="https://user-images.githubusercontent.com/123488595/215262003-537891e9-3d54-4ba9-a487-b28de6edfe54.png">

Then by using the mask 13 and photoresist, we etched the W layer out to form the contact at perticular place by Plasma etching.
	
<img width="268" alt="image" src="https://user-images.githubusercontent.com/123488595/215262045-e150bfd6-485b-44f4-af41-c5a7d49e841a.png">

This is our first level of metal interconnets. now we again do the same process as above to deposite the second level of metal interconnect by using mask 14 for etched out the sio2 and using mask 15 for etched out Al leyer.
		
<img width="272" alt="image" src="https://user-images.githubusercontent.com/123488595/215262203-012bbb28-51fb-43d9-bbab-b087100bfd8a.png">

The upper layer of Al is bit thicker as compared to lower layer of Al.Now, again deposite the layer of sio2 or si3N4 to protect the chip.
		
<img width="270" alt="image" src="https://user-images.githubusercontent.com/123488595/215262274-0e3b4b18-ac8d-4aae-82f7-6ae344a2640c.png">

And finally our CMOS is looks like this after the fabrication.

<img width="266" alt="image" src="https://user-images.githubusercontent.com/123488595/215262341-e4f57e70-6df2-4757-aab5-4e14f133e49a.png">

### Lab introduction to Sky130 basic layer layout and LEF using inverter

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215262772-4fcdab39-5497-4185-92b2-7144b421d6b4.png">

In sky130, every color is showing the different layer. here the firsst layer is for local interconnect shown by blue_perpel color, then second layer is metal 1 which is showm by light perple color, and the metal 2 is shown by pink color. N-well is showm by solide das line. green is N-diffusion region. and red is for polysilicon gate. similarly the brown color is for P-diffusion.

In tckon window, we can see that the selected area is NMOS and similarly we can chech PMOS also. and that is how we can check that the CMOS is working or not.
		
<img width="764" alt="image" src="https://user-images.githubusercontent.com/123488595/215263215-3a35b10f-554f-4020-b846-c93fed6c1cda.png">

semilarly we check for the output terminal also.(by double pressing "S" to select the entire thing at output Y).
		
<img width="748" alt="image" src="https://user-images.githubusercontent.com/123488595/215263325-47dbad70-c8cf-4ddf-962b-5e4a905664bd.png">

so, we can see that "Y" is attached to locali in cell def sky130_inv.
		
we can check the source of the PMOS is connected to the ground or not. and similarly we can check it for NMOS also.
		
### Lab steps to create std cell layout and extract spice netlist
To extract the file from here, we have to write the command in tckon window. and the comand is "extract all".		

<img width="743" alt="image" src="https://user-images.githubusercontent.com/123488595/215264350-e5e03e8f-0168-4bb3-b580-c555cecf055c.png">

Now let's go to this location from the terminal. it is exctracted.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215264618-abbb005e-3ec0-4683-b516-805e8e7a449b.png">

we will use this .ext file to create the spice file to be use with our ngspice tool. for that we have apply the comand "ext2spice cthresh 0 rthresh 0". this will not create anything new. now again we have to type "ext2spice" comand in tckon window.
		
<img width="734" alt="image" src="https://user-images.githubusercontent.com/123488595/215264840-97d36274-83b5-4bf3-a754-536d77a2007e.png">

so, now we are checking the location and at there spice file has been created.
	
<img width="956" alt="image" src="https://user-images.githubusercontent.com/123488595/215264949-420903c9-4fca-4252-86cb-3c1d7d0f8fbc.png">

let's see what inside the spice file by "vim sky130_inv.spice".
		
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/215265072-d4082e87-77b5-48a9-a793-ee4a38dcc0ce.png">

		
## <h3 id="header-3_3"> Sky130 Tech File labs</h3>
### Lab steps to create final SPICE dexk using sky130 tech.
<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/215265072-d4082e87-77b5-48a9-a793-ee4a38dcc0ce.png">

here, we can see the all details about the connectivity of the NMOS and PMOS and about the power supply also.
	
X0 is NMOS and X1 is PMOS and both's connectivity is shown as GATE DRAIN SUBSTATE SOURCE.
	
But here the scale is 10000 um. but in Magic simulation, it is 0.01.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215268967-363bd9e8-e5da-4396-8ec6-a36be6147f78.png">

SO, we are going to change the dimension here in the terminal. so any measurement will be in this scale of 0.01u. i.e., width=37*0.01u.

Now we have to include the PMOS and NMOS lib files. it is inside the libs folder in the vsdstdcellsdesign folder.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215269392-1b80914a-1bdf-483f-ae69-9c381de8c29f.png">

so, now we include this file in the terminal by ".include ./libs/pshort.lib" and ".include ./libs/nshort.lib" comand.

And then set the supply voltage "VDD" to 3.3v by "VDD VPWR 0 3.3V" comand. and similarly set the value of VSS also.
	
Now, we need to specify the input files. by Va A VGND PULSE(0V 3.3V 0 0.1ns 2ns 4ns).
	
Also add the comand for the analysis like, ".tran 1n 20n", ".control" , "run",".endc",".end". 
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215270177-88efcd7c-35fe-48e0-a828-407c14a41738.png">

after running this file we get output of ngspice like this,

<img width="956" alt="image" src="https://user-images.githubusercontent.com/123488595/215275895-42cddd25-47ef-4f70-9269-8838b89907be.png">
	
Now, ploting the graph here by comand, "plot y vs time a".

<img width="884" alt="image" src="https://user-images.githubusercontent.com/123488595/215276125-9e997b7d-4098-42b0-9b8d-f414b4dfe69b.png">


### Lab steps to characterize inverter using sky130 model file

Here, we have to find value of 4 parameters.
	
<ul>
	<li><a> rise time</a></li>
	</ul>
	
it is time taken to the output waveform to 20% value to 80% value.
	
<img width="190" alt="image" src="https://user-images.githubusercontent.com/123488595/215278187-4670e664-58cc-4a63-850b-1bddaa8e9fc6.png">
	
so, rise time= (4.00742-2.51611)e-09 = 1.4913 nsec.
	
<ul>
	<li><a> fall time </a></li>
	</ul>
	
it is the time take by output for transition from 80% to 20%.

<img width="223" alt="image" src="https://user-images.githubusercontent.com/123488595/215278356-9eba1539-ff01-48f3-9b0f-8529b1fb786f.png">

so, fall time=0.08745 nsec.
	
<ul>
	<li><a> propogation delay</a></li>
	</ul>
	
it is the time difference between the 50% of input and 50% of  the output.
	
<img width="189" alt="image" src="https://user-images.githubusercontent.com/123488595/215278859-a8536576-f6db-4bda-b0f2-59202e055e84.png">

so, propogation delay = 0.60185 nsec.
	
<ul>
	<li><a> cell fall delay</a></li>
	</ul>
	
it is time for output falling to 50% and input is rising to 50%.

<img width="211" alt="image" src="https://user-images.githubusercontent.com/123488595/215278771-bfc7aec3-c414-460e-aad6-39748a880c2a.png">

so, cell fall delay= 0.0778 nsec.


# <h4 id="header-4">Day 4 -Pre-layout timing analysis and importance of good clock tree</h4>
## <h4 id="header-4_1">Timing modelling using delay tables</h4>
### Lab steps to convert grid info to track info
Till now we have done synthesis, floorplaning, placement and how to extract the spice out of it, done charecterization of it. till now, for placement and routing, we doesn't required any information about input/output port,logic path, power, ground.

Now the concept of 'lef' file will comes into the picture. it contains all the information which we discuss earlier. so our next objective is to extract the '.lef' file out of the '.mag' file. and next step is that extracted file could be placed into the picorv32a flow.
	
you need to folow certain guideline while making standerd cells.the guidelines are
<ul>
	<li><a>The input and output port must lie on the intersection of the verticle and horizontal tracks</a></li>
	</ul>
<ul>
	<li><a>The width of the satnderd cell should be odd multiple of the track pitch and the height should be odd multiple of track verticle pitch</a></li>
	</ul>

Now oprning the track file from the pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info.
where we get the info like this,

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215284798-89aa7af7-2532-46d9-a80d-9b71c78414ae.png">

so, the track is basically nothing but it is used during the routing stage.Rout will be go over the tracks. tracks are basically trases of matel layers.i.e., metal 1, matel 2, etc.
	
PNR is a automated. so we need to specified, where from we want routs to go. this specification is given by tracks. here we can see that each of the tracks are placed at (0.23 0.46)um horizontaly and (0.17 0.34)um vertically for li1, metal 1, metal 2 layer.

In layout, we can see that the ports are on the li1 layer. so we need to insure that the ports are on the intersection of the tracks or not. For that we have to cinvert the grid into the tracks. for that we have to first into the tracks file and the open the tckon window and type the "help grid" comand.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215285560-116d3c99-86ba-46a3-a1ff-c13e06ee3ae4.png">

Then again write comand according to the track file.
	
<img width="742" alt="image" src="https://user-images.githubusercontent.com/123488595/215285853-08bb5f0f-9960-4ffd-ba97-bf922ee3613b.png">

Let's see the changes in the layout.

<img width="742" alt="image" src="https://user-images.githubusercontent.com/123488595/215286000-cb0db0db-6b30-44d0-b3b3-d905606f4d9a.png">

Here we can see that, as per the guideline the ports are placed at the intersection of the tracks.

now, between the boundaries, there is 3 boxes are coverd. so our second requirment also satisfied here.
	
### Lab steps to convert magic layout to std cell LEF
here already the port name and the port definationa is seted, if not seted the we have to set the definanation and the name of the port.

As it parameters are set, we are ready to extract the 'LEF' file from the "mag" file. but before the extraction, let give the name to the cell by using tckon window.

Now we can see this file in the vsdstdcellsdesign folder.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215286894-1e9a72cf-e7e8-41f4-9693-32868d3271fa.png">

Now, we open this file in the magic by using comand "magic -T sky130A.tech sky130_vsdinv.mag &".

Now to extract the lef file we have to write the comand in the tckon window "lef write". so it will create a lef file and we can check it in the vsdstdcellsdesign folder.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215287179-76fc5147-1654-42e0-ad92-88deb947ffec.png">

Now, lef file is created and now next step is plug this lef file in picorv32a. before that we move our files to src folder where all the design files are available at one location.

for that we are copiying this file in the src folder by 'cp' comand.

### Introduction to timing libs and steps to include new cell synthesis
The basic idea is that we have to include our custom cell into openlane flow. and the first stage in the openlane is the synthesis.

here, also we required library. so we copiying the library to the src folder. 

Before we do anything, we have to modified our congig.tcl file. so opening this file by "vim" comand and modifiying it.

Now, start the new terminal and open the openlane by docker, make flow interactive and then add the package and then prep design with the privios run by the comand " prep -design pecorv32a -tag [last running time i.e.27-01_17-53] -overwrite".

Now comes the deciding part. we have to see that the synthesis run and its maps our custom vsd inveter into this flow. so, run the synthesis.
	
### Introduction to delay tables
#### Power Aware CTS
If we make enable pin at logic '1' in the AND gate, the clock will propogatee to the AND gate. similarly, if we make enable pin at logic '0' in the OR gate, here also clock propogate to the OR gate.
	
Similarly if we make enable pin at logic 'o' in the AND gate, gate will block the clock and same this will happend with the OR gate if we make enable pin at logic '1'.

The advantage of this scenario is that, during this time period of the blocking the clocks, we can save lots of power in the clock tree. now the question is that how can we use this scenario in the clock tree.

let say we have a clocktree like this given below,

<img width="128" alt="image" src="https://user-images.githubusercontent.com/123488595/215290651-09dfee39-4fd4-44eb-bba7-94f8e01d1615.png">

Here we spitted the load of the 4 FF into the 2 buffers and the load of the 2 buffers is given to the level 1 buffer. here assumptions are,

<ul>
	<li><a>Assume c1=c2=c3=c4=25fF</a></li>
	</ul>
<ul>
	<li><a>Assume Cbuf1=Cbuf2=30fF</a></li>
	</ul>
<ul>
	<li><a>Total Cap at node 'A'=> 60fF</a></li>
	</ul>
<ul>
	<li><a>Total Cap at node 'B'=> 50fF</a></li>
	</ul>
<ul>
	<li><a>Total Cap at node 'C'=> 50fF</a></li>
	</ul>

We have done some observations here,
<ul>
	<li><a>2 levels of buffering</a></li>
	</ul>
<ul>
	<li><a>At every level,each node driving same load</a></li>
	</ul>
<ul>
	<li><a>Identical buffer at same level</a></li>
	</ul>

so, here capacitance at the every node of the clock tree is not the same. it is varying. Now load is varying then input transition is also verying because the output load at the level 1 buffer is the input of the other buffers of level 2. so, we have variety of the delays. To capture it, we have delay tables.
	
##### How delay tables are prepared?
To prepare the delay table, the perticukar element is taken out of the circuit and saparetly verying the input transition and output load and according to the variation, we will charactorize the delay of the element and make the delay table frpm it.
	
<img width="190" alt="image" src="https://user-images.githubusercontent.com/123488595/215291334-623eca51-58af-4bd6-9995-55b5acfd48ac.png">

### Delay table usage part 1
Let's looks into the sample examples and making the table for Cbuf'1' and Cbuf'2',

<img width="396" alt="image" src="https://user-images.githubusercontent.com/123488595/215291481-78208007-1c64-4f2a-ad34-72c9003ad78a.png">
	
let's take practical example on the circuit. we take 40psec as input transition on the level 1 buffer and as per assumption load is around 60fF. and delay is comes between x9 and x10. lets take x9' as the delay of the buffer of level 1. 
	
### Delay table usage part 2
Next step is to calculate the delay of the buffers of level 2. And after that we can find the letancy at the 4 clock end points.

now input transition is common for  both the buffers. now assuming that the transition is around the 60psec and load at both the buffers is 50fF. so it will give the delay of y15.
	
The total delay from input to the output is= x9' + y15.(here we are ignoring the delay of the wires). that means the skew at the any output point is zero.
	
If load is not same at the every nodes, the skew will not be the zero.

### Lab steps to configure synthesis settings to fix slack and include vsdinv
After synthesis, we observed that the slack is nagative here. wns(worst negative slack)= -24.89 and tns(total negative slack)= -759.

<img width="991" alt="image" src="https://user-images.githubusercontent.com/123488595/215292549-b599c5c8-7988-4547-8f22-dacabadf61ae.png">

let's do some modification here. for that opening the READme file from the /openlane/configuration/ less READme.md

Now lets try to make balance between area and the delay of the synthesis by changing the stratagy. comand for checking the current strategy is "echo $::env(SYNTH_STRATEGY)", and comand for changing the stategy is "set ::env(SYMTH_STRATEGY) 1". by doing this area will increase the little but but timing will improve.

Then checking the synth_bufferung and synth_sizing. if any one them is off then make it on by set the value of it by 1. 
	
Till here we not get slack 0. to make slack 0, we ahve to write comand "set ::env(SYMTH_STRATEGY) DELAY 0"

After running synthesis we will get improved timing.

<img width="982" alt="image" src="https://user-images.githubusercontent.com/123488595/215346651-5570c020-0f7a-422f-b893-0607e6ae2cb1.png">
	
Next command for run is :
	
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

Then again run the synthesis.
	

## <h4 id="header-4_2">Timing analysis with ideal clocks using openSTA</h4>
### Setup timing analysis and introduction to flip-flop setup time
#### Timing analysis (with ideal clock)
we start with taking the ideal clock and we will do timing analysis for the ideal clock first.

Let's start the setup analysis with the ideal clock(single clock).
specifications of the clock is
<ul>
	<li><a>clock frequency =1 GHz</a></li>
	</ul>
<ul>
	<li><a>clock period =1 nsec</a></li>
	</ul>
Let's take lainch Flop and Capture flop with clock. here clock tree is not built yet. so it is ideal scenario. here we have to do analysis between '0' and 'T'. with that assume that the delay of logic is 'θ'.

<img width="350" alt="image" src="https://user-images.githubusercontent.com/123488595/215295611-3da466f1-bca4-4917-8303-8e82b0d9dd3f.png">

Setup timing analysis says that θ<T. this condition should be neccessory for the the comninational logic work. 
				     
Now let's introduce the practical scenario here. Opening the capture flop and it has two mux inside it.

<img width="125" alt="image" src="https://user-images.githubusercontent.com/123488595/215295701-4ffad35f-5931-4314-8f22-a369bb6db045.png">

The way flop work, it will shown by the timing graph like this,

<img width="86" alt="image" src="https://user-images.githubusercontent.com/123488595/215295736-7c8253e6-7bdd-4d24-bac0-0fc84e82bb8a.png">

So, here mux 1 and mux 2 both have their own delay. these delay will restrict the combination delay to the requirment.

Hence finite time 's' required before clk edge for 'D' to reach Qm.
	
So, we can write that the internal delay of the MUX1 = set up time(S).

So, now θ<T becomes θ<(T-S).

	
### Introduction to clock jitter and uncertainty
Let's bring one more practical scenario here. clock is taking from the some clock source or PLL. So, because of some delay from the practical source of clock or PLL, clock pulse will not comes exacly at t=0 or at t=T. that in built variation of the clock is called jitter.
				     
<img width="327" alt="image" src="https://user-images.githubusercontent.com/123488595/215296132-a05d4f02-f607-43b3-9fb1-9e02eba33b09.png">

lets consider this uncertantity time(US) in consideration. So, now equation becomes like, θ<(T-S-US). Now assuming that 'S'=0.01ns and 'US'=0.09ns. by taking that, lets identify the timing path for our existing scenario. in our circuit stage 1 and stage 3 logic path has single clock. 

NOw, what we have to do is identify the combinational path delay for the given both logics.

<img width="380" alt="image" src="https://user-images.githubusercontent.com/123488595/215296350-9d3b9c46-295e-46e8-8c57-ddebf3e64404.png">

<img width="352" alt="image" src="https://user-images.githubusercontent.com/123488595/215296376-d5be8cb2-15c8-41e5-8781-d094a49d0469.png">

### Lab steps to configure OpenSTA for post-synth timing analysis.
when we do CTS, CTS is a stage where, we add clock buffers along with clockpath and build the clock tree. so, actually we are changing the netlist. Along the running CTS, actually the netlist file also created. so, after running the CTS, we will see the new Verilog file here.
	
Now, we see what is in the my_base.sdc file.
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215353397-025edb62-8107-4bd8-801b-ae66fb8fc3a1.png">

here, we can see the capacitor load and clock period and clock port etc.

Now, to reduce the fanout we use the command is : "set ::env(SYNTH_MAX_FANOUT) 4" and then run the synthesis.
	
but here also slack doesn't reduce.
	
Now next step is run floorplan, place IO, do global placement or detail placement and genrate pdn file the run the cts by following comands.

	
inut_floorplan
	
place_io
	
global_placement_or
	
detailed_placement

tap_decap_or

detailed_placement

After running the floorplaning and done the global placement we get positive slack.
	
<img width="950" alt="image" src="https://user-images.githubusercontent.com/123488595/215347390-7b57c0f6-c4de-4800-89b0-9097c8947224.png">

Then check the file which is created. Go to the placements folder under results and then invoke the magic tool and load the def file. The command is:magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &	
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215357227-2d080ece-da04-4b71-85d5-7c29f88305e9.png">

	
## <h4 id="header-4_3">Clock tree synthesis TritonCTS and signal integrity</h4>
### Clock tree routing and buffering uisng H-Tree algorithm
#### what is clock tree synthesis?
As shown in below, figure, let's connect clk1 to FF1 & FF2 of stage 1 and FF1 of stage 3 and FF2 of stage 4 with out any rules.

<img width="280" alt="image" src="https://user-images.githubusercontent.com/123488595/215308549-adf17411-a251-4dbe-96ac-aa1ce94a2a22.png">

Now, let's see the problem with this clock Rout. let us say time required to reach the FF1 and FF2 of stage 1 are t1 and t2 for clock1. so, we can say that t2-t1= skew (ideally skew=o).

<img width="216" alt="image" src="https://user-images.githubusercontent.com/123488595/215308715-2e81309f-9ab4-477e-afff-7648d5cf0fc0.png">

To make, skew to be 0, this rout definatly not help. so, we can say that what we built the tree is "BAD TREE". so, the concept of H-tree comes out. with the Mid point strategy, H-tree form.
	
<img width="285" alt="image" src="https://user-images.githubusercontent.com/123488595/215308879-7c239ada-073f-438f-b462-fcbca5b77c60.png">

Next thing is clock tree synthesis (buffering).as we see in the clockk tree and we observed that clock has to travel through all wires and it will charge all capacitor which are comming in the path of this wire. 
	
The problem occurse due to the charging the capacitor is signal inntigrity problem because of transition of signals. solution of this problem is add the repeaters here. the repaters are the similar as what we use in the data path but the main difference is, here repeater has equal rise and fall time.
	
<img width="283" alt="image" src="https://user-images.githubusercontent.com/123488595/215309501-836f18ab-5556-442e-b690-a5e4b8559a37.png">

### Crosstalk and clock net shielding
We build the clock tree in a manners that the skew becomes zero between launch Flop and capture flop. but if accedently any crosstalk heppense then everything what we had design is detoriated.
	
Let take the first clock net and protect it by shielding. here we protect the clock network from outside world. if the protection is not there then problems like glitch and delta delay is heppens.

The glitch is heppence because of Coupling capacitance between the wires.

<img width="203" alt="image" src="https://user-images.githubusercontent.com/123488595/215310959-a4db4ed7-040c-43e1-a65a-75271e4c50c7.png">

The shielding is the technique, by which we can protect the net from these problems. In a shielding, we put wire between the other teo wire where coupling capacitance is generate. This extra wire is grounded or connected to VDD.

Now, let's see about the delta delay.

<img width="404" alt="image" src="https://user-images.githubusercontent.com/123488595/215311259-ed24e431-674f-4573-9b11-6516296da86f.png">
	
### Lab step to run CTS using TritonCTS
Now next step is CTS. for that write the command: "run_cts".
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215360216-82a2eaa6-cc33-411a-847d-38c4d33ffd12.png">

In CTS stage the buggers are added in the paths. so, it will modifiying the netlist. so, if we go in the synthesis folfer and check the files, where cts.v file will avaiilable and this file contains the added buffers.

### Lab steps to verify CTS runs
we have run CTS.Now before we goes into the post cts flow, we need to know that the actual meaning of the "run" command. This is the proc of tcl file. so, lets see, from where, openlane take this procs. for that we need to goes into the openlane and then goes into scripts and then check the "tcl_commands". in this file tcl commands are there for every stages what we have run till now. so let's see  the "cts.tcl" file.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215361250-e3b87404-d1f8-4ed4-857c-f60ce973f253.png">

here we can see the many procs are there. so here we can see the procs are run during the CTS run.
	
<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215361426-81d0e0cf-79a6-486f-9405-ec11368fdb82.png">

so when we run the cts in the flow, basically it do this things.
	
first it run the tritonCTS by givin total information.

then it set the value of timer.

Then it goes for open the openroad.
	
if we check the openroad folder, at that directory "or.files" are available which was runs in the OPENROAD EDA tool.
	
Now les's check the "or_cts.tcl" file. inside this we can see the few switches.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215362238-03fec719-6a58-4e32-82c9-a44c1daea771.png">

Now lets check what the library does. 
For this command is "echo $::env(LIB_SYNTH_COMPLETE)" In the main flow. then again apply command :"echo $::env(LIB_TYPICAL)"
	
Now checking the clock period of this by command: "echo $::env(SYNTH_MAX_TRAN)"
	
So, clock period seted as 2.473 nsec.
	
Then checking the max cap value, by command : "echo $::env(CTS_MAX_CAP)". and it is setted as 1.53169
	
Now checking the branch buffer cells by command :"echo $::env(CTS_CLK_BUFFER_LIST)". and these are the buffer cells are listed there "sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8".
	
And last cheching the root buffer by command: "echo $::env(CTS_ROOT_BUFFER)". So, we find that this "sky130_fd_sc_hd__clkbuf_16
" buffer is root buffer.
	
<img width="296" alt="image" src="https://user-images.githubusercontent.com/123488595/215363714-42c57e54-b78e-4a33-9d3b-11c1e6b44266.png">


## <h4 id="header-4_4">Timing analysis with real clocks using openSTA</h4>
### Setup timing analysis using real clocks
With real clock, circuit looks littel bit different then ideal clock. Here the bufferes and wires are added to the circuts.
	
Here, due to buffer, clock signals are not reaching the flop at t=0. it will reach at t=0+(delay of buffer 1 and 2).Now equation change to (θ+1+2)<(T+1+3+4).

<img width="341" alt="image" src="https://user-images.githubusercontent.com/123488595/215311713-d8a06357-aad2-4a8c-bdb9-818cb5dd3bec.png">

Let's called "1+2"=∆1 and "1+3+4"=∆2 and (∆1-∆2)=skew

<img width="339" alt="image" src="https://user-images.githubusercontent.com/123488595/215311928-7d0adab3-b496-4e9a-b323-3bead79df359.png">

And here also, we have to consider the propogation skew (s) and uncertainty delay (US). so final equaltion becomes like, (θ+∆1)<(T+∆2-S-US).

we can also say that (θ+∆1)= data arrival time and (T+∆2-S-US)=data required time.																	    
If (Data required time)- (Data arrival time) = +ve then it is fine. If it is -Ve then it is called 'slack'.

#### Hold timing analysis
It is littel bit different then setup timing analysis. here we are sending the first pulse to the both launch FLop and capture flop.
									
Hold condition state that, Hold time (H)< combinational delay (θ).
So, (θ>H).

<img width="353" alt="image" src="https://user-images.githubusercontent.com/123488595/215312745-7f7f2c59-944c-4b32-aec7-89353379ad52.png">

	
Hence, finite time 'H' required for 'Qm' to reach Q i.e., internal delay of mux2= hold time.
	
Now, if we add the real time clock, the equation will be change. now equation becomes (θ+∆1)>(H+∆2).	
	
<img width="346" alt="image" src="https://user-images.githubusercontent.com/123488595/215312870-071553e5-9a03-48b4-90b0-c87dc1b5e26b.png">

### Hold time analysis with real clock
Now here also adding the Unsetainty delay(UH) value due to jitter. so, equation becomes like, (θ+∆1)>(H+∆2+UH).

we can also say that (θ+∆1)= data arrival time and (H+∆2+UH)=data required time.

If (Data arrival time) -(Data required time)= +ve then it is fine. If it is -Ve then it is called 'slack'.

Now, applying all these things in out network.

<img width="383" alt="image" src="https://user-images.githubusercontent.com/123488595/215313289-53a5035e-19fc-468b-9575-45ad60b0055c.png">

### Lab steps to analyze timing with real clock using OpenSTA
let's open the OPENROAD tool in the flow by "openroad" command.

our objective is to do analysis of the clock tree.
	
we are analysin this in the OpenROAD because OpenSTA is already built in the OpenROAD. In OpenROAD the timing analysis is done in a different way. first we have to create "db" and "db" is created in a "lef" and "def" file.
	
Now let's create the DB. To create the DB, first we have to read the lrf file by comand "% read_lef /openLANE_flow/designs/picorv32a/runs/29-01_22-23/tmp/merged.lef".
	
Then we read the "def" file by command: "read_def /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/cts/picorv32a.cts.def".
	
NOw to create the DB  write the command "write_db pico_cts.db"
	
NOW read this db file by command "read_db pico_cts.db"
	
then read the verilog file by applying the command "read_verilog /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/synthesis/picorv32a.synthesis_cts.v"
	
then read the library (max) by this command:"read_liberty -max $::env(LIB_FASTEST)".
	
similarly read the library (min) by this command: "read_liberty -min $::env(LIB_SLOWEST)".
	
Now read the sdc file by this command: "read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc"
	
now set the clocks by this command:"set_propagated_clock [all_clocks]"
	
there reports the checks by this command: "report_checks -path_delay min_max -format full_clock_expanded -digits 4".
	
so after running this we can see that the slack is positive for hold and setup both. and also we can notice the data required time and data arroval time also.

So, the Hold slack = 1.6982nsec because here we can see that (arrivel time) >(required time).
	
<img width="236" alt="image" src="https://user-images.githubusercontent.com/123488595/215372600-9a310dbe-1b7e-477a-ab17-32864a69d35c.png">

NOw setup slack = 0.9457nsec because here we can see that (required time)>(arrival time).

### Lab steps to execute openSTA with right timing libraries and CTS assignment
TritonCTS is right now built according to optimize fully according to one corner and we had bulid the clock tree for typical corner. and library also min and max. so we made tree according to typical corner but we analize it according to one corner. so, analysis become incorrect.
	
so, first we exits from the openroad by using "exit" command and we have to include the typical library for typical analysis. for that we have to open the "openroad" again and add this typical library. now we don't need to add lef and def file here. now commands for the adding a file is:
	
read_db pico_cts.db
	
read_verilog /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/synthesis/picorv32a.synthesis_cts.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clocks]

report_checks -path_delay min_max -format full_clock_expanded -digits 4

slack for typical coirner= 0.2429nsec
	
<img width="235" alt="image" src="https://user-images.githubusercontent.com/123488595/215376738-807c2aa9-5b7a-4e6d-b830-82b6b8f72820.png">

Now checking the branch buffer cells by command :"echo $::env(CTS_CLK_BUFFER_LIST)". and these are the buffer cells are listed there "sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8".

when openlane are making the CTS, at that time this buffers are place in the clock path to meet the skew value. and we always want skew value is maximum to the 10% of clock period.
	

# <h5 id="header-5">Day 5 -Final steps to build power distribution network</h5>	 
## <h5 id="header-5_1"> Routing and design rule check(DRC)</h5>
### Introduction to Maze Routing A lee As Algorithm
Next stage in the physical design is the routing and DRC stage.

Routing. by the name it is says that rout means make physical contact between Din1 and FF1 od stage 1. but algorithm wise,it understand that Din1 is the source and FF1 input pin is the target. so, algorthm has to find the best possible solution to connect the Din1 and FF1.
	
For that we use Maze Routing and Lee's algorithm. let's try to connect Block 1 and block 2 of stage 3. there are varies mathod to connect these blocks but we need best solution for the rout or connection. To understand that, we remove all other things.

Algorithm create the routing gird at backend. here algirithm create two points. one is source and other is target. Now by using this routing grid, algorithm find the best way of routing. algorithm marks the adgecent grids of source. similarly again it will find the adgecent grids of the previos grids.similarly this process will runs continuosly.
	
<img width="239" alt="image" src="https://user-images.githubusercontent.com/123488595/215314333-b2d30c66-2fa7-4e23-9608-55ffa5bbf4e7.png">

### lee's algorithm conclusion. 
The extanding process of adgecent grids are contionuous till it reaches to the target.

<img width="239" alt="image" src="https://user-images.githubusercontent.com/123488595/215317267-66bb89a2-e20a-4f02-a019-e1612a153412.png">

there is many ways to reach the target. but the best possible way is 'L' shaped way.

<img width="239" alt="image" src="https://user-images.githubusercontent.com/123488595/215317372-d2448ec7-f0b9-4939-b459-220f03e5745a.png">
	
lets take another example for this routing.

<img width="238" alt="image" src="https://user-images.githubusercontent.com/123488595/215317631-50b1dfc1-a267-40ff-999f-887cc868379d.png">
	
Now, we reouted all the blocks and the circuits looks like this,
	
<img width="280" alt="image" src="https://user-images.githubusercontent.com/123488595/215317685-bb12a004-5a9c-410e-a424-c8e3b4fc9414.png">

### DRC(Design Rule check)
While doing the routing, we need to follow certain rules for that. this is called DRC cleaning.
	
Lets take two parallel wires from the circuit for example,
	
<img width="373" alt="image" src="https://user-images.githubusercontent.com/123488595/215318203-aa583ad2-17f3-419c-a7ca-3e65d8b824ff.png">

<ul>
	<li><a> Rule 1</a></li>
	</ul>
Width of the wire should be minimum that derived from the optical wavelenth of lithography technique applied.

<img width="119" alt="image" src="https://user-images.githubusercontent.com/123488595/215317905-6602a405-0d88-4693-b477-fec5e7e0b9a4.png">

<ul>
	<li><a> Rule 2</a></li>
	</ul>
The minimum pitch between two wire shold be this much.
	
<img width="117" alt="image" src="https://user-images.githubusercontent.com/123488595/215318038-4d485e52-1473-4358-a8bf-a3db86193600.png">

<ul>
	<li><a> Rule 3</a></li>
	</ul>
The wire spacing between two wires should be this much.
	
<img width="121" alt="image" src="https://user-images.githubusercontent.com/123488595/215318111-43d2caf1-bbe4-4d78-b130-90ddc2904b1b.png">
	
Let's take other part for understand the rules. basic problem in this types of wire is the signal short.

<img width="404" alt="image" src="https://user-images.githubusercontent.com/123488595/215318308-aef969c3-aef1-4fd1-9108-a0ba7c41f922.png">

Solution of this signal short problem is take one of the wire and put it on the other metal layer. usually upper metal is wider than the lower metal.

<img width="109" alt="image" src="https://user-images.githubusercontent.com/123488595/215318382-22d157c0-5888-43f2-b767-199b44c38de0.png">

After this solution, we add two new DRC rules should be check.
<ul>
	<li><a> Rule 1</a></li>
	</ul>
via width should be some minimum value.
	
<img width="110" alt="image" src="https://user-images.githubusercontent.com/123488595/215318514-2c807302-a6ea-4848-8576-445b306d9b08.png">

<ul>
	<li><a> Rule 2</a></li>
	</ul>
Via spacing should be minimum this.
<img width="112" alt="image" src="https://user-images.githubusercontent.com/123488595/215318529-efa1b973-4162-4e39-842c-fc54ae08b239.png">

Next step is paracitic Extraction. so, the wire will get some resistance and capacitance value.

<img width="283" alt="image" src="https://user-images.githubusercontent.com/123488595/215318616-92c611e8-d906-48ae-ac19-839680aeaba8.png">

## <h5 id="header-5_2"> Power distribution network and routing</h5>
### Leb steps to build power distribution network

IS in case, our terminal is delected by some cause, the if we want the previos terminal once again then this steps should be followed:
<ul>
	<li><a>docker</a></li>
	</ul>
<ul>
	<li><a>./flow.tcl -interactive</a></li>
	</ul>
<ul>
	<li><a>package require openlane 0.9</a></li>
	</ul>
<ul>
	<li><a>prep -design picorv32a -tag [run file name i.e., 20-01_22-23]</a></li>
	</ul>

Now to check the which was the last stage we perorm, the command is:"echo $::env(CURRENT_DEF)".

So, till now we have done CTS and now we are going to do the routing. but before routing we have to generate the PDN(power distribution network)file.
for that command is: "gen_pdn".
	
<img width="993" alt="image" src="https://user-images.githubusercontent.com/123488595/215484546-93b6e790-fd1b-420b-9018-e22419543426.png">

Here we can see the total number of nodes on the net VGND and it is also says that Grid matrix is created successfully. here total connection between all PDN nodes establish in the net VGND.
	
Now, till here we have picorv32a chip, and it needs the power. so it will get power from VDD and GND pads. From the pads, power goes to the tracks and from the tracks, the cells get power.

### Lab steps from power straps to standerd cell power
To understan this, take an example here,

<img width="340" alt="image" src="https://user-images.githubusercontent.com/123488595/215486262-11b4bef8-ec09-4ba0-a6b7-61e30305594d.png">

In this figure, the green box is available is let say picorv32a chip. And the yellow, red and blue boxes which are the shown on the outside of the frame are the I/O pins and the power and ground pads. in this pads, the corner ones are called corner pads. 
	
Red pads are the power pads and Blue pads are ground pads.
	
Power is transfered to the rings from the pads through Via which is shown by black dots on the cross section points of the ring and pads.

Now we need to insure that the power is transfered from the ring to the chip. for that we have vertiocal and horizontal tracks which are also shown by the red and blue color.
	
NOw, we need to supply power to the standerd cell (Which are shown by rectangular white boxes) from these tracks. this is done by horizontal small connections shown in the figure.

So, in a lab we have done this all the things till power distribution.
	
Now next and the final step is routing.
	
### Basic and Global detailed routing and configure TritonRoute
Now current def.file is change to pdn.def from the cts.def. pdn.def file is now in the "runs/29-01_22-23/tem/floorplan/pdn.def".

In the routing process we are focusing on the routing strategy. there are 5 routing strategies are there. 0,1,2,3 and 14. routing is done in the TritonRoute engine. we have to specifies the strategy for the routing. for example if we ser the strategy to 14 then "TritonRoute14" strategy is used. 

If we set the TritonRouting strategy to "0" then it want converge to a 0 TRC routing. but because of this we will improve in the memory requirement and run time. if we use TritonRoute14 then the run time will be approximate 1 hours. but in the TritonRoute0 it will be around the 30 minutes. here we use the "TritonRoute0". so in our flow first we check the routing strategy by the command "echo $::env(ROUTING_STRATEGY)". if it is "0" then it is fine, otherwise we have to change the strategy to "0".

Now the last thing remains is routing. for that command is :"run_routing".

The routing process is very complex.So, total routing is devided into two part.
	
<ul>
	<li><a>Fast route (Global route)</a></li>
	</ul>	
<ul>
	<li><a>Detailed route</a></li>
	</ul>

<img width="532" alt="image" src="https://user-images.githubusercontent.com/123488595/215496401-618892e5-51d9-44c9-a75b-2e6b7961a922.png">

In the Global route, the routing region is devided into the rectangular grids cells as shown in the figure. and it is represented as 3D routing graph. Global route is done by FAST route engine.The detailed route is done by TritonRoute engine.
	
As shown in the figure, A,B,C,D are four pins which we want to connects through routing. and this whole image of A,B,C,D show the nets.

Now, the routing is successfully done.

<img width="993" alt="image" src="https://user-images.githubusercontent.com/123488595/215498852-c81a5999-9c1e-44f6-a819-dab9e7de5874.png">


## <h5 id="header-5_3"> Tritinroute features</h5>
### TritonRoute feature 1 -Honors pre-processed route guide
<img width="378" alt="image" src="https://user-images.githubusercontent.com/123488595/215336408-7194a72e-03db-4946-bebe-f997e1e88c1e.png">

<ul>
	<li><a> It performs initial detail route.</a></li>
	</ul>
<ul>
	<li><a> It attempts as much as possible to route within route guides.</a></li>
	</ul>
requierment of processed guides are 1)should have within unit lenth 2)should be in the preferred direction.
	
<img width="320" alt="image" src="https://user-images.githubusercontent.com/123488595/215319134-a09fe612-ba7d-4289-9493-484d967a133c.png">

	
<ul>
	<li><a>Assumes route guides for each net satisfy inter-guide connectivity. </a></li>
	</ul>
If two guides are connected then 1) they are on the same metal layer with  touching edges. 2) they are on the neighbouring metal layers with a nonzero vertically overlapped area. 

<ul>
	<li><a>Assumes route guides for each net satisfy inter-guide connectivity. </a></li>
	</ul>
	
### TritonRoute feature 2 & 3 - inter-guide connectivity and intra-layer & inter -layer routing
Each unconnected terminal. i.e, poin of a standerd-cell instance should have its pin shape overlapped by a route guide.

<img width="184" alt="image" src="https://user-images.githubusercontent.com/123488595/215336322-ed026a49-58a5-4814-a913-1de1fd0a7929.png">
	
Here we can see that black dots are pins of the cells and it is overlapped by route guide. if you have pins on the intersection of the vertical and horizontal tracks that will ensure that it will be overlapped by route guides.
	
#### Intra-layer parallel and inter-layer sequential panel routing	
Intra layer means within the layers and inter layear means between the layers.	
	
<img width="389" alt="image" src="https://user-images.githubusercontent.com/123488595/215336677-367ef8c1-38a9-4657-a06c-bf7fa46bc94c.png">

In this figure we can see the 4 layers of metal. each of these layers are devided in to the "--" lines.	lets focus on metal 2 layer. here we assume the routing direction vertical. These "--" lines are called pannels. each pannels assigns the routing guides. here we can see the blue arrows. here routing is heppenes in the even index. it means that intra layer parallel routing. first it is heppenes in the even index and the it will heppen in the odd index. but it is heppening in the parallel in this perticular layer.

So, the (a) figure shows the parallel routing of panels on M2.In (b) figure, we can see the parallel routing of even panels on M3 and (c) shows the parallel routing of odd panels on M3.

### TritonbRoute method to handle connectivity	
<ul>
	<li><a>INPUTS: LEF<DEF<preprocessed route guide</a></li>
	</ul>	
<ul>
	<li><a>OUTPUTS:detailed routing solution with optimized wore-length and via count</a></li>
	</ul>	
<ul>
	<li><a>CONSTRAINTS:Route guide honouring, connectivity constraonts and design rukes</a></li>
	</ul>		
	
Now we have to defined the space where detailed routing take spaced.
	
### Handling connectivity
<img width="404" alt="image" src="https://user-images.githubusercontent.com/123488595/215340811-5271ff2f-c7d6-44a8-a646-d96ba11878b7.png">
 
To handle the connectivity, two concepts comes into the picture,
<ul>
	<li><a>Access point:An on-gride point on the metal layer of the route guide, and is used to connect to lower-layer segments, upper-layer segments, pins or IO ports.</a></li>
	</ul>	
<ul>
	<li><a>Access point cluster (APC): A union of all access points derived from same lower-layer segment,upper-layer guide, a pin or an IO port</a></li>
	</ul>	
	
Here in the figure shown above, the illustration of access points: 
	
(a)To a lower-layer segment
	
(b)To a pin shape
	
(c)To upper layer
	
### Routing topology algorithm and final files list post-route
<img width="181" alt="image" src="https://user-images.githubusercontent.com/123488595/215341969-8f89f535-1b59-40ad-bf83-94a9b29cebd8.png">

The algorithm says that for each APCs we have to find the cost associated with it and we have to do minimum spaning tree betweem the APCs and the cost. finally the conclusion of the algorithm is that we have to find the minimul and the most optimal poits between two APCs.
	
Now, remaning things is the post routing STA analysis. for that the first goal is to extract the perasetic (SPEF). This SPEF extraction is done outside the openlane because openlane does not have SPEF extraction tool.

The .spef file can be found under the routing folder under the results folder.

<img width="960" alt="image" src="https://user-images.githubusercontent.com/123488595/215507584-4da3170a-dc62-4b2b-9ed9-b2459eeb000e.png">

The following command can be used to stream in the generated GDSII file.

"run_magic"

Now the gds file will be generated and it is stored in the magic folder under results folder.

<img width="959" alt="image" src="https://user-images.githubusercontent.com/123488595/215541691-d6ff826e-ae7d-48e7-9dbc-c224d2995511.png">

And the generated layout is,

<img width="512" alt="image" src="https://user-images.githubusercontent.com/123488595/215542186-baa6a395-4728-4e11-af67-18eeb336f36b.png">

	
# <h6 id="header-6"> All commands to run the openlane flow </h6>

docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a 

set ::env(SYNTH_STRATEGY) "DELAY 0"

run_synthesis
	
init_floorplan
	
place_io
	
global_placement_or
	
detailed_placement
	
tap_decap_or
	
detailed_placement
	
run_cts
	
gen_pdn
	
run_routing
	
run_magic
	
# <h7 id="header-7">References</h7>
	
<ul>
	<li><a>Workshop Github material</a></li>
	</ul>
<ul>
	<li><a>https://github.com/google/skywater-pdk</a></li>
	</ul>
<ul>
	<li><a>https://github.com/nickson-jose/vsdstdcelldesign</a></li>
	</ul>
<ul>
	<li><a>https://sourceforge.net/projects/ngspice/</a></li>
	</ul>
<ul>
	<li><a>https://github.com/</a></li>
	</ul>
<ul>
	<li><a>https://www.vlsisystemdesign.com/wp-content/uploads/2017/07/Introduction-to-Industrial-Physical-Design-Flow.pdf</a></li>
	</ul>

# <h8 id="header-8">Acknowledgement</h8>
I would like to express my special thanks of gratitude to Mr. kunal ghosh (co.-founder of VLSIsystem design (VSD) corp.pvt.ltd.) and Mr.Nickson Jose and Mr. SUMANTO KAR (Sr. Project Technical Assistant, IIT BOMBAY) for their guidence and temendous presenting this workshop on Advance Physical Design using OpenLANE/Sky130. The Workshop was excellent and well designed. This workshop taught me a lot of new things about the physical chip design using OpenLANE software and many more.
