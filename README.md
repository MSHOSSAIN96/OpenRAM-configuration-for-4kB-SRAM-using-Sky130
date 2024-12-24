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


![Screenshot 2024-12-24 165819](https://github.com/user-attachments/assets/247edf23-d829-4028-bab0-45cddf62a803)


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


**OpenRAM SRAM Architecture**

![sram_arch](https://github.com/user-attachments/assets/98dc6e05-9601-43fb-8c2d-9562f231cef9)

The diagram highlights the essential building blocks of the SRAM architecture designed using OpenRAM:

Bitcells: The smallest storage unit that holds a single bit of data.

Address Decoders: Decode input addresses to select the appropriate wordline in the memory.

Wordline Drivers: Drive the decoded address signals to activate specific rows in the bitcell array.

Precharge for Bitlines: Prepares the bitlines before read or write operations to improve speed and reliability.

Column Multiplexer: Selects specific columns from the bitcell array for read or write operations.

Write Drivers: Drive data signals into the selected memory cells during a write operation.

Sense Amplifiers: Amplify small signals from the bitlines during a read operation for accurate data retrieval.

Control Logic: Coordinates the overall operation of the SRAM, including timing and control signals.

**Configurability**

OpenRAM is configurable for any technology, making it versatile for different fabrication processes.

**Supported Technologies**

Currently, OpenRAM supports two main technologies:

FreePDK45: A 45nm non-fabricable process used for academic and simulation purposes.

SCMOS: A 25µm fabricable process.

**Configuring for SKY130 Technology**

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

**Environmental Setup**

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


**Bit-cells and sense amplifier details**


![Screenshot 2024-12-24 152804](https://github.com/user-attachments/assets/c458f992-d159-4717-b4ff-662c7aff356c)



**Required Cells:** These are the essential building blocks for OpenRAM:

**6-Transistor Bitcell (6T Bitcell):**

1.Includes dummy and replica bitcells for testing and stability.

2.Serves as the core storage unit.

**Sense Amplifier:**

Used to detect and amplify small voltage differences during read operations.

**D-Flip-Flop:**

A memory element used for temporary data storage and synchronization.

**Write Driver:**

Facilitates writing data into the bitcell by driving appropriate voltages.

**Tristate:**

Allows multiple outputs to connect to a single line without interference.

**Layout and SPICE**

Layout: Refers to the physical arrangement of components on silicon.

SPICE: Simulations are required for all custom cells to ensure they meet design specifications and functionality.

**Other Custom Cells (Optional)**

Additional cells that can be included for extended functionality:

NAND Gate: A basic logic gate for Boolean operations.

NOR Gate: Another basic logic gate.

Precharge Circuit: Prepares bitlines to a known voltage state before read/write operations.

Boundary Layer

Custom cells must have a defined boundary layer:

Specifies the physical area and dimensions of the cell.

Ensures compatibility with the overall design flow.

**Key Note**

OpenRAM is highly sensitive to:

Cell names: Consistent naming is critical for recognition.

Pin names: Proper identification is necessary for connections and functionality.

This ensures the accurate functionality of OpenRAM as a memory compiler.


Bit Cell (6T SRAM Cell): 6 MOSFET's SRAM cell. (2 P-MOS & 4 N-MOS)
It was designed by the cross coupling to CMOS inverters. 

![Screenshot 2024-12-24 155234](https://github.com/user-attachments/assets/39cec352-ec21-48c4-9063-d35dc9100134)


**Introduction to the 6T SRAM Cell:**

The 6T SRAM cell is a basic building block for Static Random Access Memory. It consists of six transistors, with the following functionality:

Two cross-coupled inverters (M1, M2, M3, and M4): These form a bistable latch, capable of holding a single bit (0 or 1).

Two access transistors (M5 and M6): These enable interaction with the latch during read or write operations by connecting the cell to the bitlines (BL and BR).

**Key Points from the Diagram**

**Cell Name: The naming conventions are critical in OpenRAM to indicate the functionality and port configuration:**

cell_1rw: Has one read/write port.

cell_1w_1r: Includes one write port and one read port.

cell_1rw_1r: Includes one read/write port and one additional read port.

Pin Names

bl (Bitline Left) & br (Bitline Right): Used for data transfer during read/write operations.

wl (Wordline): Activates the access transistors to enable read or write access.

vdd: Supplies power to the cell.

gnd: Ground connection.

Functionality Breakdown

**Storage Nodes:**

Q and Q̅ are the two complementary storage nodes.

They hold the logic states (0 and 1) within the latch.

**Access Mechanism:**

The wordline (wl) activates transistors M5 and M6, allowing data on the bitlines (bl, br) to be written to or read from the storage nodes (Q, Q̅).

**Important Notes**

The layout and naming of the cell are crucial for integration into the design flow of OpenRAM.

Properly defined naming conventions depend on the number of ports (read/write or combined).

This structured format provides a clear and concise explanation of the bitcell and its operation as shown in the figure.


![Screenshot 2024-12-24 161059](https://github.com/user-attachments/assets/d3a9f3f4-b850-4f52-9c50-f2f4cc65a183)

Simulation Overview

This simulation demonstrates the read and write operations of a 6T SRAM cell, conducted using a SPICE tool. The waveforms show the relationship between the control signals (like the wordline) and the internal storage/output nodes during these operations.

Key Observations:

Wordline Activation (WL):

When the wordline (WL) signal goes high, it enables the access transistors (M5 and M6), allowing interaction between the internal storage nodes (Q and Q̅) and the bitlines (BL and BR).

Read Operation:

During a read operation:

The stored value at node Q is reflected on one of the bitlines (BL or BR).

The complementary value (Q̅) reflects on the opposite bitline.

The sense amplifier detects this small voltage difference to determine the stored bit.

Write Operation:

During a write operation:

The wordline is activated, and the new data is driven onto the bitlines (BL and BR).

The access transistors propagate the bitline values to the storage nodes (Q and Q̅), overwriting the previously stored bit.

**Figure Explanation**

The figure shows the following waveforms:

Wordline (WL): The green signal indicates when the wordline is activated.

Bitlines (BL and BR): Represented by blue and yellow signals, they carry the data during read and write operations.

Internal Nodes (Q and Q̅): The storage nodes hold complementary logic values, as shown by the red and purple signals.

Operation Summary with Waveforms

When WL is High (Active):

The internal storage (Q and Q̅) values are reflected on the bitlines (BL and BR).

The data can either be read or overwritten, depending on the bitline input.

When WL is Low (Inactive):

The access transistors disconnect the storage nodes from the bitlines.

The data stored in Q and Q̅ remains unchanged.

This figure effectively illustrates the behavior of a 6T SRAM cell during read and write operations, emphasizing the role of the wordline and bitlines in enabling or isolating the storage nodes.


![Screenshot 2024-12-24 161751](https://github.com/user-attachments/assets/4b193677-b1c3-4961-9124-41a2527a0631)

In this simulation of the circuit's behavior, I observe the interaction between the word line signals (high or low) and the corresponding data transfer processes:

Word Line Activation:

When the word line signal goes high (red trace), the logic values on the large horizontal lines (e.g., blue, orange) are transferred to the smaller, specific nodes or "capelin letters" (represented by the corresponding signals below).

Logical Transfer:

Specifically, the logic present on the larger data lines is transferred to the output "Q" node.

Similarly, the logic is transferred to another node "Cuba" (green trace), which responds to the previous state when its line activates.

Retention and Transition:

When the word line becomes inactive (low), the previously stored data on "Q" and "Cuba" is held until the next activation cycle. This reflects the role of storage elements or latches in maintaining state.
Observation of Timing:

The figure demonstrates the correct timing relationships between the word line, data lines, and their transfer to output nodes. The data line logic only updates the outputs when the controlling signal (word line) is enabled.

This simulation illustrates the fundamental working principle of memory storage elements or sequential circuits where a word line acts as a gate to control data flow and retention.


**Sense Amplifier:**

Generally a differentiate volt amplifier. 

![Screenshot 2024-12-24 162843](https://github.com/user-attachments/assets/c4013663-c8ce-45a6-9113-b2697f9532af)


 .bI<blbar then the output is 0.

 ![Screenshot 2024-12-24 163129](https://github.com/user-attachments/assets/d011f465-1875-467c-9aeb-117c2c017d73)

**Sense Amplifier Circuit Description**

The circuit shown is a sense amplifier designed for memory circuits, primarily used to amplify the small voltage difference between bitlines (BL and BLBar) during a read operation. It is a current-mode design, and its behavior is described as follows:

**Circuit Operation**

**Inputs and Initialization:**

BLBar (Bit Line Bar) and BL (Bit Line) represent the differential input signals.

The read enable signal (Rd_EN) activates the circuit for operation.

**Logic Conditions:**

If BLBar = 1 and BL = 0:

Transistor M1 switches ON, pulling the intermediate node connected to it to 0.

This intermediate node's logic propagates to the output node through the subsequent stages, generating the final output Dout = 0.

If BLBar = 0 and BL = 1:

Transistor M3 switches ON, pulling the connected node to 1.

This logic propagates through the circuit, eventually resulting in Dout = 1.

Intermediate Nodes:

These nodes provide transitional values that are processed and stabilized by the sense amplifier. The outputs are derived from these nodes, which depend on the transistor switching logic.

Output Inversion:

An inverter stage is included to provide the desired final output logic level based on the intermediate states.

Design Details:

The transistor sizes (e.g., W/L ratios like 0.42u/0.21u) are critical to ensure proper current control and switching characteristics. Adjusting these sizes can tune the performance of the amplifier.

The amplifier amplifies the small differential voltage between the bitlines into a full logic level for the output.

Simulation Insights:
When the word line is high, the sense amplifier is activated, and it amplifies the voltage difference between BL and BLBar, enabling accurate data reading.

For instance:

If BLBar = 0 and BL = 1, the sense amplifier output is a logic 1.

If BLBar = 1 and BL = 0, the sense amplifier output is a logic 0.

This ensures robust and reliable data sensing in memory circuits.

Key Points:

The sense amplifier must be enabled at the correct time (via the word line and Rd_EN signals).

Proper transistor sizing ensures accurate amplification of differential signals.

The circuit’s ability to handle small voltage differences highlights its role as a critical component in memory operations.





**Dummy & replica bit cells, D-flipflop and write driver details**

![Screenshot 2024-12-24 164315](https://github.com/user-attachments/assets/395b8aee-a729-4763-aa7d-4341f8380239)


Overview of Components and Operations

Disconnected Cells for Regularity:

Disconnected cells are introduced for regularity in the design.

A replica circuit mimics the behavior of the actual sense amplifier and helps in timing control.

The output of the replica circuit is designed to go to 0 and is used to enable the sense amplifier at the correct time.

Flip-Flop Design

Positive Edge-Triggered Flip-Flop:

The flip-flop is positive edge-triggered and is constructed using:

Transmission gates

Inverters

Why Transmission Gates?

They function as switches with very low ON resistance and almost infinite OFF resistance, making them efficient for such designs.

Structure of the Flip-Flop:

It comprises two latches:

First latch: Negative level-sensitive.

Second latch: Positive level-sensitive.

Together, they ensure reliable data storage during positive clock transitions.

Layout Details:

The flip-flop layout showcases:

A new generation of blocks.

Transmission gates.

Inverters.

The arrangement is optimized for area and performance.

Write Driver Circuit

Write Driver Design:

The write driver circuit ensures proper logic levels are written to memory cells during write operations.

Components:

Two NOR gates.

Inverters.

Transistors.

Operation During Write:

The write driver handles scenarios like:

When DIN (data input) is high, corresponding transistors are activated to propagate the appropriate logic.

When DIN is low, other transistors activate to ensure correct output.

Write Process:

The write driver prepares the bitlines by driving them to the correct logic levels before enabling access.

This ensures data is correctly written to the memory cells.

Simulations and Observations

Flip-Flop Simulations:

The data remains stable when the clock is low.

During a positive clock edge, the input data is captured and reflected on the output.

Write Driver Simulations:

Whenever the write enable (Wr_EN) is low:

The input data is correctly reflected to the output.

This operation ensures reliable data transfer during write operations.

Key Takeaways

Replica Circuit: Ensures the sense amplifier is enabled at the right time.

Flip-Flop: Uses transmission gates for efficient operation and combines two latches for proper functionality.

Write Driver: Drives bitlines to the required logic levels during write operations, ensuring data integrity.

Simulations: Validate the correct functioning of both flip-flop and write driver under different input and timing conditions.

This organized structure provides clarity on the role of each component and their behavior, supported by schematic and simulation observations.


**Chapter 03: OpenRAM Technology setup**


![Screenshot 2024-12-24 165819](https://github.com/user-attachments/assets/117b9ff5-ca9d-4de9-898b-6bf750bc7085)






 












