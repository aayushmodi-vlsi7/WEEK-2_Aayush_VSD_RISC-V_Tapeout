
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

## Instructions

### Register Initialization

At the start of execution, the program sets up the working registers. The step size (r9) is defined as 1, the loop boundary (r10) as 43, the counter (r11) begins at 0, and the DAC accumulator (r17) is cleared to 0. This prepares the system for the upcoming sequence of operations.

### Accumulation Phase

The counter gradually increases from 0 towards the upper limit. With each increment, its value is added into r17. This creates a progressive growth in the DAC input register, forming the ramp-up behavior in the output signal.

### Maximum Accumulation

Once the counter reaches 43, the accumulator (r17) hits its highest point at 946. This marks the peak of the digital waveform before the descent begins.

### Descent and Oscillations

After the peak, the counter is reduced step by step. With every decrement, the accumulator is also reduced. This produces a controlled decline in r17, generating an oscillatory down-ramp until the counter value drops back to 1.

### Final Steady State

At the end of the sequence, the accumulator is adjusted once more, and the program enters an infinite loop. From this point onward, the DAC input remains stable, holding a constant output voltage.

## Execution Flow of the Program

1. **Initial Growth**  
   The counter starts small and gradually builds up. With every increment, the DAC register collects these values, causing a steady rise in its output. By the time the counter has stepped through 42 cycles, the register has accumulated close to 903.  

2. **Top of the Curve**  
   When the counter hits 43, the accumulation reaches its highest point. At this instant, the register value touches around 946, which represents the maximum in the entire sequence.  

3. **Falling Transition**  
   After the peak, the program begins reducing the counter instead of increasing it. Each subtraction reflects directly in the DAC register, which now begins to drop step by step, mirroring the earlier rise but in reverse.  

4. **Final Holding State**  
   Once the counter has come all the way down, the program no longer changes the value. The system remains in a repeating loop, leaving the DAC register frozen at its last steady number.

## Calculating DAC Analog Output

The DAC (Digital-to-Analog Converter) converts a digital value into a corresponding analog voltage. Specifically, for the digital register r17, the output voltage can be calculated as follows:

First, the digital value is normalized by dividing it by the maximum possible digital value (1023 for a 10-bit DAC). Then, it is scaled by the reference voltage span to obtain the analog voltage:

```math
V_{OUT} = \frac{r_{17}}{1023} \times V_{REF\_SPAN} \quad (\text{with } V_{REF\_SPAN} = 1.0\ \text{V})
```

This means that a higher digital value produces a proportionally higher output voltage, with the maximum output equal to the reference voltage span.

   | Digital Input ( r_{17} ) | DAC Output ( V_\text{OUT} ) |
| ------------------------ | --------------------------- |
| 903                      | 0.882 V                     |
| 946                      | 0.925 V                     |

## Summary

The VSDBabySoC project provided hands-on experience in building and simulating a small-scale System-on-Chip by integrating a RISC-V CPU, a PLL, and a DAC. I learned how these modules interact within a top-level design, how the DAC converts digital CPU outputs into analog signals, and how the PLL stabilizes the system clock. Converting TL-Verilog to standard Verilog emphasized the importance of tool compatibility, while running simulations with Icarus Verilog and analyzing waveforms in GTKWave reinforced the process of verifying functional correctness.

#### Key Learnings from this Week:

- Understanding the flow of data from CPU to DAC and mixed-signal interactions.

- Integration of multiple IP blocks into a cohesive SoC design.

- Importance of stable clock generation using PLLs for reliable CPU operation.

- Setting up testbenches and running pre-synthesis simulations to validate RTL.

- Analyzing simulation outputs and waveforms to verify system behavior.

- Familiarity with SoC design methodology, verification flow, and functional modeling.

