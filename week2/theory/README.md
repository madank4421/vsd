# BabySoC Fundamentals & Functional Modelling 

#  Part 1 – Theory (Conceptual Understanding)

## What is a System on a Chip?

A System on Chip (SoC) is an integrated circuit where all the necessary components of an entire electronic system are integrated on one piece of silicon. Rather than using numerous individual chips, an SoC integrates a general-purpose processor, custom hardware accelerators, on-chip memory, peripheral interfaces, and communication modules as a single integrated design. SoCs minimize circuit board complexity by packing all these functions on a single die.



## Components of a Typical SoC

- CPU (Processor core/s): The central unit that executes instructions.  
- Memory blocks: RAM and ROM (or Flash) on-chip.  
- System Interconnect / Bus: High-speed communication backbone that links CPU, memory, and peripherals.  
- I/O Interfaces: Minimum basic interfaces like UART, SPI, I²C, GPIO. Provide connectivity with external devices.  
- Power Management Unit (PMU): To regulate and manage energy.  

Optional/common additional components depending on application:

- GPU or DSP: For graphics or signal processing.  
- Communication modules: Wi-Fi, Bluetooth, or cellular support.  
- Analog/Mixed-signal circuits: ADCs, DACs, sometimes RF blocks.  



## What is VSDBabySoC?

BabySoC refers to a simple System on Chip (SoC) implementation that includes a CPU, a PLL for clock generation, and a DAC. This model is designed to help understand the fundamentals of SoC design and to analyze its functioning through simulation. By focusing on these core components, BabySoC provides a clear, manageable example for learning how integrated systems operate and communicate within a single chip.

![VSDBabySoC Block diagram](images/VSDBabySoC.png)

## Components in VSDBabySoC

- RVMYTH: A simple, pipelined RISC-V CPU core.  
- PLL: An 8x Phase-Locked Loop for stable and synchronized clock generation.  
- DAC: A 10-bit Digital-to-Analog Converter for converting digital input values into analog voltages.  


## Why BabySoC is a Simplified Model for Learning SoC Concepts

BabySoC is a simplified SoC model because it focuses only on the core components necessary to understand basic SoC operation, without the complexity of a full-fledged commercial SoC.  

- Minimal Components: Contains only a CPU, a PLL for clock generation, and a DAC, making it easier to study module interactions.  
- Conceptual Clarity: Reduces peripheral modules and advanced features, allowing learners to focus on the fundamentals of system integration, clocking, and data conversion.  
- Simulation-Friendly: Can be simulated easily, allowing observation of system behavior, timing, and module interaction without complex testbenches.  
- Foundation for Advanced SoC Design: Provides a strong base before moving on to more complex SoCs with multiple cores, memory hierarchies, accelerators, and communication modules.  


## The Role of Functional Modelling Before RTL and Physical Design

Functional modelling provides a high-level representation of a system, focusing on what the system does rather than how it is physically implemented. It allows designers to verify functionality, test algorithms, and detect errors early, before writing detailed RTL code. This ensures that the system behaves as intended and provides a blueprint for RTL and physical design stages.

- Functional modelling helps verify system behaviour and correctness early.  
- It simplifies simulation compared to RTL or gate-level models.  
- It Guides RTL development by specifying expected module functionality.  
- It reduces errors and iterations in later design stages.  
