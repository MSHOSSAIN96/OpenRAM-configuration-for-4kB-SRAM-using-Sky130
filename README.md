# OpenRAM-configuration-for-4kB-SRAM-using-Sky130
OpenRAM is an open-source SRAM compiler. It is a completely Python-based framework, technology independent memory compiler. 

Static Random-Access Memory (SRAM) has become a standard element of any Application Specific Integrated Circuit (ASIC), System-On-Chip (SoC), or other micro-architectures. For this wide variety of applications, SRAMs are configured using parameters like the word-length, bit lines, operating voltage, access time, and most importantly the technology node. The access time of an SRAM cell is the time require for a read or write operation of SRAM.

Manually configuring the SRAM for every change in parameter seems a slightly in-efficient and tedious task. Due to this reason, the memory compiler is used on a large scale, as it facilitates easy configuration and optimization of memory. OpenRAM, an open-source memory compiler is used for characterization and generation of SRAM designs.

This webinar mentioned multiple open-source circuit schematic design, layout design, SPICE simulations tools and memory compiler. The tools used are explained in detail. All the Skywater SKY130 PDKs related files are added to the repository mentioned in webinar, which can be used without installing the complete PDKs. To install or get other details of Skywater PDKs, it can be found in Skywater official website.

Main Objectives: 

    1.Configure open-source memory compiler OpenRAM for any memory size
    2.SRAM custom cell design
    3.Memory GDS/Lib/Lef file types

 Achievements:

     1.From here, I will discuss about various custom cells like 6T-SRAM, DFF
     2.I will configure open-source memory compiler OpenRAM for custom memory IP generation
     
**Chapter 1: Introduction**

Purpose: OpenRAM is a Python-based framework used to design and compile SRAM (Static Random-Access Memory).

Inputs:OpenRAM takes the following as input:

Custom Cells: User-defined memory cell configurations for customization.

Technology Files: Specifications like process design rules and parameters for the specific fabrication technology.

Configuration Files: User-defined settings for compiling memory blocks, such as memory size, type, and optimization goals.

**Processing Workflow**

The inputs flow into OpenRAM, which processes the data to generate memory designs. Key input components:

Design Rules: Guidelines for layout and spacing to ensure manufacturability.

Custom Cells: Predefined memory cell components for integration.

Config Scripts: Scripts specifying compilation and customization parameters.

Outputs:OpenRAM produces various deliverables, including:

Layouts: GDS II layout files for the physical representation of memory.

Netlists: Spice netlists for simulation and verification.

Verilog Models: High-level hardware description models for integration into digital designs.

Timing Models: Liberty (.lib) files to describe memory timing at various process corners.

P&R Models: LEF files (Layout Exchange Format) for place-and-route in physical design tools.

Datasheets: HTML-based documentation for the generated memory.

Logs: Compile logs for debugging and performance review.

Configuration Files: Python-based output configuration scripts.

IP Deliverables: The table on the right details specific formats for the output deliverables:

.gds: GDS II layout files for the physical memory design.

.sp: Spice netlists for circuit simulation.

.v: Verilog models for logic integration.

.lef: P&R abstract files for physical design tools.

.lib: Liberty files for timing characterization at multiple process corners.

.html: Datasheet providing details about the generated memory.

.log: Logs capturing details of the compile process.

.py: Python configuration files for reproducibility or modifications.


**1. OpenRAM SRAM Architecture**

The diagram highlights the essential building blocks of the SRAM architecture designed using OpenRAM:

Bitcells: The smallest storage unit that holds a single bit of data.

Address Decoders: Decode input addresses to select the appropriate wordline in the memory.

Wordline Drivers: Drive the decoded address signals to activate specific rows in the bitcell array.

Precharge for Bitlines: Prepares the bitlines before read or write operations to improve speed and reliability.

Column Multiplexer: Selects specific columns from the bitcell array for read or write operations.

Write Drivers: Drive data signals into the selected memory cells during a write operation.

Sense Amplifiers: Amplify small signals from the bitlines during a read operation for accurate data retrieval.

Control Logic: Coordinates the overall operation of the SRAM, including timing and control signals.

**2. Configurability**

OpenRAM is configurable for any technology, making it versatile for different fabrication processes.

**3. Supported Technologies**

Currently, OpenRAM supports two main technologies:

FreePDK45: A 45nm non-fabricable process used for academic and simulation purposes.

SCMOS: A 25µm fabricable process.

**4. Configuring for SKY130 Technology**

The next step in OpenRAM's development is to configure its SRAM architecture for the SkyWater 130nm process technology. This is an open PDK that supports fabrication.

**Reasons to use OpenRAM and Sky130 technology**

![Screenshot 2024-12-23 232752](https://github.com/user-attachments/assets/d7889257-2c6d-4a89-b9f2-59e0d72d717a)

The SKY130 process node includes:

1 level of local interconnect, which enables close connections between transistors.

5 levels of metal, providing flexibility for routing and signal interconnections across the chip.

Overall, the SKY130 platform is notable for being an open-source initiative, enabling designers and researchers worldwide to access semiconductor technology for free, fostering innovation and learning in chip design.

![Screenshot 2024-12-23 233821](https://github.com/user-attachments/assets/0c8ccbae-2207-4f1f-b4f3-069191a63d98)

The above image illustrates the process of designing SRAM cells using OpenRAM.

Understanding the Left Figure:

The figure on the left shows a single SRAM cell at the transistor level.

It consists of two individual transistors close to each other, with two additional transistors serving as access transistors.

Creating a 2x2 SRAM Array:

To build a 2x2 memory array, the single SRAM cell shown on the left needs to be replicated four times and arranged together.

This step involves aligning the cells carefully to maintain proper connectivity and spacing.

Extending to a 4x4 Memory Array:

To create a 4x4 memory array, the basic SRAM cell must be replicated 16 times.

This process highlights the complexity and precision required when manually arranging cells to build larger memories.

Challenges in Large Memory Design:

Alignment issues: When replicating cells, maintaining proper alignment for power (VDD) and ground (GND) lines is critical.

In the case of a 2x2 array, we notice that the VDD and GND lines are very close to each other, which needs to be corrected.

To fix this, rotating the cells in the second row by 180 degrees ensures the ground lines and power lines are perfectly aligned and spaced appropriately.

Spacing Considerations:

While arranging cells, the minimum spacing requirements between layers must be respected to avoid design rule violations.

These adjustments are tedious and error-prone when done manually, especially for larger memories.

Why Use OpenRAM:

OpenRAM simplifies this process by using a memory compiler.

The compiler takes design constraints as input (e.g., size, spacing) and automatically arranges the cells to generate the memory array as per the specified requirements.

This approach eliminates manual editing of each cell, saving time and reducing errors, particularly in large memory designs.

The explanation is now structured to flow logically, emphasizing the challenges and how OpenRAM addresses them efficiently.


**Chapter 2: Environmental Setup and custom cells description**

Environmental Setup

Install Python Dependencies: 
           
           1.Python 3.5 or higher
           2.Python numpy
           3.Python scipy

In the Linux shell, I have to be installed the following packages: 

                $ sudo apt install -y python3-pip
                $pip3 install numpy
                $pip3 install scipy
                
In order to get git clone, we need to clone official OpenRAM repository

              $ git clone https://github.com/VLSIDA/OpenRAM.git


Next the OpenRAM needs to decide some environment varaiables:

  1.OPENRAM_HOME   export OPENRAM_HOME = <path-to-compiler>
  2.OPENRAM_TECH   export OPENRAM_TECH = <path-to-technology-files>
  3.PYTHONPATH     export PYTHONPATH = "$PYTHONPATH:$OPENRAM_HOME"

Install Spice Simulator
         1. NGSPICE
      
Install Layout tools: 
         1.MAGIC Layout Tool
         2. KLayout

In order to check whether I have the required dependencies I can use

**python3 --version**

After completing clone, we will directly enter OpenRAM directly. 

To set environment variables, I need to edit bashrc file. 

In the bashrc file I need to export few paths for the OpenRAM Variables. 

![1](https://github.com/user-attachments/assets/7fbcd7ff-0bdb-4880-88c3-cb877a617dbc)


![Screenshot from 2024-12-24 19-26-48](https://github.com/user-attachments/assets/a3241203-1348-46d9-ba7b-dc889c410f61)


![Screenshot from 2024-12-24 19-26-34](https://github.com/user-attachments/assets/0ab2345a-fe8e-4e9a-a56f-7858cdf7e383)




  





