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
4. **GTKWave** is used to view the waveforms.  

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

---

## Conclusion
Even though the VSDBabySoC is small and educational in nature, it captures the essence of real SoCs.  
It not only demonstrates digital processing but also shows how clock generation and analog interfacing can be part of the same chip.  
By simulating this project, I built confidence in understanding SoC fundamentals and functional verification techniques.

