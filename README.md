# A FAULT-TOLERANT HYBRID BOOTH–WALLACE TREE MULTIPLIER 

## ABSTRACT

Multiplication is a fundamental operation in digital systems, widely used in DSP, image processing, machine learning, and microprocessors. This project proposes a high-speed, fault-tolerant multiplier using a hybrid Radix-4 Booth encoder and Wallace tree architecture. Booth encoding reduces the number of partial products, while the Wallace tree enables fast parallel reduction using carry-save adders, improving speed and reducing delay.

To enhance reliability, concurrent error detection and correction is incorporated using redundant logic. The design is implemented in Verilog HDL and verified through simulation and fault-injection testing. Results demonstrate improved speed, reduced complexity, and enhanced fault tolerance compared to conventional multiplier architectures.

---

# INTRODUCTION

Multiplication is one of the most fundamental arithmetic operations in digital systems and is extensively used in applications such as digital signal processing, image processing, machine learning accelerators, cryptographic hardware, and embedded systems. Since multiplication is performed repeatedly in these applications, the speed, hardware efficiency, and reliability of the multiplier significantly influence the overall system performance.

Conventional multiplier architectures, such as array multipliers, generate and accumulate partial products sequentially, which leads to higher propagation delay and increased hardware complexity as the operand size increases. To overcome these limitations, advanced multiplication techniques are employed to reduce the number of partial products and accelerate their reduction.

This project presents a Hybrid Booth–Wallace Tree Multiplier with Concurrent Error Detection and Correction. The proposed architecture combines two efficient multiplication techniques to achieve high-speed operation:

- Radix-4 Booth Encoding reduces the number of partial products by recoding the multiplier bits.
- Wallace Tree Reduction compresses the generated partial products in parallel using carry-save adders.

The combination of these techniques significantly reduces the critical path delay and improves multiplication speed compared to conventional multiplier designs.

To enhance reliability, the multiplier incorporates a fault-tolerant mechanism with concurrent error detection and correction. This circuitry continuously monitors the computation process, detects faults caused by transient disturbances, process variations, or hardware degradation, and automatically corrects erroneous outputs to ensure accurate operation.

The complete architecture is modeled in Verilog HDL and verified through functional simulation and fault-injection testing. The results demonstrate that the proposed design achieves high-speed multiplication, reduced hardware complexity, and improved fault tolerance, making it well suited for high-performance and reliable digital systems.

---

# WORKING OF PROPOSED HYBRID MULTIPLIER

## 1. Working of Booth Multiplier

The Booth multiplier is a high-speed multiplication technique used to reduce the number of partial products generated during binary multiplication. In a conventional multiplier, one partial product is generated for each bit of the multiplier. For an N-bit multiplication, this results in N partial products. Radix-4 Booth encoding processes two multiplier bits at a time, reducing the number of partial products to approximately N/2.

After encoding, the corresponding partial products are generated and shifted according to their bit positions. For a 16-bit multiplication, only eight partial products are produced instead of sixteen, which significantly reduces the hardware required in the next stage.

In the proposed hybrid multiplier, the Booth encoder serves as the first stage and generates a reduced set of partial products. This reduction lowers the computational complexity and improves the overall multiplication speed.

In this method, the multiplier bits are grouped into overlapping sets of three bits of the form `(y₂ᵢ₊₁, y₂ᵢ, y₂ᵢ₋₁)`, where the least significant previous bit is assumed to be zero. Each group is decoded to determine whether the multiplicand M should be multiplied by 0, +1, −1, +2, or −2.

Multiplication by two is achieved by a one-bit left shift, while negative multiples are generated using two’s complement representation.

![Working of Booth Multiplier](images/booth_multiplier.png)

---

## 2. Working of Wallace Tree Multiplier

The Wallace tree multiplier is a partial-product reduction method that helps in adding the partial products produced during the multiplication process. In contrast to traditional multipliers that add the partial products sequentially, the Wallace tree adds them in parallel through the use of full adders and half adders. This enables the multiplication process to take place much faster as there is a drastic reduction in the propagation delay.

In the Wallace tree, bits of the same weight from each row of the partial product are placed in a column. Whenever there are three bits in one column, they are reduced through the use of a full adder into one sum bit and one carry bit. Where there are two bits, a half adder can be used. This process continues stage by stage until only two rows are left.

These two rows, carrying the output of the sum and carry, are finally added using the carry propagate adder.

---

## 3. Working of Hybrid Booth-Wallace Tree Multiplier

The proposed multiplier combines the advantages of Radix-4 Booth encoding and Wallace tree reduction to achieve high-speed and hardware-efficient multiplication. The overall architecture consists of four main stages: Booth encoding, partial product generation, Wallace tree reduction, and final addition.

Initially, the multiplicand and multiplier are applied to the Radix-4 Booth encoder. The multiplier bits are grouped into overlapping sets of three bits, and each group is decoded to determine whether the multiplicand should be multiplied by 0, +M, −M, +2M, or −2M.

These partial products are then aligned according to their bit positions and supplied to the Wallace tree reduction stage. The Wallace tree compresses multiple partial-product rows in parallel using full adders and half adders until only two rows remain: a sum row and a carry row.

The final two rows are added using a carry-propagate adder to generate the multiplication result.

In the proposed 16-bit design, Booth encoding reduces the number of partial products from sixteen to eight.

![Hybrid Booth-Wallace Tree Multiplier](images/hybrid_booth_wallace.png)

---

## 4. Working of Fault Detection and Correction Mechanism

To improve the reliability of the proposed multiplier, a concurrent error detection and correction mechanism is integrated with the Hybrid Booth–Wallace Tree architecture.

After the final product is generated by the Booth–Wallace multiplier, a redundant checking circuit independently verifies the result. If a mismatch is detected, an error signal is asserted, indicating that one or more bits of the product may be corrupted due to transient faults, noise, or hardware defects.

The correction logic then uses the recomputed result from the redundant path to replace the faulty output with the correct product.

![Fault Detection and Correction Mechanism](images/fault_detection_correction.png)

---

# METHODOLOGY

The designed Fault-Tolerant Hybrid Booth-Wallace Tree Multiplier with Concurrent Error Detection and Correction system has been designed using Verilog HDL technology and has been verified using Cadence EDA tool.

## (i) RTL Design and Module Description

### booth_encoder_16.v

This module implements the Booth encoding logic and reduces the number of partial products generated during multiplication.

### partial_product_gen.v

This module generates the partial products according to the Booth encoder outputs.

### wallace_tree_16.v

This module compresses the partial products in parallel using full adders and half adders arranged in a Wallace tree structure.

### final_adder.v

This module adds the final sum and carry rows obtained from the Wallace tree to produce the 32-bit multiplication result.

### majority_voter.v

This module implements the fault correction logic and selects the correct output using majority voting.

### hybrid_booth_wallace_16.v

This is the top-level module that integrates the Booth encoder, partial product generator, Wallace tree, final adder, and majority voter into a complete 16-bit fault-tolerant multiplier.

### hybrid_booth_wallace_16_ft.v

This file contains the fault-tolerant simulation model used to verify the error detection and correction mechanism under injected fault conditions.

### tb_hybrid_multiplier.v

This is the testbench used to verify the functionality of the top-level design.

---

## (ii) Functional Simulation

Simulation of functionality was done using the Cadence Xcelium simulator. Different combinations of signed 16-bit numbers were used to verify multiplication functionality.

![RTL Simulation Results](images/rtl_simulation.png)

---
## (iii) RTL Verification

Verification in SimVision was performed to ensure that the generated product was consistent with the correct multiplication results for each test case.

![RTL Verification](images/rtl_verification.png)

---

## (iv) Logic Synthesis

The synthesis of the hybrid Booth Wallace multiplier was performed using the Cadence Genus tool. The RTL code was mapped to the gate-level netlist using the target standard-cell library.

![Logic Synthesis](images/logic_synthesis.png)

---

## (v) Import into Innovus

The synthesized gate-level netlist was imported into Cadence Innovus for physical implementation. The required technology files and constraints were loaded to prepare the design for layout generation.

---

## (vi) Floorplanning

The core area and aspect ratio were defined, and input/output pins were placed around the core boundary. VDD/VSS power rings and stripes were created for reliable power distribution.

![Floorplanning](images/floorplan.png)

---

## (vii) Placement

Standard cells were automatically arranged within the core area and optimized to reduce wire length, improve timing, and minimize congestion.

![Placement](images/placement.png)

---

## (viii) Routing

Global routing and detailed routing were performed to connect all signal nets using the available metal layers.

![Routing](images/routing.png)

---

## (ix) Physical Verification

After routing, the design was subjected to Design Rule Check (DRC), Layout Versus Schematic (LVS), connectivity verification, and parasitic extraction.

---

## (x) Performance Evaluation

The final design was evaluated using functional simulation waveforms, synthesis reports, physical layout results, and the observed behavior of the fault detection and correction mechanism.

---

# EXPERIMENTAL SETUP

## a. RTL Simulation Setup

**Tools Used**

- Cadence Xcelium – Verilog compilation and simulation
- Cadence SimVision – Waveform viewing and debugging

**Input Files**

- booth_encoder_16.v
- partial_product_gen.v
- wallace_tree_16.v
- final_adder.v
- majority_voter.v
- hybrid_booth_wallace_16.v
- hybrid_booth_wallace_16_ft.v
- tb_hybrid_multiplier.v

---

## b. Logic Synthesis Setup

**Tool Used**

- Cadence Genus Synthesis Solution

**Input Files**

- Verilog RTL files
- Technology library (.lib)
- Synthesis constraints file (constraints.sdc)
- Genus TCL script (run_genus.tcl)

---

## c. Physical Design Setup

**Tool Used**

- Cadence Innovus Implementation System

**Input Files**

- Synthesized gate-level netlist
- Timing constraints file (.sdc)
- Technology LEF file
- Standard cell LEF file
- Liberty timing library
- MMMC file
- Innovus run script

---

# RESULTS

## 1. RTL Simulation Results

Functional simulation was performed using Cadence Xcelium with the testbench `tb_hybrid_multiplier.v`.

Various combinations of positive and negative 16-bit signed inputs were applied to verify multiplication functionality. Faults were intentionally injected to validate the fault detection and correction mechanism.

The following input combinations were applied:

1. 10 × 5
2. −12 × 6
3. 25 × −4
4. −15 × −3
5. 32767 × 2
6. −32768 × 1

![RTL Simulation Waveform](images/rtl_simulation_results.png)

---

## 2. Synthesis Results

The verified RTL design was synthesized using Cadence Genus. The synthesis process successfully mapped the design to the target standard-cell library and generated the gate-level netlist.

The synthesis summary reported:

- 808 standard cells
- 1372 nets
- 97 ports

![Synthesis Results](images/synthesis_results.png)

![Synthesized Gate-Level Implementation](images/synthesized_gate_level.png)

---

## 3. Physical Design Results

The synthesized netlist was imported into Cadence Innovus for floorplanning, placement, and routing.

The complete physical design flow was executed successfully, resulting in the final routed layout.

![Physical Design Results](images/physical_design_results.png)

---

# CONCLUSION

In this project, a Fault-Tolerant Hybrid Booth–Wallace Tree Multiplier with Concurrent Error Detection and Correction was successfully designed and implemented using Verilog HDL.

The proposed architecture combines Radix-4 Booth encoding and Wallace tree reduction to achieve high-speed signed multiplication while reducing propagation delay and hardware complexity.

The complete design was verified through RTL simulation using Cadence Xcelium and SimVision, synthesized using Cadence Genus, and physically implemented in Cadence Innovus through floorplanning, placement, and routing.

Physical design results showed successful routing with zero DRC violations, no congestion overflow, and approximately 49.9% placement density. During timing analysis, WNS and TNS were reported as 0 ns.

Overall, the proposed architecture achieves an effective balance between performance, area efficiency, and reliability, making it suitable for DSP processors, AI accelerators, cryptographic systems, embedded hardware, and other safety-critical VLSI applications.

---

# REFERENCES

1. A. D. Booth, “A Signed Binary Multiplication Technique,” Quarterly Journal of Mechanics and Applied Mathematics, vol. 4, no. 2, pp. 236–240, 1951.

2. C. S. Wallace, “A Suggestion for a Fast Multiplier,” IEEE Transactions on Electronic Computers, vol. EC-13, no. 1, pp. 14–17, 1964.

3. L. Dadda, “Some Schemes for Parallel Multipliers,” Alta Frequenza, vol. 34, pp. 349–356, 1965.

4. V. Dharani et al., “Booth Multiplier: A Systematic Study,” in Lecture Notes in Electrical Engineering, 2021.

5. “Power and Area-Efficient Multiplier Architectures: A Comparative Study of Array, Dadda, Booth, Wallace Tree, and Vedic Multipliers,” IEEE Conference Publication, 2024.

6. D. Umesh and S. G. Nayak, “High-Speed Signed 8-bit Multiplier Using Booth Encoding and Dadda Tree Architecture,” Proceedings of IEEE CONIT, 2025.

7. A. Biswas and J. Jin, “Implementation of an 8-bit Wallace Tree Multiplier,” 2025.

8. Priyanka C, Chiranth C. Shet, Darshan M. R., et al., “A Reconfigurable Precision Hybrid Booth-Encoded Wallace–Dadda Multiplier for Energy-Efficient DSP and AI Hardware,” International Journal of Engineering Research & Technology (IJERT), vol. 15, no. 4, pp. 1–4, 2026.
