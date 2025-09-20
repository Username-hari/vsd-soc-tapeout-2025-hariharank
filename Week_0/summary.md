# Digital VLSI SOC Design and Planning

---

## Task 1 – Chip Journey

1.  **C Code → Compile with GCC → Verified (O1)**
    * This is the initial high-level specification, often written as a **C model**, to define the chip's intended behavior. It's verified using a C testbench.

2.  **RTL Design (Verilog/VHDL) → Verified (O2)**
    * The hardware is described using a Register Transfer Level (RTL) language like **Verilog**. This "soft copy" of the hardware is also verified to ensure it matches the C model's functionality.

3.  **ASIC Synthesis → Gate-level Netlist/Macros/Analog IPs → Verified (O3)**
    * The RTL code is converted into a physical representation made of standard logic gates (**Gate-Level Netlist**), synthesized macros, and analog IPs. The output of this stage is verified to be logically equivalent to the RTL design.

4.  **SoC Integration**
    * The synthesized components, including the processor, peripherals, and other IPs, are integrated together.

5.  **Physical Design → GDSII Layout → Fabrication**
    * This involves creating the actual physical layout of the chip through steps like **floorplanning, placement, and routing**. The final output is a GDSII file, which is sent for manufacturing.

6.  **Post-Silicon Test → Run same C testbench → Verified (O4)**
    * After the chip is fabricated, the original C testbench is run on the physical silicon to confirm that the hardware works as intended in the real world.

7.  **Applications**
    * The finished chip can be used in various applications, such as **smartwatches, Arduino boards, TV panels, and AC controllers**.

### Final Check

A crucial concept in this workflow is maintaining consistency across all stages. The behavior and output should match at every verification point:
**O1 == O2 == O3 == O4**

---

## Task 2 - Tool Installation and Setup

### 1. Setup Virtual Environment
* Install **Oracle VirtualBox**.
* Check system requirements:
    * 6 GB RAM
    * 50 GB HDD
    * Ubuntu 20.04+
    * 4 vCPUs

### 2. Install Yosys
* Clone the Yosys repository.
* Install required dependencies (build tools, libraries).
* Build and install Yosys for RTL synthesis.

**Installation Snapshot:**
* *(<img width="900" height="573" alt="image" src="https://github.com/user-attachments/assets/dbe929be-8b5a-4b33-86ba-03df44d3477b" />
)*.

### 3. Install Icarus Verilog (Iverilog)
* Install Iverilog for simulation of Verilog designs.

**Installation Snapshot:**
* *(<img width="1179" height="798" alt="image" src="https://github.com/user-attachments/assets/624e691d-8ab9-46b7-8370-a11745f622a7" />
)*.

### 4. Install GTKWave
* Install GTKWave to view simulation waveforms.

**Installation Snapshot:**
* *(<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/c8a3ffa1-5280-41bf-a53a-a17593e9bfa6" />
)*.

### 5. Optional: OpenSTA
* For timing analysis (not required for SFAL participants).
* Install OpenSTA from the OpenROAD project repository.


### Installation Flow
Setup VirtualBox + Ubuntu → Install Yosys → Install Iverilog → Install GTKWave → (Optional) OpenSTA
