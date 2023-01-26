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

# Introduction to QFN-48 Package, chip, pads, core, die and IPs

- *<b>Pads</b>* surround the rectangular metal patches where external bonds are made. input,output and power pad cells are commonly used pad cells.
- *<b>Core</b>* is the section of the chip where the fundamental logic of the design is placed.
- *<b>Die</b>* contains the core, it is the small semiconductor material specimen on which the fundamental circuit is fabricated. IC's are fabricated on a single 9 inch or 12 inch diameter silicon wafer, which contains hundreds of mirror images of the fundamental logic. This wafer is then cut into small pieces, each piece has similar functionality of the fundamental logic called Die.

![chip](https://user-images.githubusercontent.com/68071764/214861155-b7655dd6-af78-4296-9bb2-2edfc54286d6.png)

- *<b>Foundry</b>* PLLs, DAC, ADC and SRAMs come under the category of foundry IP. They have to be manually designed or need some human interference (or intelligence) essentially to define and create them. IPs(Intellectual property) are targeted on a particular technology nodes. Macros are basically digital blocks which are made up of purely digital logic.

![chip_info](https://user-images.githubusercontent.com/68071764/214861171-7840ba80-8154-4654-a3ce-eda93eaf844f.png)


  
      
