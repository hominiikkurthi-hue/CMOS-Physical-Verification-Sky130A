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
```
$ ls /usr/local/share/pdk/sky130A
```

If the directory does not exist, install the PDK using the Open_PDKs source and build instructions.

After installation, verify the technology files:
```
$ ls /usr/local/share/pdk/sky130A/libs.tech/
$ ls /usr/local/share/pdk/sky130A/libs.tech/magic/
$ ls /usr/local/share/pdk/sky130A/libs.tech/ngspice/
```
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

**Screenshot 5: CMOS Inverter Layout**

<img width="521" height="308" alt="Screenshot 2026-10-29 075800" src="https://github.com/user-attachments/assets/0b8dc157-33a5-4bd8-a0a7-e4523205ef2a" />





### 2.6 DRC — Design Rule Check

DRC verification was performed using Magic VLSI with the SKY130A technology.

**Commands:**
```tcl
drc check
drc count
drc why
```

**Result:** Zero DRC errors were reported.

**Screenshot 14: Design Rule Check (DRC) Verification**

The final DRC check confirms that no design rule violations were detected in the CMOS inverter layout.
<img width="773" height="406" alt="image" src="https://github.com/user-attachments/assets/210de781-a124-4254-80be-dbbdcc862c1d" />

### 2.7 LVS — Layout Versus Schematic Verification

Layout Versus Schematic (LVS) is an important physical verification step used to verify whether the CMOS inverter layout matches the reference schematic in terms of device connectivity and circuit structure.

**Objectives**
- Verify the electrical connectivity of the CMOS inverter.
- Check NMOS and PMOS devices against the reference schematic.
- Identify connectivity errors and device mismatches.

**Tools Used**
- **Magic VLSI:** Physical layout and netlist extraction.
- **Netgen:** Layout Versus Schematic comparison.
- **SKY130A PDK:** Technology-specific device definitions.

**LVS Verification Procedure**

1. Create the CMOS inverter layout using Magic VLSI.
2. Extract the layout netlist using the appropriate SKY130A technology settings.
3. Generate the reference schematic netlist from Xschem.
4. Compare the extracted layout netlist with the reference schematic netlist using Netgen.
5. Analyze the comparison report and correct any mismatches if necessary.

**ScreenShot of LVS**

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/40ac7615-27b7-4757-8e28-98486b59d1a7" />




### 2.8 Post-Layout Simulation

Post-layout simulation evaluates the electrical behavior of the CMOS inverter using the extracted layout netlist. It helps analyze the effects of parasitic resistance and capacitance introduced during physical layout.

**Objectives**
- Verify the inverter's functionality after layout.
- Analyze input and output voltage waveforms.
- Examine propagation delay and output transition behavior.
- Compare pre-layout and post-layout simulation results.

**Tools Used**
- **Magic VLSI:** Layout extraction.
- **NGSpice:** Circuit simulation.
- **SKY130A PDK:** Transistor models and technology files.

**Procedure**

1. Extract the layout netlist and parasitic information using the supported extraction flow.
2. Prepare the post-layout simulation testbench.
3. Run the simulation using NGSpice.
4. Analyze the input and output waveforms.
5. Compare the results with the pre-layout simulation.

### 2.9 Results and Discussion

The CMOS inverter physical verification workflow includes schematic design, pre-layout simulation, physical layout creation, Design Rule Checking (DRC), Layout Versus Schematic (LVS) verification, and post-layout simulation.

Each stage contributes to validating the circuit design, checking physical layout constraints, verifying device connectivity, and evaluating electrical performance after layout.

### 2.10 Conclusion

This project demonstrates the CMOS inverter design and physical verification workflow using the SKY130A Process Design Kit and open-source EDA tools.

Xschem is used for schematic design, NGSpice for circuit simulation, Magic VLSI for physical layout and design rule checking, and Netgen for Layout Versus Schematic comparison.

The project provides practical experience in CMOS circuit design, physical layout implementation, design rule verification, netlist comparison, and post-layout analysis.










