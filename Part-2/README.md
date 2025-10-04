<div align="center">
  
# Week 2 – BabySoC Fundamentals & Functional Modelling
</div>  


## Overview
The **VSDBabySoC** is a small-scale System-on-Chip example that demonstrates how different building blocks of hardware can be connected together to form a functional SoC.  
The design brings together three main components: a RISC-V processor (RVMyth), a phase-locked loop for clock generation, and a simple digital-to-analog converter.  
Through this integration, the project highlights both digital and mixed-signal aspects of chip design.

---

## Why this project matters
- Shows how a CPU core can be combined with clock circuitry and an analog interface.  
- Provides hands-on practice with **Verilog-based SoC simulation**.  
- Acts as a learning model for verifying dataflow between digital logic and analog-style peripherals.  

---

## Core Components

### 🔹 RISC-V CPU (RVMyth)
- A simple RISC-V pipeline implementation.  
- Executes basic instructions and produces digital outputs.  
- The CPU’s 10-bit bus drives the DAC input.

### 🔹 Phase-Locked Loop (PLL)
- Generates a clean, high-frequency clock signal.  
- Feeds the CPU with the required operating clock.  
- Serves as an example of integrating analog-inspired modules inside digital designs.

### 🔹 Digital-to-Analog Converter (DAC)
- Converts the CPU’s 10-bit digital output into an analog equivalent.  
- Bridges the gap between digital processing and analog signal generation.  
- Helps demonstrate mixed-signal verification in simulation.

---

## The SoC Integration
The top-level module (`vsdbabysoc.v`) ties everything together:
- PLL provides the operating clock to the CPU.  
- CPU generates data and pushes it onto a digital bus.  
- DAC receives this data and produces the final analog waveform.  

This setup illustrates **how multiple IPs communicate within a single chip environment**.

---

## Simulation & Testbench
- A dedicated testbench drives reset, clock, and reference signals.  
- Simulation outputs are dumped into `.vcd` files.  
- GTKWave is used to study the SoC behavior in terms of:
  - Reset activity  
  - Clock synchronization  
  - Data transfer between CPU and DAC  

---

## Key Learnings
- **Integration basics**: combining CPU, clocking, and peripheral blocks into one SoC.  
- **Clocking**: understanding how PLL-generated clocks synchronize processor operations.  
- **Data path**: observing how CPU output translates into DAC input and then into analog form.  
- **Verification**: using simulation waveforms to validate functional correctness of a mixed-signal SoC.  
