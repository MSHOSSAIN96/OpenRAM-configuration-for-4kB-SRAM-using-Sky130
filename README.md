# OpenRAM-configuration-for-4kB-SRAM-using-Sky130
OpenRAM is an open-source SRAM compiler. It is a completely Python-based framework, technology independent memory compiler. 

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



