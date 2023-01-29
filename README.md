# <b>OpenLane</b>
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

- source [filename].cir
- run
- setplot 
- dc1 
- plot out vs in 

SPICE Analysis for Switching Threshold and Propagation Delay:
CMOS robustness depends on:

1. Switching threshold = Vin is equal to Vout. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.

2. Propagation delay = rise or fall delay

DC transfer analysis is used for finding switching threshold. SPICE DC analysis below uses DC input of 2.5V. Simulation operation is DC sweep from 0V to 2.5V by 0.05V steps:

- Vin in 0 2.5
- *** Simulation Command ***
- .op
- .dc Vin 0 2.5 0.05

Below is the result of SPICE simulation for DC analysis, the line intersection is the switching threshold:
![Screenshot 2023-01-29 at 12 20 46 PM](https://user-images.githubusercontent.com/68071764/215310169-f85ca662-f041-4377-a590-3513efad6274.png)

Meanwhile, transient analysis is used for finding propagation delay. SPICE transient analysis uses pulse input:

- starts at 0V
- ends at 2.5V
- starts at time 0
- rise time of 10ps
- fall time of 10ps
- pulse-width of 1ns
- period of 2ns

![Screenshot 2023-01-29 at 3 54 01 AM](https://user-images.githubusercontent.com/68071764/215310235-37ad0659-6a26-4fbe-ae77-b8f1b586a493.png)

The simulation operation has 10ps step and ends at 4ns:

- Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
- *** Simulation Command ***
- .op
- .tran 10p 4n

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

<b>SPICE extraction:<b/> Within the Magic environment, following commands are used in tkcon to achieve .mag to .spice extraction:
 
- extract all
- ext2spice cthresh 0 rethresh 0
- ext2spice
 
![Screenshot 2023-01-29 at 3 36 23 PM](https://user-images.githubusercontent.com/68071764/215320306-686959e9-d200-4a0d-994d-ce529663e9ff.png)
 
 - Now check what is present inside spice file.
 
 ![Screenshot 2023-01-29 at 3 40 08 PM](https://user-images.githubusercontent.com/68071764/215321040-b5cac56a-d6cb-47de-9598-8d2ef2c90e5b.png)

 So here is our extracted SPICE model present with us.
 

