# <b>About the project</b>

This project gives an interactive tutorial experienced during the VSD Advanced Physical Design workshop using OpenLANE.

OpenLane is not a tool, it is basically a flow that comprises many opensource EDA tools which includes OpenRoad, Yosys, ABC, Fault, Qflow, Magic and a number of custom scripts for design exploration and optimization. Basic aim to have OpenLane is to have complete RTL to GDSII flow.

# Table of Contents
 DAY1: Inception of open-source EDA, OpenLANE and Sky130 PDK
 - Introduction to QFN-48 Package, chip, pads, core, die and IPs
 - Introduction to RISC-V
 - Introduction to all components of open-source digital asic design
 - Simplified RTL2GDS flow
 - Introduction to OpenLANE detailed ASIC design flow
 - OpenLANE Directory structure in detail
 - Lab [Day 1] - Determine Flip-flop Ratio
 - Review files after design prep and run synthesis
 - characterize synthesis results 
 
 DAY2: Good Floorplan vs Bad Floorplan and Introduction to Library Cells
 
 - Floorplanning considerations
 - Utilization Factor & Aspect Ratio
 - Pre-placed cells
 - Decoupling capacitors
 - Power Planning
 - Pin Placement
 - Floorplan run on OpenLANE & view in Magic
 - Placement
- Placement run on OpenLANE & view in Magic
- library characterization
- Timing characterization

DAY3:
 
## DAY1: Inception of open-source EDA, OpenLANE and Sky130 PDK

### Introduction to QFN-48 Package, chip, pads, core, die and IPs

- *<b>Pads</b>* surround the rectangular metal patches where external bonds are made. input,output and power pad cells are commonly used pad cells.
- *<b>Core</b>* is the section of the chip where the fundamental logic of the design is placed.
- *<b>Die</b>* contains the core, it is the small semiconductor material specimen on which the fundamental circuit is fabricated. IC's are fabricated on a single 9 inch or 12 inch diameter silicon wafer, which contains hundreds of mirror images of the fundamental logic. This wafer is then cut into small pieces, each piece has similar functionality of the fundamental logic called Die.

![chip](https://user-images.githubusercontent.com/68071764/214861155-b7655dd6-af78-4296-9bb2-2edfc54286d6.png)

- *<b>Foundry</b>* PLLs, DAC, ADC and SRAMs come under the category of foundry IP. They have to be manually designed or need some human interference (or intelligence) essentially to define and create them. IPs(Intellectual property) are targeted on a particular technology nodes. Macros are basically digital blocks which are made up of purely digital logic.

![chip_info](https://user-images.githubusercontent.com/68071764/214861171-7840ba80-8154-4654-a3ce-eda93eaf844f.png)

### Introduction to RISC-V

- *<b>RISC V</b>* is an open source specification of an Instruction Set Architecture (ISA). ISA commonly used for personal computers. The RISC-V ISA is provided under open source licenses that do not require fees to use, which provides it a huge edge over other commercially available ISAs. It is a simple, stable, small standard base ISA with extensible ISA support, that has been redefining the flexibility, scalability, extensibility, and modularity of chip designs.

- *<b>PicoRV32</b>* with the advent of an open standard ISA like RISC-V, there are many people who have created their own microprocessor cores which are documented in the RISC-V github handle found at RISC-V Cores List.PicoRV32 is one such implementation of the RISC-V ISA created by clifford wolf. PicoRV32 is a CPU core that implements the RISC-V RV32IMC Instruction Set. It can be configured as RV32E, RV32I, RV32IC, RV32IM, or RV32IMC core, and optionally contains a built-in interrupt controller.

- *<b>Qflow</b>* is a complete tool chain for synthesizing digital circuits starting from verilog source and ending in physical layout for a specific target fabrication process.The tool chain is basically a collection of steps ordered in a particular sequence that makes it easier to analyse the design as it propagates through the steps.The steps are ordered in a sequential-order i.e, they can only be executed one after the other from top to bottom.

![flow](https://user-images.githubusercontent.com/68071764/214872786-d44412f7-901a-4575-b6e9-33fe7a1ba882.png)

-  *<b> Assembly language(Bridge between software and hardware)</b>*  Applications and softwares running in our laptops and PCs are written in a variety of languages like C, C++, Python and Java etc. These different languages cannot be directly processed and implemented by the hardware (Microprocessor Core). They need to be converted into form that is understandable by the core. This is where the ISA or the instruction set architecture (assembly language) comes into the picture. Special programs called the compiler and assembler are used to convert the instructions in different languages to the target assembly language. The compiler converts the code into assembly language, which in our case is the RISC-V ISA and the assembler takes care of converting the different instructions in textual format into binary representations which are understood by the microprocessor and executed. 
 
![APP](https://user-images.githubusercontent.com/68071764/214866200-680ae242-d5c2-414d-85f9-70351ae757ec.png)

### Introduction to all components of open-source digital asic design

The design of digital Application Specific Integrated Circuit (ASIC) requires three enablers or elements - Resistor Transistor Logic Intellectual Property (RTL IPs), Electronic Design Automation (EDA) Tools and Process Design Kit (PDK) data.

![image](https://user-images.githubusercontent.com/68071764/214874172-046829ea-b8ae-49a3-9aba-d421ef319b0c.png)

- Opensource RTL Designs: github, librecores, opencores
- Opensource EDA tools: QFlow, OpenROAD, OpenLANE
- Opensource PDK data: Google Skywater130 PDK
- PDK (Process Design Kit): A set of data files and documents which serves as the interface between the designer and the fab. This includes cell libraries, IO libraries, design rules (DRC, LVS, etc.)

### Simplified RTL2GDS flow

![image](https://user-images.githubusercontent.com/68071764/214878015-de7eaed8-12fc-44d7-9a4a-3a333b24f3dc.png)

- <b>Synthesis:</b> RTL Converted to gate level netlist using standard cell libraries (SCL)
- Floor Planning/ Power Planning: Plan silicon area and create robust power distribution network. The power network usually uses the upper metal layer which are thicker than lower layer and thus lower resistance. This lowers the IR drop problem.
- <b>Placement:</b> Placing cells on floorplan rows aligned with sites
  1. Global Placement: for optimal position of cell
  2. Detailed Placement: for legal positions
- <b>Clock tree synthesis:</b> clock distribution to all flip flops and is usually a tree (H-tree, X-tree ... )
- <b>Routing:</b> Use horizontal and vertical wires to connect cells together. The router uses PDK information (thickness, pitch, width,vias) for each metal layer to do the routing. The Sky130 defines 6 routing layers. It doe global routing and detailed routing.
- <b>Signoff:</b> Involves physical verification like DRC and LVS and timing verification. Design Rule Checking or DRC ensures final layout honors all design rules and Layout versus Schematic or LVS ensures final layout matches the gate level netlist from synthesis phase. Timing verification ensures timing constraints are met.

### Introduction to OpenLANE detailed ASIC design flow

![image](https://user-images.githubusercontent.com/68071764/214881814-5692869c-bc8c-494f-a23d-86ce8006258b.png)

OpenLANE utilises a variety of opensource tools in the execution of the ASIC flow:

| TASKS | TOOLS |
|-----|-----
|RTL Synthesis & Technology Mapping|yosys, abc|
|Floorplan & PDN|init_fp, ioPlacer, pdn and tapcell|
|Placement|RePLace, Resizer, OpenPhySyn & OpenDP|
|Static Timing Analysis|OpenSTA|
|Clock Tree Synthesis	|TritonCTS|
|Routing	| FastRoute and TritonRoute|
|SPEF Extraction| SPEF-Extractor|
|DRC Checks, GDSII Streaming out|Magic, Klayout|
|LVS check|	Netgen|
|Circuit validity checker|checker|

### OpenLANE Directory structure in detail 

The openLANE file structure looks something like this:

![Screenshot 2023-01-26 at 11 48 13 AM](https://user-images.githubusercontent.com/68071764/214909475-73846c57-95bf-4c26-975c-a67c283d2d4a.png)

- <b>skywater-pdk:</b> contains PDK files provided by foundry
- <b>open_pdks:</b> contains scripts to setup pdks for opensource tools
- <b>sky130A:</b> contains sky130 pdk files

#### Lab [Day 1] - Determine Flip-flop Ratio

- The task is to find the flip-flop ratio ratio for the design picorv32a. This is the ratio of the number of flip flops to the total number of cells. For the OpenLane installation, the steps are very straight forward and can be found on the OpenLane repo.

1. <b>Invoking OpenLANE:</b> 

- Openlane can be invoked using docker command followed by opening an interactive session. flow.tcl is a script that specifies details for openLANE flow.
- Open the docker platform inside the openlane/
- flow.tcl -interactive: run script for automating the whole RTL to GDSII flow but in step by step -interactive mode
- package require openlane 0.9: retrives all dependencies for running v0.9 of OpenLANE

![Screenshot 2023-01-25 at 11 50 51 PM](https://user-images.githubusercontent.com/68071764/214909671-4920a450-4242-43e3-b7d6-b34d49e63e01.png)

![Screenshot 2023-01-26 at 11 51 09 AM](https://user-images.githubusercontent.com/68071764/214912956-4f22123b-4c33-4f5c-9aa8-391223472f25.png)


2. <b>Design Preparation Step</b>

- % prep -design picorv32a: Setup the filesystem where the OpenLANE tools can dump the outputs. This also creates a run/ folder inside the specific design directory which contains the command log files, results, and the reports dump by each tools. These folders will be empty for now except for lef files generated by this design setup stage. This merged the cell LEF files .lef and technology LEF files .tlef generating merged.nom.lef inside run/tmp/

![Screenshot 2023-01-26 at 12 25 38 PM](https://user-images.githubusercontent.com/68071764/214913686-946055f3-5717-4bc4-b01a-9a843298597c.png)

![Screenshot 2023-01-26 at 12 27 09 PM](https://user-images.githubusercontent.com/68071764/214913757-b0ced192-ddfa-47d5-927a-6f29fe0449ee.png)

- Since today's date is 26th January, a folder titled "26-01..." is created within the picorv32a directory:

![Screenshot 2023-01-26 at 12 36 45 PM](https://user-images.githubusercontent.com/68071764/214915539-4413a946-3450-4ed6-879c-9c672bef22d4.png)

- The merged file is created during the merging operation in the pircorv32a design preparation (it merges lef and techlef files). 

![Screenshot 2023-01-26 at 12 43 23 PM](https://user-images.githubusercontent.com/68071764/214915935-9d8f7fb3-0fe4-4fbb-8e5c-e8419ef2b1eb.png)

3.  <b>Run synthesis:</b>

- % run_synthesis: Run yosys RTL synthesis, ABC scripts (for technology mapping), and OpenSTA.

![Screenshot 2023-01-26 at 12 36 45 PM](https://user-images.githubusercontent.com/68071764/214914425-d8a7e682-9b96-482a-953a-12b1952c15dd.png)

![Screenshot 2023-01-26 at 1 02 46 PM](https://user-images.githubusercontent.com/68071764/214914519-dc64c3de-6a8c-4f69-9759-316ac883a23a.png)

- The yosys and ABC tools are utilised to convert RTL to gate level netlist
![Screenshot 2023-01-26 at 2 01 33 PM](https://user-images.githubusercontent.com/68071764/214917249-e8679a88-5490-49fc-92d9-d928ceb2fc0d.png)

- Calcuation of Flop Ratio:

Flop ratio = *<b>Number of D Flip flops/ Total Number of cells</b>*

  ![Screenshot 2023-01-26 at 1 29 40 PM](https://user-images.githubusercontent.com/68071764/214917782-cf4db7a7-a658-4adc-8ab0-c70842b5a900.png)
       
![Screenshot 2023-01-26 at 1 32 06 PM](https://user-images.githubusercontent.com/68071764/214917814-d2438e17-d7e1-4824-9490-28e0fecb78ef.png)

- dfxtp_4 = 1613,
- Number of cells = 18036,
- Flop ratio = 1613/18036 = 0.0894= 8.94%

### characterize synthesis results 

After running synthesis, inside the runs/[date]/results/synthesis is picorv32a_synthesis.v which is the mapping of the netlist to standard cell library using ABC. The runs/[date]/reports/synthesis will contain synthesis statistic reports and static timing analysis reports. The runs/[date]/synthesis/logs contains log files for the terminal output dumps for running yosys and OpenSTA.

- ![Screenshot 2023-01-26 at 1 56 33 PM](https://user-images.githubusercontent.com/68071764/214918935-c3e667d5-eb07-409e-8652-52685cecc380.png)

- ![Screenshot 2023-01-26 at 1 58 45 PM](https://user-images.githubusercontent.com/68071764/214919005-e3ef8383-01bd-471c-98bf-6a19be2667d9.png)
- ![Screenshot 2023-01-26 at 2 07 24 PM](https://user-images.githubusercontent.com/68071764/214919070-7d4d1438-d53a-437f-96a1-b992858cde27.png)


## Day 2: Floorplanning and library cells

### Floorplanning considerations

1. Utilization Factor & Aspect Ratio

- Two parameters are of importance when it comes to floorplanning namely, Utilisation Factor and Aspect Ratio. They are defined as follows:

  Utilisation Factor =  Area occupied by netlist /  Total area of core         
                   
  Aspect Ratio =  Height /  Width
                 
- A Utilisation Factor of 1 signifies 100% utilisation leaving no space for extra cells such as buffer. However, practically, the Utilisation Factor is 0.5-0.6. Likewise, an Aspect ratio of 1 implies that the chip is square shaped. Any value other than 1 implies rectanglular chip.

2. Pre-placed cells
Once the Utilisation Factor and Aspect Ratio has been decided, the locations of pre-placed cells need to be defined. Pre-placed cells are IPs comprising large combinational logic which once placed maintain a fixed position. Since they are placed before placement and routing, the are known as pre-placed cells.

3. Decoupling capacitors
Pre-placed cells must then be surrounded with decoupling capacitors (decaps). The resistances and capacitances associated with long wire lengths can cause the power supply voltage to drop significantly before reaching the logic circuits. This can lead to the signal value entering into the undefined region, outside the noise margin range. Decaps are huge capacitors charged to power supply voltage and placed close the logic circuit. Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the circuit. They pervent crosstalk and enable local communication.

![Screenshot 2023-01-27 at 3 03 22 PM](https://user-images.githubusercontent.com/68071764/215054620-3e9ac8c2-6014-4433-8e53-600f2b929822.png)

![Screenshot 2023-01-27 at 2 59 12 PM](https://user-images.githubusercontent.com/68071764/215054634-8a07fab0-d9c4-4f73-8fe5-975b3233177d.png)

4. Power Planning
Each block on the chip, however, cannot have its own decap unlike the pre-placed macros. Therefore a good power planning ensures that each block has its own VDD and VSS pads connected to the horizontal and vertical power and GND lines which form a power mesh.

5. Pin Placement
The netlist defines connectivity between logic gates. The place between the core and die is utilised for placing pins. The connectivity information coded in either VHDL or Verilog is used to determine the position of I/O pads of various pins. Then, logical placement blocking of pre-placed macros is performed so as to differentiate that area from that of the pin area.

6. Logical Cell Placement Blockage 
This makes sure that the automated placement and routing tool does not place any cell on the pin locations of the die.

-Below are all 6 steps for floor planning:

![Screenshot 2023-01-27 at 3 15 07 PM](https://user-images.githubusercontent.com/68071764/215057001-397edcea-b219-445d-ad1b-9c22f2b2aa0e.png)

### Floorplan run on OpenLANE & view in Magic

1. <b>Set configuration variables:</b> Before running floorplan stage, the configuration variables or switches must be configured first. The configuration variables are on openlane/configuration:

- floorplan.tcl - System default envrionment variables
- conifg.tcl
- sky130A_sky130_fd_sc_hd_config.tcl

![Screenshot 2023-01-27 at 4 24 43 PM](https://user-images.githubusercontent.com/68071764/215093714-8e87d313-9dab-4ab4-bb2d-180c8fa7bf4e.png)
![Screenshot 2023-01-27 at 4 22 02 PM](https://user-images.githubusercontent.com/68071764/215093745-881c1569-91e8-4973-bdc8-02a9292b9a13.png)
![Screenshot 2023-01-27 at 4 22 44 PM](https://user-images.githubusercontent.com/68071764/215093765-785d2ae2-e3e2-4f01-8bf1-3977044dc4f3.png)

Floorplan envrionment variables or switches:

1. FP_CORE_UTIL - floorplan core utilisation
2. FP_ASPECT_RATIO - floorplan aspect ratio
3. FP_CORE_MARGIN - Core to die margin area
4. FP_IO_MODE - defines pin configurations (1 = equidistant/0 = not equidistant)
5. FP_CORE_VMETAL - vertical metal layer
6. FP_CORE_HMETAL - horizontal metal layer

2. <b>Run floorplan on OpenLane:</b>

run_floorplan
![Screenshot 2023-01-27 at 6 24 27 PM](https://user-images.githubusercontent.com/68071764/215095780-c33f67b6-b40e-4d0f-b56e-c98a73587660.png)

3. <b>Check the results:</b> The output of this stage is runs/[date]/results/floorplan/picorv32a.floorplan.def which is a design exchange format, containing the die area and positions.

 ![Screenshot 2023-01-28 at 12 06 37 AM](https://user-images.githubusercontent.com/68071764/215167675-e19dabcf-132b-4285-adf5-58ebcbddf025.png)

 ![Screenshot 2023-01-27 at 11 35 48 PM](https://user-images.githubusercontent.com/68071764/215165947-8610cc63-b6e5-4749-a945-e2aab5d7306f.png)

- The die area here is in database units and 1 micron is equivalent to 1000 database units. Thus area of the die is <b> (660685/1000)microns*(671405/1000)microns = 443587 microns squared.</b>

4. <b>View the layout on magic.</b> Open def file using <b>magic:</b>

‌<b>ativirani07@vsd-pd-workshop-01:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/my_run1/results/floorplan$ magic -T /home/ativirani07/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../temp/merged.lef def read  picorv32a.floorplan.def</b>


![Screenshot 2023-01-28 at 1 29 16 PM](https://user-images.githubusercontent.com/68071764/215254484-933a150d-d305-4762-8c3a-0625836903fc.png)
![Screenshot 2023-01-29 at 1 04 20 AM](https://user-images.githubusercontent.com/68071764/215307665-94449121-add7-4c3e-a4f6-db9c52c3a16b.png)

- One can zoom into Magic layout by selecting an area with left and right mouse clcik followed by pressing "z" key. Here, equidistant input pins (FP_IO_MODE = 1) can be viewed: To center the view, press "s" to select whole die then press "v". Type "what" in tkcon to display information of selected object. These objects might be IO pin, decap cell, or well taps as shown below.

![Screenshot 2023-01-29 at 1 11 36 AM](https://user-images.githubusercontent.com/68071764/215307746-c4ba7593-a397-4cf4-b2ef-f5f8d99ea041.png)
![Screenshot 2023-01-29 at 1 13 45 AM](https://user-images.githubusercontent.com/68071764/215307791-d554058f-8f7c-41c4-9ad6-34d855166ee2.png)
![Screenshot 2023-01-29 at 1 15 26 AM](https://user-images.githubusercontent.com/68071764/215307920-a6e4ee19-ff9e-490d-975a-be97bb73ac82.png)
![Screenshot 2023-01-29 at 1 15 49 AM](https://user-images.githubusercontent.com/68071764/215307926-d2672aa1-ebee-4376-a9a3-4da97f85293c.png)


5. <b>Run placement:</b> The objective of placement is the convergence of overflow value. If overflow value progressively reduces during the placement run it implies that the design will converge and placement will be successful. Post placement, the design can be viewed on magic within results/placement directory:

<b>run_placement</b>
![Screenshot 2023-01-29 at 2 19 55 AM](https://user-images.githubusercontent.com/68071764/215308142-030a2712-0af6-413f-aeb4-3533694feb58.png)

Zoomed-in views of the standard cell placement:
![Screenshot 2023-01-29 at 2 29 10 AM](https://user-images.githubusercontent.com/68071764/215308162-aeebdee4-3a28-40f4-898d-35a0568aa118.png)

<b>Note: Power distribution network generation is usually a part of the floorplan step. However, in the openLANE flow, floorplan does not generate PDN. The steps are - floorplan, placement CTS and then PDN</b>

### Library Characterization:

Of all RTL-to-GDSII stages, one common thing that the EDA tool always need is data from the library of gates which keeps all standards cells (and, or, buffer gates,...), macros, IPs, decaps, etc. Same cells might have different flavors inside the library (different sizes, delays, threshold voltage). Bigger cell sizes means bigger drive strength to drive longer and thicker wires. Bigger threshold voltage (due to bigger size) will take more time to switch(slower clock) than those with smaller threshold voltage.

A single cell needs to go through the cell design flow. The inputs to make a single cell comes from the foundry Process Design Kits:

1. DRC & LVS Rules = tech files and poly subtrate paramters (CUSTOME LAYOUT COURSE)
2. SPICE Models = Threshold, linear regions, saturation region equations with added foundry parameters. Including NMOS and PMOS parameteres (Ciruit Deisgn and Spice simulation Course)
3. User defined Spec = Cell height (separation between power and ground rail), Cell width (depends on drive strength), supply voltage, metal layer requirement (which metal layer the cell needs to work)

The library cell developer must adhere to the rules given on the inputs so that when the cell is used on a real design, there will be no errors. Next is design the library cell:

1. Design the circuit function (Output: circuit design language (CDL))
2. Model the pmos and nmos that meets input library requirement
3. Layout the design using Euler's path and sticky diagram to produce best area. This can be done on magic layout tool.The outputs are:
   - GDSII (layout file)
   - LEF (defines the width and height of cell)
   - extract spice netlist .cir (parasitics of each element of cell: resistance, capacitance) Afte design is characterization using GUNA software, where the outputs are timing, noise, and power characterization. .

<b>Timing Characterization:</b>

Below are the timing variables for slew. This is two inverters in series, red is output of first inverter and blue is output of second inverter:
![Screenshot 2023-01-28 at 11 38 28 PM](https://user-images.githubusercontent.com/68071764/215308882-5d163d0d-9f3c-4353-8dbe-1935b7f905eb.png)

Below are the timing variables for propagation delay. The red is input waveform and blue is output waveform of the buffer. The left side is rise delay and right side is fall delay.

![Screenshot 2023-01-29 at 11 54 06 AM](https://user-images.githubusercontent.com/68071764/215309103-8eb1af2f-dd1d-45ba-bb5b-71937aa9a7d4.png)

Negative propagation delay is unexpected. That means the output comes before the input so designer needs to choose correct threshold point to produce positive delay. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

##DAY 3: Design a Library Cell using Magic Layout and Ngspice Characterization

Configurations on OpenLANE can be changed on the flight. For example, to change IO_mode to be not equidistant, use <b>% set ::env(FP_IO_MODE) 2;</b> on OpenLANE. The IO pins will not be equidistant on mode 2 (default of 1).
Run floorplan again via <b>% run_floorplan</b> and view the def layout on magic. However, changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session. To echo current value of variable:<b> echo $::env(FP_IO_MODE)</b>

<b>Designing a Library Cell:</b>
1. SPICE deck = component connectivity (basically a netlist) of the CMOS inverter.
2. SPICE deck values = value for W/L (0.375u/0.25u means width is 375nm and lengthis 250nm). PMOS should be wider in width(2x or 3x) than NMOS. The gate and supply voltages are normally a multiple of length (in the example, gate voltage can be 2.5V)
3. Add nodes to surround each component and name it. This will be used in SPICE to identify a component.

### SPICE Deck Netlist Description:

![Screenshot 2023-01-29 at 2 52 19 AM](https://user-images.githubusercontent.com/68071764/215309427-c29b0a53-86f6-4d3e-ad8b-ba43f2743472.png)

Notes:

- Syntax for the PMOS and NMOS descriptiom:
<b>'[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]'</b>
- All components are described based on nodes and its values
- <b>.op</b> is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.5 steps
- <b>tsmc_025um_model.mod</b> is the model file containing the technological parameters for the 0.25um NMOS and PMOS The steps to simulate in SPICE:
```
source [filename].cir
run
setplot 
dc1 
plot out vs in 
```
SPICE Analysis for Switching Threshold and Propagation Delay:
CMOS robustness depends on:

1. Switching threshold = Vin is equal to Vout. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.

2. Propagation delay = rise or fall delay

DC transfer analysis is used for finding switching threshold. SPICE DC analysis below uses DC input of 2.5V. Simulation operation is DC sweep from 0V to 2.5V by 0.05V steps:
```
Vin in 0 2.5
*** Simulation Command ***
.op
.dc Vin 0 2.5 0.05
```
Below is the result of SPICE simulation for DC analysis, the line intersection is the switching threshold:
![Screenshot 2023-01-29 at 12 20 46 PM](https://user-images.githubusercontent.com/68071764/215310169-f85ca662-f041-4377-a590-3513efad6274.png)

Meanwhile, transient analysis is used for finding propagation delay. SPICE transient analysis uses pulse input:
```
starts at 0V
ends at 2.5V
starts at time 0
rise time of 10ps
fall time of 10ps
pulse-width of 1ns
period of 2ns
```
![Screenshot 2023-01-29 at 3 54 01 AM](https://user-images.githubusercontent.com/68071764/215310235-37ad0659-6a26-4fbe-ae77-b8f1b586a493.png)

The simulation operation has 10ps step and ends at 4ns:
```
 Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
 *** Simulation Command ***
 .op
 .tran 10p 4n
```
Below is the result of SPICE simulation for transient analysis:

![Screenshot 2023-01-29 at 3 54 21 AM](https://user-images.githubusercontent.com/68071764/215310332-c29d773e-583b-4e1b-bfce-03cc5d481fa8.png)

### SPICE Deck creation & Simulation

A SPICE deck includes information about the following:

1. Model description
2. Netlist description
3. Component connectivity
4. Component values
5. Capacitance load
6. Nodes
7. Simulation type and parameters
8. Libraries included

### Inverter Standard cell Layout & SPICE extraction

The Magic layout of a CMOS inverter will be used so as to intergate the inverter with the picorv32a design. To do this, inverter magic file is sourced from vsdstdcelldesign by cloning it within the openlane_working_dir/openlane directory as follows:

git clone https://github.com/nickson-jose/vsdstdcelldesign

This creates a vsdstdcelldesign named folder in the openlane directory.  

To invoke magic to view the sky130_inv.mag file, the sky130A.tech file must be included in the command along with its path. To ease up the complexity of this command, the tech file can be copied from the magic folder to the vsdstdcelldesign folder.

![Screenshot 2023-01-29 at 4 07 20 AM](https://user-images.githubusercontent.com/68071764/215311231-061e9777-b1d2-4702-8797-242d66e4c7fd.png)

### CMOS Fabrication Process (16-Mask CMOS Process):
1. <b>Selecting a substrate</b> = Layer where the IC is fabricated. Most commonly used is P-type substrate
2. <b>Creating active region for transistor</b> = Separate the transistor regions using SiO2 as isolation

- Mask 1 = Covers the photoresist layer that must not be etched away (protects the two transistor active regions)
- Photoresist layer = Can be etched away via UV light
- Si3N4 layer = Protection layer to prevent SiO2 layer to grow during oxidation (oxidation furnace)
- SiO2 layer = Grows during oxidation (LOCOS = Local Oxidation of Silicon) and will act as isolation regions between transistors or active regions

![Screenshot 2023-01-29 at 1 32 56 PM](https://user-images.githubusercontent.com/68071764/215313442-6c2b49bd-eba9-4e72-aa40-7d8e53bca2c7.png)

3. <b>N-Well and P-Well Fabrication</b> = Fabricate the substrate needed by PMOS (N-Well) and NMOS (P-Well)

- Phosporus (5 valence electron) is used to form N-well
- Boron (3 valence electron) is used to form P-Well.
- Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated then Mask 3 while N-Well (PMOS side) is being fabricated

![Screenshot 2023-01-29 at 1 34 20 PM](https://user-images.githubusercontent.com/68071764/215313474-496fde2e-2cf3-4f02-b3d8-d3ac6756a450.png)

4. <b>Formation of Gate</b> = Gate fabrication affects threshold voltage. Factors affecting threshold voltage includes:
![Screenshot 2023-01-29 at 1 35 01 PM](https://user-images.githubusercontent.com/68071764/215313511-ae895cc8-4879-4593-8faa-86051a63102f.png)

Main parameters are:

- Doping Concentration = Controlled by ion implantation (Mask 4 for Boron implantation in NMOS P-Well and Mask 5 for Arsenic implantation in PMOS N-Well)
- Oxide capacitance = Controlled by oxide thickness (SiO2 layer is removed then rebuilt to the desire thickness)
Mask 6 is for gate formation using polysilicon layer.

![Screenshot 2023-01-29 at 1 37 20 PM](https://user-images.githubusercontent.com/68071764/215313608-3414ea76-0f89-40a8-8e6b-dd9646c1c522.png)

5. <b>Lightly Doped Drain formation </b>= Before forming the source and drain layer, lightly doped impurity is added:

- Mask 7 for N- implantation (lightly doped N-type) for NMOS
- Mask 8 for P- implantation (lightly doped P-type) for PMOS.
- Heavily doped impurity (N+ for NMOS and P+ for PMOS) is for the actual source and drain but the lightly doped impurity will help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.

![Screenshot 2023-01-29 at 1 38 33 PM](https://user-images.githubusercontent.com/68071764/215313687-74c4ec20-9915-4a27-9ee6-5e97089cc1bf.png)

6. <b>Source and Drain Formation </b>= Mask 9 is for N+ implantation and Mask 10 for P+ implantation

Channeling is when implantations dig too deep into substrate so add screen oxide before implantation
The side-wall spacers maintains the N-/P- while implanting the N+/P+

![Screenshot 2023-01-29 at 1 40 36 PM](https://user-images.githubusercontent.com/68071764/215313831-1e3d87ae-25ba-42b3-9741-ab193825026a.png)

7. <b>Form Contacts and Interconnects </b>= TiN is for local interconnections and also for bringing contacts to the top. TiS2 is for the contact to the actual Drain-Gate-Source. Mask 11 is for etching off the TiN interconnect for the first layer contact.

![Screenshot 2023-01-29 at 1 41 45 PM](https://user-images.githubusercontent.com/68071764/215313884-fed4b522-7c5b-49e5-bd32-00ee7665f920.png)

8. <b>Higher Level Metal Formation</b> = We need to planarize first the layer via CMP before adding a metal interconnect. Aluminum contact is used to connect the lower contact to higher metal layer. Process is repeated until the contact reached the outermost layer.

- Mask 12 is for first contact hole
- Mask 13 is for first Aluminum contact layer
- Mask 14 is for second contact hole
- Mask 15 is for second Aluminum contact layer. Mask 16 is for making contact to topmost layer.

![Screenshot 2023-01-29 at 1 43 00 PM](https://user-images.githubusercontent.com/68071764/215313943-a93bcb30-6a40-47d3-9856-f1779097b8f8.png)

### Inverter Standard cell Layout & SPICE extraction

- The Magic layout of a CMOS inverter will be used so as to intergate the inverter with the picorv32a design. To do this, inverter magic file is sourced from <b>vsdstdcelldesign</b> by cloning it within the <b>openlane_working_dir/openlane</b> directory as follows:

git clone https://github.com/nickson-jose/vsdstdcelldesign

- This creates a vsdstdcelldesign named folder in the openlane directory.

- To invoke magic to view the sky130_inv.mag file, the sky130A.tech file must be included in the command along with its path. To ease up the complexity of this command, the tech file can be copied from the magic folder to the vsdstdcelldesign folder.

- The sky130_inv.mag file can then be invoked in Magic very easily:

magic -T sky130A.tech sky130_inv.mag &

![Screenshot 2023-01-29 at 2 42 46 PM](https://user-images.githubusercontent.com/68071764/215317722-284b5d87-2711-4646-aff5-6bc6ecc2738d.png)
![Screenshot 2023-01-29 at 3 04 48 PM](https://user-images.githubusercontent.com/68071764/215318265-e3842c9d-16ff-4393-9fad-b804538de4df.png)

- In Sky130 the first layer is called the local interconnect layer or Locali as shown above.

- To verify whether the layout is that of CMOS inverter, verification of P-diffusiona nd N-diffusion regions with Polysilicon can be observed:

![Screenshot 2023-01-29 at 3 07 05 PM](https://user-images.githubusercontent.com/68071764/215317892-285f4fa4-9b13-4e89-aeff-2f69b507b499.png)
![Screenshot 2023-01-29 at 3 08 55 PM](https://user-images.githubusercontent.com/68071764/215318284-eabe8314-e75e-4299-a950-217b605906c4.png)

Other verification steps are to check drain and source connections. The drains of both PMOS and NMOS must be connected to output port (here, Y) and the sources of both must be connected to power supply VDD (here, VPWR).

<b>LEF or library exchange format:</b> A format that tells us about cell boundaries, VDD and GND lines. It contains no info about the logic of circuit and is also used to protect the IP.

SPICE extraction: Within the Magic environment, following commands are used in tkcon to achieve .mag to .spice extraction:
``` 
extract all
ext2spice cthresh 0 rethresh 0
ext2spice
 ```
![Screenshot 2023-01-29 at 3 36 23 PM](https://user-images.githubusercontent.com/68071764/215320306-686959e9-d200-4a0d-994d-ce529663e9ff.png)
 
 - Now check what is present inside spice file.
 
 ![Screenshot 2023-01-29 at 3 40 08 PM](https://user-images.githubusercontent.com/68071764/215321040-b5cac56a-d6cb-47de-9598-8d2ef2c90e5b.png)

 So here is our extracted SPICE model present with us.
 
 The above SPICE model give connectivity information of our inverter. Now for transient analysis we have to define the connections.

- VGND to be connected to VSS
- supply voltage (VDD) to be connected form VPWR to VSS (ground).
- create a node 0 and give VDD = 3V.
- Give pulse voltage between A and VGND (VSS).
 
 command to our SPICE DECK
 
```
 * SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

* .subckt sky130_inv A Y VPWR VGND
M0 Y A VGND VGND nshort_model.0 ad=1435 pd=152 as=1365 ps=148 w=35 l=23
M1 Y A VPWR VPWR pshort_model.0 ad=1443 pd=152 as=1517 ps=156 w=37 l=23
C0 A VPWR 0.08fF
C1 Y VPWR 0.08fF
C2 A Y 0.02fF
C3 Y VGND 2fF
C4 VPWR VGND 0.74fF
* .ends

* Power supply 
VDD VPWR 0 3.3V 
VSS VGND 0 0V 

* Input Signal
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

* Simulation Control
.tran 1n 20n
.control
run
.endc
.end 
 ```
For simulation, ngspice is invoked in ther terminal:
 ```
 ngspice sky130_inv.spice
 ```
 ![Screenshot 2023-01-29 at 5 22 20 PM](https://user-images.githubusercontent.com/68071764/215325250-06885339-af1c-4a31-8418-21ee3497cf1d.png)

 The output "y" is to be plotted with "time" and swept over the input "a":
```
 plot y vs time a
 ```
 ![Screenshot 2023-01-29 at 5 51 10 PM](https://user-images.githubusercontent.com/68071764/215325775-3cb2b578-39d2-4696-820c-f20ee71ead86.png)

 The spikes in the output at switching points is due to low capacitance loads. This can be taken care of by editing the spice deck to increase the load capacitance value.
 
 ### Inverter Standard cell characterization
Four timing parameters are used to characterize the inverter standard cell:

- Rise transition: Time taken for the output to rise from 20% of max value to 80% of max value
- Fall transition- Time taken for the output to fall from 80% of max value to 20% of max value
- Cell rise delay = time(50% output rise) - time(50% input fall)
- Cell fall delay = time(50% output fall) - time(50% input rise)

 1. Rise transiton - Time taken by output waveform to transit from 20% to 80% of VDD 20% value (0.66) = 2.19298 ns
    80% value (2.64) = 2.15672= 0.036
 
      2.19298- 2.15672= 0.03626ns
 
![Screenshot 2023-01-29 at 8 34 11 PM](https://user-images.githubusercontent.com/68071764/215336273-41fbc96d-59fe-4c81-a2f6-393978e774fc.png)
 
 2. Fall transition - Time taken by output waveform to transit from 80% (2.64) to 20% (0.66) of VDD.
 
 ![Screenshot 2023-01-29 at 10 28 14 PM](https://user-images.githubusercontent.com/68071764/215342922-f92094c2-941e-4829-830c-36eebc718b2f.png)

     4.07407-4.03704= 0.03703ns
 
3 & 4. Propagation delay - The difference between the time when output as well as input is at 50% (1.65). ( o/p falls and i/p rises gives fall delay, o/p rises and i/p falls gives us the rise delay)

rise delay: 

![Screenshot 2023-01-29 at 8 59 19 PM](https://user-images.githubusercontent.com/68071764/215336738-65dadd10-74ce-482f-bc54-189cfd5d7e63.png)
 
 2.18148-2.15556= 0.02592ns
 
fall delay:
 
![Screenshot 2023-01-29 at 10 40 05 PM](https://user-images.githubusercontent.com/68071764/215343464-e3d805ae-a066-4afc-aed0-fd1fe0a888a8.png)

 4.05287-4.05057= 0.0023ns

 
The above characterisation is done at 27 C.

Next objective is to use this layout of inverter to create a lef file. Using this lef in openlane and plugging this cell we will make a custom cell. We will plug this in picorv32a.
 
 ### Magic Features & DRC rules
The technology file is a setup file that declares layer types, colors, patterns, electrical connectivity, DRC, device extraction rules and rules to read LEF and DEF files. Magic layouts can be sourced from opencircuitdesign.com using the command:
 
 
 ### DAY 4 Pre-layout timing analysis and importance of good clock tree
 
 #### Lab Part 1 [Day 4] - Extracting the LEF File:
 
 PnR tool does not need all informations from the .mag file like the logic part but only PnR boundaries, power/ground ports, and input/output ports. This is what a LEF file actually contains. So the next step is to extract the LEF file from Magic. But first, we need to follow guidelines of the PnR tool for the standard cells:

- The input and output ports lies on the intersection of the horizontal and vertical tracks (ensure the routes can reach that ports).
The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch

- The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch.

- To check these guidelines, we need to change the grid of Magic to match the actual metal tracks. The ``` pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks ```.info contains those metal informations.

- Use grid command inside the tkon terminal to match the tracks informations:
 
 ![Screenshot 2023-01-30 at 12 30 52 AM](https://user-images.githubusercontent.com/68071764/215351344-447b64f0-022d-4dbe-b9a8-10a0d3ed76a1.png)
 
![Screenshot 2023-01-30 at 12 37 20 AM](https://user-images.githubusercontent.com/68071764/215351352-d6078296-fd0c-4e2e-bf23-de329af27a92.png)

-  The grids show where the routing for the local-interconnet layer can only happen, the distance of the grid lines are the required pitch of the wire. Below, we can see that the guidelines are satisfied:
 
 ![Screenshot 2023-01-30 at 12 37 20 AM](https://user-images.githubusercontent.com/68071764/215351464-48cf3724-fcfc-41ba-b7c6-495b5ef62bf3.png)

-  Next, we will extract the LEF file. The LEF file contains the cell size, port definitions, and properties which aid the placer and router tool. With that, the ports definition, port class, and port use must be set first. The instructions to set these definitions via Magic are on the <B>vsdstdcelldesign</b> repo.

- Next, save the mag file with a new filename save sky130_myinverter.mag. Then type lef write on the tcon terminal. It will generate a LEF file with same name as the magfile sky130_myinverter.lef. Inside that LEF file is:
 
 ![image](https://user-images.githubusercontent.com/68071764/215351586-d4da20b4-44e2-4321-8145-ca65fe576216.png)


### Lab Part 2 [Day 4] - Plug-in the Customized Inverter Cell to OpenLane:

Inside ``` pdks/sky130A/libs.ref/sky130_fd_sc_hd/```  are the liberty timing files for SKY130 PDK which contains the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different different supply voltages (1v80, 1v65, 1v95, etc.). These are the so called PVT corners. The library name sky130_fd_sc_hd__ss_025C_1v80 describes the PVT corner as slow-slow (delay is maximum), 27° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell is obtained by simulating the cell in a variety of operating conditions (different corners) and these data are represented in the liberty file.

The liberty file characterizes all cells and is used by the ABC script during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file. Same cell functionality but different sizes are available inside the library.

Provided inside the cloned vsdstdcelldesign are the liberty files containing the customized inverter cell.

1. Copy the extracted lef file sky130_vsdinv.lef and the liberty files sky130*.lib from /openlane/vsdstdcelldesign/libs to the src directory of picorv32a. 

![Screenshot 2023-01-30 at 1 08 47 AM](https://user-images.githubusercontent.com/68071764/215500593-e9f72be8-dd5e-4381-ae50-c1ce4ce09601.png)

2. Add the folowing to config.tcl inside the picorv32a:
```
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/scr/sly130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/scr/sly130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/scr/sly130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/scr/sly130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

This sets the liberty file that will be used for ABC mapping of synthesis (LIB_SYNTH) and for STA (_FASTEST,_SLOWEST,_TYPICAL) and also the extra LEF files (EXTRA_LEFS) for the customized inverter cell. The whole config.tcl then is:


```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_MIN) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_MAX) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set ::env(FP_CORE_UTIL) 65
set ::env(FP_IO_VMETAL) 4
set ::env(FP_IO_HMETAL) 3


set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
source $filename
}
```

3. Run docker and prepare the design picorv32a. Plug the new lef file to the OpenLANE flow via:
```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```

4. Next run_synthesis. Below is the synthesis statistics report runs/[date]/reports/synthesis/1-synthesis.AREA_0.stat.rpt after the run, and as we can see sky130_vsdinv cell is successfully included in the design!

![Screenshot 2023-01-30 at 10 29 24 AM](https://user-images.githubusercontent.com/68071764/215497791-931f1f83-1235-4158-8293-e8922fd377db.png)

- 
![Screenshot 2023-01-30 at 10 30 11 AM](https://user-images.githubusercontent.com/68071764/215500801-b3f25534-8a9e-430d-b438-f4e000892b43.png)

#### Delay Table:
In order to avoid large skew between endpoints of a clock tree (signal arrives at different point in time):

- Buffers on the same level must have same capacitive load to ensure same timing delay or latency on the same level.
- Buffers on the same level must also be the same size (different buffer sizes -> different W/L ratio -> different resistance -> different RC constant -> different delay).

![image](https://user-images.githubusercontent.com/68071764/215745841-81d6d294-af69-4281-ac03-6a1f04059b27.png)
Buffers on different level will have different capacitive load and buffer size but as long as they are the same load and size on the same level, the total delay for each clock tree path will be the same thus skew will remain zero. This means different levels will have varying input transition and output capacitive load and thus varying delay.

Delay tables are used to capture the timing model of each cell and is included inside the liberty file. The main factor in delay is the output slew. The output slew in turn depends on capacitive load and input slew. The input slew is a function of previous buffer's output cap load and input slew and it also has its own transition delay table.

![image](https://user-images.githubusercontent.com/68071764/215746037-0ebeced2-d7b1-4b88-b9ed-c1e62d109c4e.png)

Notice how skew is zero since delay for both clock path is x9'+y15.

### Lab Part 3 [Day 4] - Fix Negative Slack:

1. Let us change some variables to minimize the negative slack. We will now change the variables "on the flight". Use echo $::env(SYNTH_STRATEGY) to view the current value of the variables before changing it:
```
% echo $::env(SYNTH_STRATEGY)
AREA 0
% set ::env(SYNTH_STRATEGY) "DELAY 0"
% echo $::env(SYNTH_BUFFERING)
1
% echo $::env(SYNTH_SIZING)
0
% set ::env(SYNTH_SIZING) 1
% echo $::env(SYNTH_DRIVING_CELL)
sky130_fd_sc_hd__inv_2
```

With SYNTH_STRATEGY of Delay 0, the tool will focus more on optimizing/minimizing the delay, index can be 0 to 3 where 3 is the most optimized for timing (sacrificing more area). SYNTH_BUFFERING of 1 ensures cell buffer will be used on high fanout cells to reduce delay due to high capacitance load. SYNTH_SIZING of 1 will enable cell sizing where cell will be upsize or downsized as needed to meet timing. SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength (larger driving cell needed).

2.Below is the log report for slack and area. 

![Screenshot 2023-01-30 at 2 46 57 PM](https://user-images.githubusercontent.com/68071764/215747607-f802c7da-ed6c-42e6-9439-ef1406a9ba56.png)

3. Next run floor plan by executing the following codes one by one:
```
init_floorplan
place_io
global_placement_or
detailed_placement
tap_decap_or
detailed_placement
gen_pdn
run_cts
```

![image](https://user-images.githubusercontent.com/68071764/215748056-c872ce6d-02d6-4e99-b96f-dde190a0bf98.png)

![Screenshot 2023-01-30 at 1 35 00 PM](https://user-images.githubusercontent.com/68071764/215497475-bf6c1bc5-f662-443e-8ba6-e12e8b2ca5e4.png)

We can see our sky130_vsdinv file in the merged.lef file inside the tmp folder. The macro is present.

![Screenshot 2023-01-30 at 1 57 48 PM](https://user-images.githubusercontent.com/68071764/215497300-37e8753f-4b66-4022-b24c-10a00fc08e53.png)

We can also see the sky130_vsdinv inside the layout also:

![image](https://user-images.githubusercontent.com/68071764/215750555-edf4a27e-597f-44e8-acce-87f7e103ab0b.png)


### Timing Analysis (Pre-Layout STA using Ideal Clocks):
Pre-layout STA will not yet include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).
![image](https://user-images.githubusercontent.com/68071764/215751429-9278e8c1-2d82-4860-939d-f83f6eeda5af.png)

Setup timing analysis equation is:
```
Θ < T - S - SU
```
- Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop
- T = Time period, also called the required time
- S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

![image](https://user-images.githubusercontent.com/68071764/215751704-eb393729-f001-444f-8850-f779b68e249f.png)

- SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.

### Lab Part 5 [Day 4] - Pre-Layout STA with OpenSTA:

In cts we try to change the netlist by making clock tree.

The below files can be found in th extras folder in vsdstdcelldesign.

Making the pre_sta.conf and save it in the openlane folder.
```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/ativirani07/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/ativirani07/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/ativirani07/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/ativirani07/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/picorv32a.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
```

After cts new .v files start getting created.

Creating picorv32a.sdc and save this file in the src folder of picorv32a folder.
```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 12.000
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.65
create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set IO_PCT  0.2
set input_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"


set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```

This is replicating the same results as we had after run synthesis stage. pre_sta.conf will be the fill on which we will be doing our STA analysis.

To perform pre STA run the command below by opening the terminal in openlane folder which is inside the openlane_working_dir.
```
sta [file_name] // (our case = pre_sta.conf)
```

As we haven't done CTS hold time doesn't hold any significance. The delay of any cell is function of input slew and output load. We can play with these data and can get slack as positive. So we can also play with some of some of these parameters.

Command to check what a particular cell is driving:
```
report_net -connections _[cell number/net number]_

```
To replace the buffer (from buf 1 to buf 4) we use the following command.

replace_cell _23732_ sky130_fd_sc_hd__buf_4

report check will report the worst path, by default it is the max setup slack
```
report_checks -field {net cap slew input_pins} -digits 4
```
Upsizing the buffer will change the cell. We can replace cell to bring donw the slack. It will slightly increase the area,
```
1. First find the cell you want to replace. 
2. Then echo its net details by the following command 
report_net -connections _[cell number/net number]_ 
3. Then use the replace command to upsize it.
replace_cell _[net to be replaced]_ [new net name]

```
## Clock Tree Synthesis

There are three parameters that we need to consider when building a clock tree:

* Clock Skew = In order to have minimum skew between clock endpoints, clock tree is used. This results in equal wirelength (thus equal latency/delay) for every path of the clock.
* Clock Slew = Due to wire resistance and capacitance of the clock nets, there will be slew in signal at the clock endpoint where signal is not the same with the original input clock signal anymore. This can be solved by clock buffers. Clock buffer differs in regular cell buffers since clock buffers has equal rise and fall time.
* Crosstalk = Clock shielding prevents crosstalk to nearby nets by breaking the coupling capacitance between the victim (clock net) and aggresor (nets near the clock net), the shield might be connected to VDD or ground since those will not switch. Shileding can also be done on critical data nets.

![Screenshot 2023-01-30 at 1 18 37 PM](https://user-images.githubusercontent.com/68071764/215497587-9d1d6b05-7d46-4d74-8c50-33ad61ee3d69.png)

 
 ### LAB DAY 4 (PART 4)
 
 In the terminal in which we run the run_cts command there only go to openroad. Type the following command in the terminal.
```
openroad
```

This will open the open road. Our objective to do the analysis of the entire circut where clock tree has been build now. Now we will open OpenSTA here. For timing alnalysis.

1. We first create a db `
2. db is create using lef and def file. In our analysis we use these db. (It is a one time process. Whenever lef changes we have to change the db)
3. To create a db

// first read lef (it is inside the tmp folder (merged.lef)
read_lef [location] {my case = read_lef /openLANE_flow/designs/picorv32a/runs/30-01_04-42/tmp/merged.lef}

// secondly read def (it is present inside cts folder present under the results folder/cts)
read_def [location] {my case = /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/cts/picorv32a.cts.def}

// creating db
write_db [name] // my case = pico_cts.db (created under the openlane folder)

// reading db 
read_db [name] // my case = pico_cts.db

//  reading verilog (it is present inside cts folder present under the results/synthesis/picorv32a.synthesis_cts.v)
read_verilog [location] // {my case = /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis_cts.v}

// reading library (max)
read_liberty -max $::env(LIB_FASTEST)

// reading library (min)
read_liberty -min $::env(LIB_SLOWEST)

// reading sdc
read_sdc [location] {my case = /openLANE/designs/picorv32a/src/my_base.sdc}

// now the clock has been generated 
set_propagated_clock [all_clocks]

// report
report_checks -path_delay min_max -format full_clock_expanded -digits 4

All the loaction should be after /openLANE_flow/.....
```
run_cts
```
The CTS run adds clock buffers in therefore buffer delays come into picture and our analysis from here on deals with real clocks. Setup and hold time slacks may now be analysed in the post-CTS STA anlysis in OpenROAD within the openLANE flow:
```
openroad
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/30-01_04-42/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
 ![Screenshot 2023-01-30 at 5 41 00 PM](https://user-images.githubusercontent.com/68071764/215495685-e7b43474-bbfc-4b7e-b3c7-9920c2f06c3e.png)
 
 For typical cornor both the slack is met i.e., no violation. For max and min cornor we have to do it seperatly because multicornor is not supported.

The buffer which we have:

![Screenshot 2023-01-30 at 4 27 15 PM](https://user-images.githubusercontent.com/68071764/215495934-b4d8a43e-b25c-484f-9c8c-88dbf02df8c4.png)

 
 
 
## DAY 5: Final Steps for RTL2GDS using TritonRoute and OpenSTA
 
 ### Maze Routing:
 One simple routing algorithm is Maze Routing or Lee's routing:

The shortest path is one that follows a steady increment of one (1-to-9 on the example below). There might be multiple path like this but the best path that the tool will choose is one with less bends. The route should not be diagonal and must not overlap an obstruction such as macros.
This algorithm however has high run time and consume a lot of memory thus more optimized routing algorithm is preferred (but the principles stays the same where route with shortest path and less bends is preferred)
 
 ![Screenshot 2023-01-30 at 6 27 15 PM](https://user-images.githubusercontent.com/68071764/215484704-c22bd851-f0ff-49e9-81c2-ad31f199c690.png)

 ### DRC Cleaning:
DRC cleaning is the next step after routing. DRC cleaning is done to ensure the routes can be fabricated and printed in silicon faithfully. Most DRC is due to the constraints of the photolitographic machine for chip fabrication where the wavelength of light used is limited. There are thousands of DRC and some DRC are:

- Minimum wire width
- Minimum wire pitch (center to center spacing)
- Minimum wire spacing (edge to edge spacing)
- Signal short = this can be solved my moving the route to next layer using vias. This results in more DRC (Via width, Via Spacing, etc.). Higher metal layer must be wider than lower metal layer and this is another DRC.
 
 ### Power Distribution Network:
 
This is just a review on PDN. The power and ground rails has a pitch of 2.72um thus the reason why the customized inverter cell has a height of 2.72 or else the power and ground rails will not be able to power up the cell. Looking at the LEF file runs/[date]/tmp/merged.nom.lef, you will notice that all cells are of height 2.72um and only width differs.

As shown below, power and ground flows from power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails.
 
 ![image](https://user-images.githubusercontent.com/68071764/215485019-aa6898fd-8f5e-4677-b079-95521f315c51.png)

 Lab Part 1 [Day 5] - Routing Stage:
 
 Power Distribution Network generation
 
Unlike the general ASIC flow, Power Distribution Network generation is not a part of floorplan run in OpenLANE. PDN must be generated after CTS and post-CTS STA analysis:
 
 gen_pdn
 
 run_routing 
 
 A DEF file will be formed runs/[date]/results/routing/picorv32.def Open the DEF file output of routing stage in Magic:
 

 
 Similar to what we did when we plugged in the custom inverter cell, look for sky130_myinverter at the DEF file then search that cell instance in magic:
 
 ![Screenshot 2023-01-30 at 5 52 10 PM](https://user-images.githubusercontent.com/68071764/215489976-87917e27-f482-420a-abb3-d6b0244d3255.png)

 ![Screenshot 2023-01-30 at 5 56 03 PM](https://user-images.githubusercontent.com/68071764/215490022-eeacf7a2-12fe-428d-86f9-3cfc5ad02147.png)

 ![Screenshot 2023-01-30 at 5 57 39 PM](https://user-images.githubusercontent.com/68071764/215490042-17cc534d-e01c-4a0b-bb99-b464bc6529e4.png)

 
 
 Differences from older OpenLANE versions
In the new version, FP_CORE_UTIL, FP_CORE_VMETAL and FP_CORE_HMETAL environment variables are missing in ioPlacer.log and config.tcl. They need to be included in config.tcl file.
run_floorplan fails after the STA analysis in the new version. An alternate command can be used: init_floorplan
SPEF extraction need not be externally performed in the new version. It has been integrated into the OpenLANE flow
Note: In the new version following commands may be used for an error-free flow:

```
init_floorplan 
place_io
global_placement_or
detailed_placement 
tap_decap_or
detailed_placement
gen_pdn
run_routing
 ```
### References
.
