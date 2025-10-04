
<div align="center">
  
# Week 2 – BabySoC Fundamentals & Functional Modelling
</div>  

## Introduction
The **VSDBabySoC** is a simplified System-on-Chip that demonstrates how different types of hardware modules can be tied together to build a working chip.  
Although it is a small design, it captures the essence of SoC integration by bringing together:
- A processor (digital logic),
- A clock generator (timing/analog-inspired),
- And a digital-to-analog interface (mixed-signal).

This project helps in visualizing how these blocks interact with each other and provides a platform for practicing functional modeling and waveform analysis.

---

## Why this project is important
Modern SoCs combine many types of components — CPUs, timers, memory, and even analog circuits.  
VSDBabySoC is a scaled-down version that gives beginners exposure to:
- **Integration of multiple IPs** into one design,
- **Clocking techniques** using PLLs,
- **Mixed-signal interaction**, where digital outputs are converted into analog values,
- **Verification flow** using Verilog simulations and GTKWave.

By working with this project, I was able to see how different pieces of hardware “talk” to each other in simulation.

---

## Components of the SoC

### 1. RVMyth (RISC-V CPU Core)
- A lightweight RISC-V pipeline that executes simple instructions.  
- Produces a 10-bit digital output bus.  
- Represents the *computational brain* of the SoC.

### 2. Phase-Locked Loop (PLL)
- Generates a stable and higher frequency clock signal.  
- Provides the CPU with its operating clock.  
- Illustrates how clock circuits are integrated with digital systems.

### 3. Digital-to-Analog Converter (DAC)
- Takes the 10-bit digital output from the CPU.  
- Converts it into an analog signal.  
- Acts as the *bridge* between the digital SoC world and analog interfacing.

---

## Integration in the Top-Level Design
The heart of the system is the **top module** (`vsdbabysoc.v`), which connects all the blocks together:
- The PLL’s clock drives the CPU.  
- The CPU executes instructions and sends data to the DAC.  
- The DAC transforms this data into an analog signal.  

This top-level integration reflects the real-world scenario where multiple IP blocks are stitched together on silicon to form a complete chip.

---

## Testbench & Simulation Flow
To verify the design:
1. The **testbench** provides input stimuli like reset and reference signals.  
2. The simulation runs through **Icarus Verilog (iverilog)** and **vvp**.  
3. Outputs are dumped into a `.vcd` file.  
4. GTKWave is used to view the waveforms.  

Through this, I observed:
- How the SoC reacts to reset,  
- How clock synchronization drives the logic,  
- And how data flows from CPU → DAC.

---

## Key Takeaways
- **Integration Skills**: Saw how CPU, PLL, and DAC are combined in one design.  
- **Clock Understanding**: Learned why a PLL-generated clock is important for stable CPU operation.  
- **Dataflow Insight**: Understood how processor outputs are captured by peripherals.  
- **Verification Practice**: Hands-on experience with compiling, simulating, and analyzing waveforms in GTKWave.  

## 1. Cloning the Repository
   To begin the setup, we first need to fetch the VSDBabySoC repository. This will provide the complete SoC source files, including the CPU (RVMyth), DAC, PLL, and testbench.
   ```bash
cd ~/VLSI
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC/
```
## 2. Converting TL-Verilog to Verilog
   Since the RVMyth processor is written in TL-Verilog (.tlv), it cannot be directly simulated. Therefore, we must convert it into standard Verilog (.v) using the SandPiper-SaaS tool. This step ensures compatibility with simulation tools like Icarus Verilog.
   ```bash
# Install tools
sudo apt update
sudo apt install python3-venv python3-pip

# Create virtual env
python3 -m venv sp_env
source sp_env/bin/activate

# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

## 3. Running Pre-Synthesis Simulation
   The pre-synthesis simulation step validates that the RTL behaves as expected before moving to synthesis. Here, we test the design only at the functional level, ignoring delays and physical constraints. This helps verify correct integration of CPU, PLL, and DAC within the SoC.
   ```bash
mkdir -p output/pre_synth_sim

iverilog -o output/pre_synth_sim/pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v

cd output/pre_synth_sim
./pre_synth_sim.out
```
## 4. Viewing Waveforms with GTKWave
   After simulation, the results are saved in a .vcd file, which can be analyzed using GTKWave. The waveform provides visual confirmation of the CPU execution, PLL clock generation, and DAC output transitions.
   ```bash
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```
<img width="2777" height="1618" alt="GTKwaveform" src="https://github.com/user-attachments/assets/0bfe6ac8-7342-46c7-92b7-2dd336b19d1f" />

The waveform shown above comes from the pre-synth simulation (pre_synth_sim.vcd) of the VSDBabySoC project.
It demonstrates the interaction between the CPU (RVMyth), the DAC, and the PLL.

#### 1. CPU Activity

- The signals CPU_dmem_addr, CPU_dmem_rd_data, and CPU_ld_data toggle as the CPU fetches and executes instructions.

- These transitions indicate instruction fetches, memory accesses, and data loads during program execution.

- The DAC input bus D[9:0] shows numeric values being written from the CPU output register into the DAC.

#### 2. DAC Output

- The DAC takes the 10-bit digital input (Dext[10:0] / D[9:0]) from the CPU and generates an analog-equivalent output (OUT).

- The waveform for OUT appears as a gradual ramp and fall pattern, consistent with the instruction sequence (ramp-up, peak, ramp-down).

- This validates that the DAC is correctly translating CPU output into the analog domain.

#### 3. PLL Behavior

- The PLL-related signals (VCO_IN, period, refpd) show how the phase-locked loop aligns its frequency with the reference clock.

- At the beginning, there are small fluctuations as the PLL locks to the reference.

- After a short settling time, the PLL stabilizes and provides a steady clock (CLK) to the CPU.

- The value of period ≈ 35 confirms the generated clock frequency, while refpd ≈ 283 corresponds to the phase difference measurements.

#### 4. System-Level View

- Together, these signals prove that the CPU is active, the DAC output responds correctly, and the PLL provides stable clocking.

- The simulation verifies functional correctness before synthesis:

- The CPU is able to fetch/execute instructions.

- The DAC output follows the expected waveform shape.

- The PLL successfully locks and stabilizes the system clock.

