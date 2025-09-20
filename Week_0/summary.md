1.  **C Code → Compile with GCC → Verified (O1)**
    * [cite_start]This is the initial high-level specification, often written as a **C model**, to define the chip's intended behavior[cite: 2, 3]. [cite_start]It's verified using a C testbench[cite: 2, 3].

2.  **RTL Design (Verilog/VHDL) → Verified (O2)**
    * [cite_start]The hardware is described using a Register Transfer Level (RTL) language like **Verilog**[cite: 2, 3]. [cite_start]This "soft copy" of the hardware is also verified to ensure it matches the C model's functionality[cite: 2, 3].

3.  **ASIC Synthesis → Gate-level Netlist/Macros/Analog IPs → Verified (O3)**
    * [cite_start]The RTL code is converted into a physical representation made of standard logic gates (**Gate-Level Netlist**), synthesized macros, and analog IPs[cite: 2]. [cite_start]The output of this stage is verified to be logically equivalent to the RTL design[cite: 2].

4.  **SoC Integration**
    * [cite_start]The synthesized components, including the processor, peripherals, and other IPs, are integrated together[cite: 2].

5.  **Physical Design → GDSII Layout → Fabrication**
    * [cite_start]This involves creating the actual physical layout of the chip through steps like **floorplanning, placement, and routing**[cite: 2]. [cite_start]The final output is a GDSII file, which is sent for manufacturing[cite: 2].

6.  **Post-Silicon Test → Run same C testbench → Verified (O4)**
    * [cite_start]After the chip is fabricated, the original C testbench is run on the physical silicon to confirm that the hardware works as intended in the real world[cite: 1, 2].

7.  **Applications**
    * [cite_start]The finished chip can be used in various applications, such as **smartwatches, Arduino boards, TV panels, and AC controllers**[cite: 1].

### Final Check

A crucial concept in this workflow is maintaining consistency across all stages. The behavior and output should match at every verification point:
[cite_start]**O1 == O2 == O3 == O4** [cite: 1]
