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
<details>
<summary>Click to view installation commands</summary>

```bash
# Update package lists
sudo apt-get update
# Install essential build tools and Yosys dependencies
sudo apt-get install -y build-essential clang bison flex \
libreadline-dev gawk tcl-dev libffi-dev git \
graphviz xdot pkg-config python3 libboost-system-dev \
libboost-python-dev libboost-filesystem-dev zlib1g-dev
# Clone the repository
git clone [https://github.com/YosysHQ/yosys.git](https://github.com/YosysHQ/yosys.git)
cd yosys
# Configure, compile, and install
make config-gcc
make
sudo make install
</details>

Installation Snapshot:

<img width="900" height="573" alt="image" src="https://github.com/user-attachments/assets/dbe929be-8b5a-4b33-86ba-03df44d3477b" />

3. Install Icarus Verilog (Iverilog)
Install Iverilog for simulation of Verilog designs.

<details>
<summary>Click to view installation commands</summary>

Bash

# Update package lists
sudo apt-get update
# Install Icarus Verilog
sudo apt-get install -y iverilog
</details>

Installation Snapshot:

<img width="1179" height="798" alt="image" src="https://github.com/user-attachments/assets/624e691d-8ab9-46b7-8370-a11745f622a7" />

4. Install GTKWave
Install GTKWave to view simulation waveforms.

<details>
<summary>Click to view installation commands</summary>

Bash

# Update package lists
sudo apt-get update
# Install GTKWave
sudo apt-get install -y gtkwave
</details>

Installation Snapshot:

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/c8a3ffa1-5280-41bf-a53a-a17593e9bfa6" />

5. Install ngspice
Install the ngspice circuit simulator.

<details>
<summary>Click to view installation commands</summary>

Bash

# Assumes you have downloaded the tarball (e.g., ngspice-37.tar.gz)
tar -zxvf ngspice-*.tar.gz
cd ngspice-*
# Create a build directory
mkdir release
cd release
# Configure, compile, and install
../configure --with-x --with-readline=yes --disable-debug
make
sudo make install
</details>

Installation Snapshot:

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/c28dca87-7c5f-4483-a5c4-c92ba41f5be5" />


6. Install Magic
Install the Magic VLSI layout tool.

<details>
<summary>Click to view installation commands</summary>

Bash

# Update package lists
sudo apt-get update
# Install dependencies
sudo apt-get install -y m4 tcsh csh libx11-dev tcl-dev tk-dev \
libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev
# Clone the repository
git clone [https://github.com/RTimothyEdwards/magic](https://github.com/RTimothyEdwards/magic)
cd magic
# Configure, compile, and install
./configure
make
sudo make install
</details>

Installation Snapshot:

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/e0323ed8-6d3f-4536-a569-d3409aedf73d" />


7. Install OpenLANE
Install the OpenLANE automated RTL to GDSII flow.

<details>
<summary>Click to view installation commands</summary>

Bash

# Update package lists and upgrade
sudo apt-get update && sudo apt-get upgrade
# Install dependencies
sudo apt install -y build-essential python3 python3-venv python3-pip make git
# Install Docker (required by OpenLANE)
sudo apt install -y docker-ce docker-ce-cli containerd.io
# Clone the repository
git clone [https://github.com/The-OpenROAD-Project/OpenLane](https://github.com/The-OpenROAD-Project/OpenLane)
cd OpenLane
# Build the OpenLANE environment using make
make
# Run the built-in test to verify the flow
make test
</details>

Installation Snapshot:

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/4dedda12-29c1-4e0f-b811-008086866dc3" />


8. Optional: OpenSTA
For timing analysis (not required for SFAL participants).

Install OpenSTA from the OpenROAD project repository.

<details>
<summary>Click to view installation commands</summary>

Bash

# Clone the repository
git clone [https://github.com/The-OpenROAD-Project/OpenSTA.git](https://github.com/The-OpenROAD-Project/OpenSTA.git)
cd OpenSTA
# Create a build directory
mkdir build
cd build
# Configure and build
cmake ..
make
</details>

Installation Flow
Setup VirtualBox + Ubuntu → Yosys → Iverilog → GTKWave → ngspice → Magic → OpenLANE → (Optional) OpenSTA
