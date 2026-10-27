# CMOS Physical Verification Using SKY130A

## Project Overview
This project focuses on CMOS inverter design and physical verification using the SKY130A Process Design Kit (PDK) and open-source VLSI tools.

## Objectives
- Design a CMOS inverter schematic.
- Simulate circuit behavior using Ngspice.
- Create the physical layout using Magic.
- Perform Design Rule Checking (DRC).
- Perform Layout Versus Schematic (LVS) verification using Netgen.

## Tools Used
- **SKY130A PDK:** Technology files and device models.
- **Xschem:** Schematic design.
- **Ngspice:** Circuit simulation.
- **Magic:** Physical layout and DRC.
- **Netgen:** LVS verification.

NGSpice	37	Pre and post-layout simulation
SKY130A PDK	Latest	Process design kit
Development Environment
Host OS: Windows 11
Virtualization: Oracle VirtualBox
Guest OS: Ubuntu 24.04.5 LTS
Note: Ubuntu 26.04.1 LTS (latest) was initially attempted but caused tool compatibility issues during EDA tool compilation. Downgraded to Ubuntu 24.04.5 LTS for a stable environment.

System Resources
Resource	Value
OS	Ubuntu 24.04.5 LTS (Noble)
Kernel	7.0.0-38-generic
RAM (total)	10 GiB
RAM (available)	2.9 GiB
Swap	4.0 GiB
CPU cores	4
Disk size	59 GiB
Disk used	38 GiB (68%)
Disk available	19 GiB
1. Environment Setup
1.1 System Validation
lsb_release -a
uname -a
free -h
df -h /
nproc

## Project Workflow
1. Configure the SKY130A PDK and EDA tools.
2. Draw the CMOS inverter schematic.
3. Simulate the circuit and inspect the waveforms.
4. Create the layout in Magic.
5. Run DRC and fix any violations.
6. Extract the layout netlist and run LVS.
7. Document the results with screenshots.

## 1. Environment Setup

This section explains how to prepare the Ubuntu environment for the SKY130A CMOS inverter physical verification project.

## 1.1 System Validation

First, check the Ubuntu operating system and available system resources.

Run the following commands in the Ubuntu terminal:
'''
$ lsb_release -a    
$ uname -a     
$ free -h    
$ df -h /   
$ nproc   
'''
These commands display the Ubuntu version, kernel information, available memory, disk space, and number of CPU cores.

Screenshot 1: System Information and Environment Verification

The screenshot documents the system configuration used for the project.
<img width="839" height="377" alt="image1" src="https://github.com/user-attachments/assets/2792ba3a-7e30-4183-ba92-176d340500c6" />


