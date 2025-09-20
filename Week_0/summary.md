# Digital VLSI SoC Design and Planning

## Task 1 – Chip Journey

**High-level flow (summary):**

1. **C Code → Compile with GCC → Verified (O1)**

   * The initial high-level specification is often written as a C model that defines the chip's intended behavior. It is verified using a C testbench.

2. **RTL Design (Verilog/VHDL) → Verified (O2)**

   * HDL (Register Transfer Level) code (Verilog or VHDL) describes the hardware. This RTL is verified to ensure it matches the C model's functionality.

3. **ASIC Synthesis → Gate-level Netlist / Macros / Analog IPs → Verified (O3)**

   * RTL is synthesized into a gate-level netlist and instantiated macros and analog IPs. The output is verified for logical equivalence with the RTL.

4. **SoC Integration**

   * Integrate synthesized components (processor, peripherals, IP blocks, buses, etc.) into the SoC.

5. **Physical Design → GDSII Layout → Fabrication**

   * Perform floorplanning, placement, routing, extraction and LVS/DRC. Generate final GDSII for fabrication.

6. **Post-Silicon Test → Run same C testbench → Verified (O4)**

   * After fabrication, run the original C testbench on silicon to confirm real-world functionality.

**Applications**

* The finished chip can be used in devices like smartwatches, Arduino boards, TV panels, AC controllers, and other embedded products.

**Final check / golden rule:**

* Maintain consistency across all verification points so the observed behavior matches at every stage:

```
O1 == O2 == O3 == O4
```

---

## Task 2 — Tool Installation and Setup

### 1. Setup Virtual Environment

* Install Oracle VirtualBox (or another virtualization solution).
* **Recommended VM specs:**

  * 6 GB RAM (minimum)
  * 50 GB HDD available
  * Ubuntu 20.04 LTS or newer
  * 4 vCPUs

---

### 2. Install Yosys (RTL synthesis)

* Clone the Yosys repository, install build dependencies, build and install.

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
git clone https://github.com/YosysHQ/yosys.git
cd yosys

# Configure, compile, and install
make config-gcc
make -j$(nproc)
sudo make install
```

</details>

**Installation snapshot:**

<img width="900" height="573" alt="image" src="https://github.com/user-attachments/assets/06330b80-f167-40f8-b04c-43de070881ca" />


---

### 3. Install Icarus Verilog (iverilog)

* Icarus Verilog is used for Verilog simulation.

<details>
<summary>Click to view installation commands</summary>

```bash
# Update package lists
sudo apt-get update

# Install Icarus Verilog
sudo apt-get install -y iverilog
```

</details>

**Installation snapshot:**

<img width="1179" height="798" alt="image" src="https://github.com/user-attachments/assets/9fe8f64b-fe7a-4d1f-a6ef-4bc977777b81" />


---

### 4. Install GTKWave

* GTKWave is used to view simulation waveforms.

<details>
<summary>Click to view installation commands</summary>

```bash
# Update package lists
sudo apt-get update

# Install GTKWave
sudo apt-get install -y gtkwave
```

</details>

**Installation snapshot:**

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/b9618561-4e39-44d2-b13b-7c2caa8fde4e" />


---

### 5. Install ngspice

* ngspice is a circuit simulator used for analog simulations.

<details>
<summary>Click to view installation commands</summary>

```bash
# If using a packaged tarball (example: ngspice-37.tar.gz):
# Extract the tarball
tar -zxvf ngspice-*.tar.gz
cd ngspice-*

# Create a build directory
mkdir release
cd release

# Configure, compile, and install
../configure --with-x --with-readline=yes --disable-debug
make -j$(nproc)
sudo make install
```

</details>

**Installation snapshot:**

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/9d5d35b4-0f98-4f22-90e1-541b9819a0c0" />


---

### 6. Install Magic (VLSI layout)

* Magic is an open-source VLSI layout tool.

<details>
<summary>Click to view installation commands</summary>

```bash
# Update package lists
sudo apt-get update

# Install dependencies
sudo apt-get install -y m4 tcsh csh libx11-dev tcl-dev tk-dev \
  libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev

# Clone the repository
git clone https://github.com/RTimothyEdwards/magic.git
cd magic

# Configure, compile, and install
./configure
make -j$(nproc)
sudo make install
```

</details>

**Installation snapshot:**

<img width="898" height="576" alt="image" src="https://github.com/user-attachments/assets/429d4752-645c-4799-ac1d-f9aac7f89522" />



---

### 7. Install OpenLANE (OpenLane) — RTL-to-GDS flow

* OpenLane provides an automated open-source flow from RTL to GDSII. It requires Docker.

<details>
<summary>Click to view installation commands</summary>

```bash
# Update package lists and upgrade
sudo apt-get update && sudo apt-get upgrade -y

# Install base dependencies
sudo apt install -y build-essential python3 python3-venv python3-pip make git

# Install Docker (follow Docker official instructions for your distro)
# Example (Ubuntu):
sudo apt-get install -y docker.io
sudo systemctl enable --now docker

# Clone the OpenLane repository
git clone https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane

# Build the OpenLane environment (this may take a while)
make

# (Optional) Run the built-in tests to verify the flow
make test
```

</details>

---

### 8. Optional: OpenSTA (Static Timing Analysis)

* OpenSTA is used for timing analysis and is part of the OpenROAD project tools.

<details>
<summary>Click to view installation commands</summary>

```bash
# Clone the repository
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA

# Create a build directory
mkdir build
cd build

# Configure and build
cmake ..
make -j$(nproc)
```

</details>

---

## Installation flow (recommended order)

1. Setup VirtualBox and Ubuntu VM
2. Yosys (synthesis)
3. Icarus Verilog (simulation)
4. GTKWave (waveform viewer)
5. ngspice (analog simulation)
6. Magic (layout)
7. OpenLane (RTL → GDSII flow)
8. (Optional) OpenSTA (timing analysis)

---

## Verification checklist (after each stage)

* **O1:** C model passes testbench.
* **O2:** RTL simulation matches the C model.
* **O3:** Gate-level netlist (post-synthesis) matches RTL behavior (logical equivalence).
* **O4:** Post-silicon tests run the same C testbench and pass on silicon.

---

