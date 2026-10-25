# NS2 Installation Guide on Ubuntu 22.04

A step-by-step guide to installing and configuring **NS2 (Network Simulator 2), NAM (Network Animator), AWK, and XGraph** on Ubuntu 22.04 for Computer Networks laboratory experiments.

## 📌 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [1. Install NS2](#1-install-ns2)
- [2. Install NAM](#2-install-nam)
- [3. Install AWK](#3-install-awk)
- [4. Install XGraph](#4-install-xgraph)
- [5. Verify the Installation](#5-verify-the-installation)
- [6. Run an NS2 Simulation](#6-run-an-ns2-simulation)
- [Useful Resources](#useful-resources)
- [Troubleshooting](#troubleshooting)

## 📖 Introduction

NS2 is a discrete-event network simulator used for studying and analyzing computer networks. It supports simulation of network protocols, traffic behavior, routing algorithms, and network performance.

This guide covers the installation of the following tools:

| Tool | Purpose |
|---|---|
| NS2 | Network simulation |
| NAM | Visualization of network simulations |
| AWK | Processing simulation trace files |
| XGraph | Plotting simulation results and graphs |
| Gedit | Text editor for writing TCL scripts |

## 💻 Prerequisites

- Ubuntu 22.04 LTS (64-bit recommended)
- Internet connection
- Sudo privileges
- Basic knowledge of Linux terminal commands

## 1. Install NS2

Open the Terminal using `Ctrl + Alt + T`.

### Step 1: Update package lists

```bash
sudo apt update
```

### Step 2: Install NS2

```bash
sudo apt install ns2 -y
```

### Step 3: Install NAM and Gedit

```bash
sudo apt install nam gedit -y
```

### Step 4: Install Tcl

```bash
sudo apt install tcl -y
```

### Step 5: Verify NS2

```bash
ns
```

If NS2 is installed correctly, the Tcl interpreter prompt should appear:

```text
%
```

Type `exit` to return to the terminal.

## 2. Install NAM

NAM (Network Animator) is used to visualize network simulations, including nodes, links, and packet movements.

On Ubuntu 22.04, the NAM package available through the default repositories may not work in every environment. If you need the provided Debian package, follow these steps.

### Step 1: Download the NAM package

Download the Debian package from the following link:

[Download NAM 1.14 for Ubuntu](https://drive.google.com/file/d/0B7S255p3kFXNNmtLeXhsaG5hXzQ/edit?resourcekey=0-NJmLwKSqlXCAjXSHr6ILiA)

### Step 2: Open the Downloads directory

```bash
cd ~/Downloads
```

### Step 3: Install NAM

Make sure the downloaded file is named `nam_1.14_amd64.deb`, then run:

```bash
sudo apt install ./nam_1.14_amd64.deb
```

If the filename differs, replace it with the actual downloaded filename.

### Step 4: Launch NAM

```bash
nam
```

If the NAM graphical window opens, the installation is working.

**Note:** The downloaded package is third-party software. Check its compatibility and source before installing it. If NAM fails to launch, see the troubleshooting section.

## 3. Install AWK

AWK is a text-processing utility commonly used to extract and analyze information from NS2 trace files.

### Step 1: Update package lists

```bash
sudo apt update
```

### Step 2: Install GNU AWK

```bash
sudo apt install gawk -y
```

### Step 3: Verify the installation

```bash
awk --version
```

Example output:

```text
GNU Awk 5.1.0
```

The version displayed on your system may differ depending on package updates.

## 4. Install XGraph

XGraph is a plotting utility used in some NS2 laboratory experiments to display simulation results graphically.

Follow this tutorial for installation instructions:

- [XGraph Installation Tutorial on YouTube](https://www.youtube.com/watch?v=qt9NkCi9ZRI)

Additional NS2 resources and examples:

- [NS2 Tutorials on GitHub](https://github.com/Arose-Niazi/Tutorials/tree/master/NS2)

**Note:** XGraph may require additional configuration or dependencies on Ubuntu 22.04. Follow the tutorial carefully and verify that the installation method is compatible with your system.

## 5. Verify the Installation

Run the following commands individually in the terminal.

### Check NS2

```bash
ns
```

Type `exit` after verifying the Tcl prompt.

### Check NAM

```bash
nam
```

A graphical window should open if NAM is configured correctly.

### Check AWK

```bash
awk --version
```

### Check Tcl

```bash
tclsh
```

Type `exit` to close the Tcl interpreter.

### Check Gedit

```bash
gedit
```

If the corresponding command works, the tool is available. XGraph should also be tested using a valid graph data file.

## 6. Run an NS2 Simulation

Follow these steps to test a simple two-node network simulation.

### Step 1: Create a TCL file

```bash
gedit lab1.tcl
```

Paste the following code into the file:

```tcl
# Create a simulator object
set ns [new Simulator]

# Open the NAM trace file
set nf [open lab1.nam w]
$ns namtrace-all $nf

# Create two nodes
set n0 [$ns node]
set n1 [$ns node]

# Create a duplex link between the nodes
$ns duplex-link $n0 $n1 1Mb 10ms DropTail

# Define the finish procedure
proc finish {} {
    global ns nf
    $ns flush-trace
    close $nf
    exec nam lab1.nam &
    exit 0
}

# Schedule the simulation to finish
$ns at 5.0 "finish"

# Run the simulation
$ns run
```

Save the file and close the editor.

### Step 2: Execute the simulation

Navigate to the directory containing `lab1.tcl` and run:

```bash
ns lab1.tcl
```

If NS2 and NAM are configured correctly, a NAM window should open displaying two nodes connected by a link.

**Expected result:** A network animation window showing two nodes and their connecting link.

## 7. Troubleshooting

### Problem: `ns: command not found`

Install NS2 again and check the installation:

```bash
sudo apt update
sudo apt install ns2 -y
```

### Problem: `nam: command not found`

Check whether NAM is installed:

```bash
dpkg -l | grep nam
```

If necessary, reinstall the package using the correct Debian package filename.

### Problem: NAM does not open

Try launching NAM from the terminal:

```bash
nam
```

Read any error messages displayed. Older NAM builds may encounter compatibility issues with newer Linux libraries or graphical environments.

### Problem: NS2 runs, but NAM animation is not displayed

Check that the TCL script generates a NAM trace file:

```bash
ls -l lab1.nam
```

Run the script again and inspect any errors printed in the terminal.

### Problem: `awk` is not installed

Install GNU AWK:

```bash
sudo apt install gawk -y
```

## 📚 Useful Resources

1. [NAM 1.14 Debian Package](https://drive.google.com/file/d/0B7S255p3kFXNNmtLeXhsaG5hXzQ/edit?resourcekey=0-NJmLwKSqlXCAjXSHr6ILiA)
2. [XGraph Installation Tutorial](https://www.youtube.com/watch?v=qt9NkCi9ZRI)
3. [NS2 Tutorials and Examples](https://github.com/Arose-Niazi/Tutorials/tree/master/NS2)

## 🎯 Learning Objectives

After completing this guide, you should be able to:

- Install NS2 on Ubuntu 22.04.
- Configure NAM for network animation.
- Install and use AWK for trace-file analysis.
- Set up XGraph for plotting simulation results.
- Create and execute basic TCL simulation scripts.
- Troubleshoot common NS2 installation problems.

## 🤝 Contributing

Suggestions, corrections, and improvements are welcome. Feel free to open an issue or submit a pull request to improve this guide.

## 📄 License

This repository is intended for educational purposes. Add a suitable open-source license, such as the MIT License, if you want others to reuse and modify your original content under its terms.

---

**Maintained for educational use | Ubuntu 22.04 | Computer Networks Laboratory**
