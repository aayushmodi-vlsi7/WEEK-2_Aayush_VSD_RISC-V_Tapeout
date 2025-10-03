<div align="center">
  
# Week 2 – BabySoC Fundamentals & Functional Modelling
</div>  

## What is a System-on-Chip (SoC)?
A System-on-Chip (SoC) is an integrated circuit that brings together all the essential parts of a computer system onto a single chip. Instead of having separate components like CPU, memory, and peripherals placed on different boards, SoCs combine them into one compact design. This makes SoCs faster, more power-efficient, and cost-effective — which is why they are widely used in smartphones, IoT devices, and modern embedded systems.
## Main Components of an SoC
A typical SoC consists of:

- ### CPU (Processor):
The brain of the chip that executes instructions and manages operations.

- ### Memory: 
Includes on-chip SRAM, ROM, and interfaces for external memory (like DRAM) to store data and instructions.

- ### Peripherals: 
Interfaces that allow the SoC to communicate with the outside world (UART, SPI, I2C, GPIO, timers, etc.).

- ### Interconnect (Bus System): 
The communication backbone that links the CPU, memory, and peripherals together efficiently.

These components together allow the SoC to perform as a self-contained computing system.
## Why BabySoC?
BabySoC is a **simplified model** of an SoC designed specifically for learning purposes. Real-world SoCs are highly complex, but BabySoC removes unnecessary complications while keeping the core structure intact.

- It provides a **hands-on way** to understand how a CPU connects with memory and peripherals.

- It is easier to simulate and debug compared to industrial-scale SoCs.

- BabySoC gives learners the right foundation before moving on to advanced RTL design and physical implementation.

In short, BabySoC acts as a **training ground** to grasp SoC fundamentals without being overwhelmed by industry-level complexity.

## Role of Functional Modelling

Before we dive into RTL design or physical layout, it is important to first create a functional model of the SoC. Functional modelling helps in:

- **Early Validation:** Ensures the design behaves as expected before investing effort into detailed RTL coding.

- **Debugging at a High Level:** Makes it easier to test logic and functionality without worrying about low-level implementation.

- **Bridging Theory and Practice:** Learners can see how abstract SoC concepts translate into working simulations.

- **Efficient Design Flow:** Reduces errors and rework during later design stages.

By simulating BabySoC using tools like **Icarus Verilog** and **GTKWave**, we can observe its behavior, validate signal flow, and confirm functional correctness. This forms the foundation for moving forward into synthesis and physical design.

## Summary

- An **SoC** integrates CPU, memory, peripherals, and interconnects into a single chip.

- **BabySoC** provides a simplified, learner-friendly model to study SoC fundamentals.

- **Functional modelling** comes before RTL and physical design, ensuring correctness and reducing complexity.

- Hands-on practice with BabySoC helps bridge theory and practical SoC design, giving a solid foundation for advanced learning.

