# <b>OpenLane</b>
OpenLane is not a tool, it is basically a flow that comprises many opensource EDA tools which includes OpenRoad, Yosys, ABC, Fault, Qflow, Magic and a number of custom scripts for design exploration and optimization. Basic aim to have OpenLane is to have complete RTL to GDSII flow.

# Table of Contents
 DAY1: Inception of open-source EDA, OpenLANE and Sky130 PDK
 - Introduction to QFN-48 Package, chip, pads, core, die and IPs
 - Introduction to RISC-V
 - Introduction to all components of open-source digital asic design
 - Simplified RTL2GDS flow
 - Introduction to OpenLANE and Strive chipsets
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

-  *<b> Assembly language(Bridge between software and hardware)</b>*  Applications and softwares running in our laptops and PCs are written in a variety of languages like C, C++, Python and Java etc. These different languages cannot be directly processed and implemented by the hardware (Microprocessor Core). They need to be converted into form that is understandable by the core. This is where the ISA or the instruction set architecture (assembly language) comes into the picture. Special programs called the compiler and assembler are used to convert the instructions in different languages to the target assembly language. The compiler converts the code into assembly language, which in our case is the RISC-V ISA and the assembler takes care of converting the different instructions in textual format into binary representations which are understood by the microprocessor and executed. 
 
![APP](https://user-images.githubusercontent.com/68071764/214866200-680ae242-d5c2-414d-85f9-70351ae757ec.png)
