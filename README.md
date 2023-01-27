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

- Synthesis: RTL Converted to gate level netlist using standard cell libraries (SCL)
- Floor Planning/ Power Planning: Plan silicon area and create robust power distribution network. The power network usually uses the upper metal layer which are thicker than lower layer and thus lower resistance. This lowers the IR drop problem.
- Placement: Placing cells on floorplan rows aligned with sites
  1. Global Placement: for optimal position of cell
  2. Detailed Placement: for legal positions
- Clock tree synthesis: clock distribution to all flip flops and is usually a tree (H-tree, X-tree ... )
- Routing: Use horizontal and vertical wires to connect cells together. The router uses PDK information (thickness, pitch, width,vias) for each metal layer to do the routing. The Sky130 defines 6 routing layers. It doe global routing and detailed routing.
- Signoff: Involves physical verification like DRC and LVS and timing verification. Design Rule Checking or DRC ensures final layout honors all design rules and Layout versus Schematic or LVS ensures final layout matches the gate level netlist from synthesis phase. Timing verification ensures timing constraints are met.

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

- skywater-pdk: contains PDK files provided by foundry
- open_pdks: contains scripts to setup pdks for opensource tools
- sky130A: contains sky130 pdk files

#### Lab [Day 1] - Determine Flip-flop Ratio

- The task is to find the flip-flop ratio ratio for the design picorv32a. This is the ratio of the number of flip flops to the total number of cells. For the OpenLane installation, the steps are very straight forward and can be found on the OpenLane repo.

1. Invoking OpenLANE: Openlane can be invoked using docker command followed by opening an interactive session. flow.tcl is a script that specifies details for openLANE flow.

- Open the docker platform inside the openlane/
- flow.tcl -interactive: run script for automating the whole RTL to GDSII flow but in step by step -interactive mode
- package require openlane 0.9: retrives all dependencies for running v0.9 of OpenLANE

![Screenshot 2023-01-25 at 11 50 51 PM](https://user-images.githubusercontent.com/68071764/214909671-4920a450-4242-43e3-b7d6-b34d49e63e01.png)

![Screenshot 2023-01-26 at 11 51 09 AM](https://user-images.githubusercontent.com/68071764/214912956-4f22123b-4c33-4f5c-9aa8-391223472f25.png)


2. Design Preparation Step

- % prep -design picorv32a: Setup the filesystem where the OpenLANE tools can dump the outputs. This also creates a run/ folder inside the specific design directory which contains the command log files, results, and the reports dump by each tools. These folders will be empty for now except for lef files generated by this design setup stage. This merged the cell LEF files .lef and technology LEF files .tlef generating merged.nom.lef inside run/tmp/

![Screenshot 2023-01-26 at 12 25 38 PM](https://user-images.githubusercontent.com/68071764/214913686-946055f3-5717-4bc4-b01a-9a843298597c.png)

![Screenshot 2023-01-26 at 12 27 09 PM](https://user-images.githubusercontent.com/68071764/214913757-b0ced192-ddfa-47d5-927a-6f29fe0449ee.png)

- Since today's date is 26th January, a folder titled "26-01..." is created within the picorv32a directory:

![Screenshot 2023-01-26 at 12 36 45 PM](https://user-images.githubusercontent.com/68071764/214915539-4413a946-3450-4ed6-879c-9c672bef22d4.png)

- The merged file is created during the merging operation in the pircorv32a design preparation (it merges lef and techlef files). 

![Screenshot 2023-01-26 at 12 43 23 PM](https://user-images.githubusercontent.com/68071764/214915935-9d8f7fb3-0fe4-4fbb-8e5c-e8419ef2b1eb.png)

3.  Run synthesis:

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

- Set configuration variables. Before running floorplan stage, the configuration variables or switches must be configured first. The configuration variables are on openlane/configuration:

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

To run the picorv32a floorplan in openLANE:

run_floorplan
![Screenshot 2023-01-27 at 6 24 27 PM](https://user-images.githubusercontent.com/68071764/215095780-c33f67b6-b40e-4d0f-b56e-c98a73587660.png)

- The output of this stage is runs/[date]/results/floorplan/picorv32a.floorplan.def which is a design exchange format, containing the die area and positions.
- 
- ![Screenshot 2023-01-28 at 12 06 37 AM](https://user-images.githubusercontent.com/68071764/215167675-e19dabcf-132b-4285-adf5-58ebcbddf025.png)

- ![Screenshot 2023-01-27 at 11 35 48 PM](https://user-images.githubusercontent.com/68071764/215165947-8610cc63-b6e5-4749-a945-e2aab5d7306f.png)

- The die area here is in database units and 1 micron is equivalent to 1000 database units. Thus area of the die is (660685/1000)microns*(671405/1000)microns = 443587 microns squared.


