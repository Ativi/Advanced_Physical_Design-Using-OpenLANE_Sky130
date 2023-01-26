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
 - Design Preparation Step 
 - Review files after design prep and run synthesis
 - Steps to characterize synthesis results 

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



