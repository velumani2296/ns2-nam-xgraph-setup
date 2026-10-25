# NS2 Installation Guide on Ubuntu 22.04

A step-by-step guide to installing and configuring **NS2, NAM, AWK, and XGraph** on Ubuntu 22.04 for Computer Networks laboratory experiments.

This repository provides installation commands, configuration instructions, a sample NS2 simulation, and troubleshooting tips to help students set up their network simulation environment.

## 📌 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [1. Install NS2](#1-install-ns2)
- [2. Install NAM](#2-install-nam)
- [3. Install AWK](#3-install-awk)
- [4. Install XGraph](#4-install-xgraph)
- [5. Verify the Installation](#5-verify-the-installation)
- [6. Run an NS2 Simulation](#6-run-an-ns2-simulation)
- [7. Troubleshooting](#7-troubleshooting)
- [Contributing](#contributing)

## 📖 Introduction

NS2 (Network Simulator 2) is a discrete-event network simulator used for studying and analyzing computer networks. It supports the simulation of network protocols, routing algorithms, traffic behavior, and network performance.

This guide covers the following tools:

| Tool | Purpose |
|---|---|
| NS2 | Simulating computer networks |
| NAM | Visualizing network simulations |
| AWK | Processing NS2 trace files |
| XGraph | Plotting simulation results |
| Gedit | Editing TCL simulation scripts |
| Tcl | Scripting language used by NS2 |

## 💻 Prerequisites

Before starting, make sure you have:

- Ubuntu 22.04 LTS or 24.04 LTS (64-bit recommended).
- An active internet connection.
- Sudo privileges.
- Basic knowledge of Linux terminal commands.

## 1. Install NS2

### Step 1: Open the terminal

Press `Ctrl + Alt + T` to open the terminal.

### Step 2: Update package lists

```bash
sudo apt update
```

### Step 3: Install NS2

```bash
sudo apt install ns2 -y
```

### Step 4: Install NAM

```bash
sudo sudo apt-get install nam
```

### Step 5: Install Tcl

```bash
sudo apt install tcl -y
```

### Step 6: Verify NS2

Run the following command:

```bash
ns
```

If NS2 is installed correctly, the Tcl interpreter prompt should appear:

```text
%
```

Type the following command to exit:

```tcl
exit
```

## 2. Install NAM on Ubuntu 22.04 or 24.04

NAM (Network Animator) is used to visualize network simulations, including nodes, links, and packet movements.

If the default Ubuntu package does not work in your environment, you can try the provided Debian package.

### Step 1: Download the NAM package

Download the NAM 1.14 Debian package from the link below:

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

If the filename is different, replace it with the actual filename.

### Step 4: Launch NAM

```bash
nam
```

If the graphical window opens, NAM is working.

**Note:** The supplied Debian package is a third-party package. Compatibility with Ubuntu 22.04 may vary. If NAM does not launch, check the terminal output for missing libraries or other errors.

## 3. Install AWK on Ubuntu 22.04 or 24.04

AWK is a text-processing utility commonly used to analyze NS2 trace files and extract simulation statistics.

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

The installed version may differ depending on your Ubuntu updates.

## 4. Install XGraph on Ubuntu 22.04 or 24.04

XGraph is a graph plotting utility used in some NS2 laboratory experiments to visualize simulation results.

On newer Linux versions, traditional XGraph installation methods may not work. The following steps use the `xgraph` executable provided with the NS2 tutorial files.

### Step 1: Download XGraph

Open the following GitHub repository:

[XGraph Files](https://github.com/velumani2296/ns2-nam-xgraph-setup/tree/main/xgraph)

Download the `xgraph.tar` file and save it in your **Downloads** folder.

### Step 2: Open the Downloads directory

Open the terminal and run:

```bash
cd ~/Downloads
```

### Step 3: Update package lists

```bash
sudo apt-get update
```

After extraction, make sure the file named `xgraph` is available in the current directory.

If the archive extracts into a subfolder, navigate to that folder before proceeding.

### Step 4: Copy XGraph to `/bin`

Run the following command:

```bash
sudo cp xgraph /bin/xgraph
```

This copies the XGraph executable to `/bin`.

### Step 5: Set executable permissions

```bash
sudo chmod 755 /bin/xgraph
```

The `755` permission allows the owner to read, write, and execute the file while other users can read and execute it.

**Security note:** Avoid using `chmod 777` because it unnecessarily allows every user to modify the executable.

### Step 6: Verify XGraph

Run:

```bash
xgraph
```

If XGraph requires input files or displays an error, inspect the terminal output and check the executable's compatibility with Ubuntu 22.04 or Ubuntu 24.04.


### Important XGraph notes

- Avoid adding `-geometry` options to your TCL command when using this provided XGraph executable, as they may cause compatibility issues.
- Keep a copy of the `xgraph` executable in the same folder as your TCL files if you want it readily accessible for lab work.
- If you keep the executable only in the current directory, invoke it as `./xgraph` rather than `xgraph`, unless that directory is included in your `PATH`.
- Make sure the input files exist and contain valid graph data before running XGraph.
- If XGraph does not launch, inspect the terminal errors and verify that the executable is compatible with your system.

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

Type `exit` to close the interpreter.


### Check XGraph

```bash
xgraph
```

XGraph should be tested with a valid graph data file if it requires input.

## 6. Run an NS2 Simulation

Follow the steps below to test a simple two-node network simulation.

### Step 1: Create a TCL file

Create a new file using Gedit:

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

Save the file and close Gedit.

### Step 2: Execute the simulation

Navigate to the folder containing `lab1.tcl` and execute:

```bash
ns lab1.tcl
```

If NS2 and NAM are configured correctly, a NAM window should open displaying two nodes connected by a link.

### Expected result

- Two nodes connected by a duplex link.
- A NAM animation window.
- A simulation that finishes after 5 seconds of simulated time.

**Note:** This example generates a NAM trace for visualization. It does not generate the `out0.tr` and `out1.tr` files required by the XGraph example. Those files must be generated separately by a simulation designed to produce graph data.

## 7. Troubleshooting

### Problem 1: `ns: command not found`

Install NS2:

```bash
sudo apt update
sudo apt install ns2 -y
```

### Problem 2: `nam: command not found`

Check whether NAM is installed:

```bash
dpkg -l | grep nam
```

If necessary, install the package again using the correct Debian package filename.

### Problem 3: NAM does not open

Run NAM from the terminal:

```bash
nam
```

Check the displayed errors. Older NAM builds may have compatibility issues with newer Linux libraries or graphical environments.

### Problem 4: NAM animation does not appear

Check whether the NAM trace file exists:

```bash
ls -l lab1.nam
```

Then rerun the TCL script:

```bash
ns lab1.tcl
```

Resolve any errors displayed in the terminal.

### Problem 5: `awk` is not installed

Install GNU AWK:

```bash
sudo apt install gawk -y
```

### Problem 6: XGraph command is not found

Check the executable:

```bash
ls -l /bin/xgraph
```

If the file is missing, repeat the extraction and copy steps in the XGraph installation section.

### Problem 7: XGraph does not launch

Inspect the executable:

```bash
file /bin/xgraph
```

Make sure the executable is compatible with your operating system and that any required graphical libraries are available.

### Problem 8: XGraph cannot find input files

Check that the files exist in the current working directory:

```bash
ls -l out0.tr out1.tr
```

Replace these filenames with the actual files generated by your simulation.


## 🎯 Learning Objectives

After completing this guide, you should be able to:

- Install and run NS2 on Ubuntu 22.04.
- Install and test NAM for network animation.
- Install AWK for processing NS2 trace files.
- Configure XGraph for plotting simulation data.
- Write and execute basic TCL simulation scripts.
- Identify and troubleshoot common installation problems.

## 🤝 Contributing

Suggestions, corrections, and improvements are welcome.

If you find an issue or have a better installation method:

1. Open an issue in this repository.
2. Describe the problem and include relevant terminal errors.
3. Submit a pull request with your improvements.

Contributions that improve compatibility with newer Ubuntu versions are especially welcome.

## 📄 License

This repository is intended for educational purposes.

Consider adding an open-source license, such as the MIT License, if you want others to reuse and modify your original content under its terms. Ensure that third-party files and resources are distributed according to their respective licenses.

---

**Maintained for educational use | Ubuntu 22.04 or Ubuntu 24.04 | Computer Networks Laboratory**
