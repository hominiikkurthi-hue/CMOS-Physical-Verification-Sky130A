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

<img width="1672" height="941" alt="Tools Used_ Semiconductor Design Workflow" src="https://github.com/user-attachments/assets/8bbe38f6-2bec-4e61-8226-31c4f42170be" />

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
```
$  lsb_release -a    
$ uname -a     
$ free -h    
$ df -h /   
$ nproc   
```
These commands display the Ubuntu version, kernel information, available memory, disk space, and number of CPU cores.

Screenshot 1: System Information and Environment Verification

The screenshot documents the system configuration used for the project.
<img width="839" height="377" alt="image1" src="https://github.com/user-attachments/assets/2792ba3a-7e30-4183-ba92-176d340500c6" />

## 1.2 Update Ubuntu Packages

Update the package list and install available system updates.
```
$ sudo apt update
$ sudo apt upgrade -y
```
This helps prepare Ubuntu for installing the required EDA tools.

## 1.3 Install and Verify Git

Install Git for version control and project management.
```
$ sudo apt install git -y
$ git --version
```
Screenshot 2: Git Version Verification

The screenshot shows the Git version available in the Ubuntu environment.

<img width="829" height="421" alt="image2" src="https://github.com/user-attachments/assets/bd629af7-7648-438c-bf9a-a35381f1c20c" />

## 1.4 Install Required Dependencies

Install the basic packages needed to build and configure open-source EDA tools.

sudo apt install -y git build-essential \
autoconf automake libtool m4 tcsh \
tcl-dev tk-dev python3 python3-pip \
libx11-dev libxrender-dev libxext-dev \
libreadline-dev gawk bison flex

These packages provide compilers, build utilities, Tcl/Tk support, and other dependencies required by the tools.

## 1.5 Verify Magic VLSI

Check whether Magic VLSI is installed.
```
$ which magic
$ magic --version
```
If Magic is installed, these commands display its executable path and version.

Screenshot 3: Magic VLSI Verification

<img width="836" height="407" alt="image3" src="https://github.com/user-attachments/assets/1cd29531-a3b0-4bd3-8dac-7e8cbae80a49" />

## 1.6 Verify Xschem

Check whether Xschem is installed.
```
$ xschem --version
```
Launch the schematic editor using:
```
$ xschem
```
Screenshot 4: Xschem Launch

<img width="959" height="446" alt="image4" src="https://github.com/user-attachments/assets/fb346b3d-2aec-480b-bba5-96e298af5d34" />

## 1.7 Verify Netgen

Check whether Netgen is installed.
```
$ netgen --version
```
Netgen is used for Layout Versus Schematic (LVS) comparison.

Screenshot 5: Netgen Verification
<img width="935" height="395" alt="image5" src="https://github.com/user-attachments/assets/da752310-f518-4cf6-9866-ab69cd4d3721" />

## 1.8 Install and Verify ngspice

Install ngspice using:
```
$ sudo apt install ngspice -y
```
Check the installation:
```
$ ngspice --version
```
ngspice is used to simulate the CMOS inverter and analyze its input and output waveforms.

Screenshot 6: ngspice Verification
<img width="758" height="439" alt="image6" src="https://github.com/user-attachments/assets/41a1234b-7677-47e0-8a8f-8c2aa915e6b0" />

## 1.9 Install the SKY130A PDK

The SKY130A PDK provides transistor models, technology files, and design rules required for CMOS circuit design and physical verification.

First, check whether the PDK is already installed:

ls /usr/local/share/pdk/sky130A

If the directory does not exist, install the PDK using the Open_PDKs source and build instructions.

After installation, verify the technology files:

ls /usr/local/share/pdk/sky130A/libs.tech/
ls /usr/local/share/pdk/sky130A/libs.tech/magic/
ls /usr/local/share/pdk/sky130A/libs.tech/ngspice/

Check for Netgen files if the corresponding directory is present in your installation.

Screenshot 7: SKY130A PDK Verification

<img width="848" height="463" alt="image7" src="https://github.com/user-attachments/assets/c702c78a-31a0-41d6-95b8-622df11d2c26" />

## 1.10 Environment Setup Summary

The environment setup prepares Ubuntu for CMOS inverter design and physical verification using the SKY130A PDK and open-source EDA tools.

The following checklist is used to track the progress of the environment setup


## 2. Lab 1 — CMOS Inverter Physical Verification

### 2.1 Inverter Specifications

| Parameter | NFET (NMOS) | PFET (PMOS) |
|---|---|---|
| Model | `sky130_fd_pr__nfet_01v8` | `sky130_fd_pr__pfet_01v8` |
| Width (W) | 4.5 µm | 3 µm |
| Length (L) | 0.18 µm | 0.18 µm |
| Fingers (nf) | 3 | 3 |
| Body | VSS | VDD |

### 2.2 Schematic — Xschem

The CMOS inverter schematic is designed using Xschem and the SKY130A PDK.

**Command:**

```bash
cd ~/SKY130A-CMOS-Project/xschem
xschem cmos_inverter.sch
```

**Screenshot 1: CMOS Inverter Schematic**

<img width="1920" height="1080" alt="Screenshot (946)" src="https://github.com/user-attachments/assets/292a9491-d179-4f4c-ba47-0b54566f6d9a" />


### 2.3 Custom CMOS Inverter Symbol — Xschem

A custom symbol is created in Xschem for the CMOS inverter subcircuit, with VDD, VIN, VOUT, and VSS pins for testbench integration.

<img width="1920" height="1080" alt="Screenshot (948)" src="https://github.com/user-attachments/assets/b9d8876c-4891-43e6-b629-1b0c2625ec02" />

### 2.4 Schematic Testbench — Pre-Layout Simulation

The schematic is simulated using NGSpice to verify the inverter's operation before layout.

<img width="1920" height="1080" alt="Screenshot (949)" src="https://github.com/user-attachments/assets/5fe7a98f-d038-4cfc-9b26-63d40a8c4646" />



**Screenshot 3: Pre-Layout Simulation Waveforms**

<img width="558" height="466" alt="WhatsApp Image 2026-10-27 at 11 51 18 PM" src="https://github.com/user-attachments/assets/dd9cb603-7e66-44b2-bfe8-9102a6d018cd" />

### 2.5 Layout — Magic VLSI

The physical layout of the CMOS inverter is created using Magic and the SKY130A technology.

**Screenshot 4: CMOS Inverter Layout**
<img width="1920" height="833" alt="image" src="https://github.com/user-attachments/assets/0f344317-dbee-4cdf-9618-2e91495487ad" />





