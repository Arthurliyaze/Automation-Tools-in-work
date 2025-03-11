# Dynamic Simulation Automation Tool - Comprehensive User Manual

## Table of Contents
- [Dynamic Simulation Automation Tool - Comprehensive User Manual](#dynamic-simulation-automation-tool---comprehensive-user-manual)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [What is a Dynamic Simulation?](#what-is-a-dynamic-simulation)
  - [System Requirements](#system-requirements)
    - [Software Requirements:](#software-requirements)
    - [Hardware Requirements:](#hardware-requirements)
  - [Installation Steps](#installation-steps)
    - [Step 1: Install Python](#step-1-install-python)
    - [Step 2: Install Required Libraries](#step-2-install-required-libraries)
  - [How the Tool Works](#how-the-tool-works)
    - [Step 1: Define the Channels](#step-1-define-the-channels)
    - [Step 2: Write the Contingencies](#step-2-write-the-contingencies)
      - [**Fault Parameters**](#fault-parameters)
      - [**Simulation Settings:**](#simulation-settings)
      - [**Fault Injection and Clearing:**](#fault-injection-and-clearing)
    - [Step 3: Configure Your Simulation Files](#step-3-configure-your-simulation-files)
      - [**Preparing Files for Dynamic Simulation**](#preparing-files-for-dynamic-simulation)
      - [**Detailed Breakdown of the Script**](#detailed-breakdown-of-the-script)
      - [Importing Required Libraries](#importing-required-libraries)
      - [Main Function: Collecting Input Files](#main-function-collecting-input-files)
      - [File Selection Dialogs](#file-selection-dialogs)
      - [Generating the Configuration File](#generating-the-configuration-file)
      - [Finalizing the Configuration](#finalizing-the-configuration)
      - [**Troubleshooting**](#troubleshooting)
    - [Step 4: Test the Script](#step-4-test-the-script)
      - [**Detailed Breakdown of the Script**](#detailed-breakdown-of-the-script-1)
      - [Batch Mode vs. Single Mode](#batch-mode-vs-single-mode)
      - [Loading Simulation Files](#loading-simulation-files)
      - [Pre-Processing (User-Defined Models, Prefix Files)](#pre-processing-user-defined-models-prefix-files)
      - [Solving Power Flow](#solving-power-flow)
      - [Channel Definition Files](#channel-definition-files)
      - [Dynamic Simulation Start](#dynamic-simulation-start)
      - [Event Simulation (Contingencies)](#event-simulation-contingencies)
    - [Step 5: Run Multiprocess](#step-5-run-multiprocess)
      - [**Why using multiprocess?**](#why-using-multiprocess)
      - [**Detailed Breakdown of the Script**](#detailed-breakdown-of-the-script-2)
      - [Main Function](#main-function)
      - [The `writeRunSimulationbatFile()` Function](#the-writerunsimulationbatfile-function)
      - [The `RunSimulationProcess()` Function](#the-runsimulationprocess-function)
      - [The `RunSimulationProcessParallel()` Function](#the-runsimulationprocessparallel-function)
    - [Step 6: Plot results](#step-6-plot-results)
      - [The `create_batchidv_file_with_out_files()` function](#the-create_batchidv_file_with_out_files-function)
      - [The `convert_eps_to_pdf()` function](#the-convert_eps_to_pdf-function)
  - [Conclusion](#conclusion)

---

## Introduction

Welcome to the **Dynamic Simulation Automation Tool** user manual! This guide is designed to help you get started with using the tool, even if you're not familiar with programming or PSSE (Power System Simulation for Engineering). This tool will help automate the process of running dynamic simulations, which are typically used to analyze how power systems respond to disturbances or faults over time. The tool allows you to run multiple simulations at once and manage results efficiently.

This manual covers:
- A detailed explanation of what a dynamic simulation is.
- System requirements and how to install everything you need.
- Step-by-step instructions on how to use the tool.
- A breakdown of the Python code used in the tool.

---

## What is a Dynamic Simulation?

A **dynamic simulation** is a technique used to analyze the behavior of a power system over time. It simulates the system’s response to various events, such as faults, disturbances, or changes in load. Dynamic simulations are essential for studying transient conditions, like voltage fluctuations or system instability, and help engineers design safer and more reliable power systems.

In the case of **PSSE** (Power System Simulation for Engineering), dynamic simulations are run to model the time-domain behavior of an electrical grid. This includes simulating how generators, loads, and protection systems respond to disturbances.

By using this tool, you can automate the process of running multiple dynamic simulations, saving time and ensuring accuracy.

---

## System Requirements

Before using the tool, ensure that your system meets the following requirements:

### Software Requirements:
1. **Python 3.x**: The tool is written in Python, a programming language used for scripting and automation. You will need Python version 3.x (ideally Python 3.9 or newer).
2. **Libraries**:
    - **numpy**: A library used for mathematical and numerical operations.
    - **wxPython**: A library for building graphical user interfaces (GUIs), which the tool uses to help you select files easily.
    - **pywin32**: A Python package that allows interaction with Windows OS features, particularly useful for handling processes and system interactions.
    - **subprocess**: A module used to run system-level commands, such as executing batch files.
    - **ghostscript**: Required for converting `.eps` (Encapsulated PostScript) files into `.pdf` files.
    - **Other Python Modules**: `os`, `sys`, `csv`, `getopt`, `time`, `datetime`, etc.
3. **PSSE 35**: The tool is designed to interact with PSSE 35 simulations, but it can be modified for use with other PSSE versions if needed.

### Hardware Requirements:
- **CPU**: A modern processor with at least 2 cores. The tool can run simulations in parallel, so a multi-core processor will provide better performance.
- **RAM**: 8 GB of RAM or more is recommended for running multiple simulations.
- **Disk Space**: Sufficient disk space is needed to store simulation outputs and logs, especially if running a large number of simulations.

---

## Installation Steps

### Step 1: Install Python

If you don’t have Python installed on your system:
1. Download Python from the official [Python website](https://www.python.org/downloads/).
2. During installation, ensure that you select the option to "Add Python to PATH" to make it accessible from the command line.
3. If Python 2.x is also installed on your computer, please move the Python 3.x PATH to the top in the environment variables, verify by opening a terminal/command prompt and typing:
   ```bash
   python --version
   ```
   This should return the installed version of Python.

### Step 2: Install Required Libraries

Once Python is installed, open your command prompt (Windows) or terminal (Mac/Linux) and run the following command to install the required libraries:
```bash
pip install numpy wxPython pywin32 ghostscript
```
This will install all the necessary Python packages.

---
## How the Tool Works
### Step 1: Define the Channels
To run the dynamic simulation, you have to define the "channel" (the parameter you want to monitor), the script (`channelDef.py`) configures the PSSE simulation environment by defining specific subsystems and setting up channels to monitor various parameters, particularly focusing on voltage levels at certain buses.

One major advantage of defining channels in Python instead of using an IDV file is flexibility and automation. Python allows you to programmatically control and modify the channel definitions, enabling dynamic adjustments, conditional logic, and integration with other parts of your simulation workflow. Here's a detailed breakdown of how the code works:

---

The **voltage buses** are defined with the following list:

```python
volt_bus = [8117, 8102, 8172]
```

This defines three buses (8117, 8102, 8172) as part of the voltage monitoring. These buses are selected because they are critical to the analysis of voltage levels in the system.

---

The `psspy.bsys` function is then used to define different **bus subsystems** for monitoring. For example, the following lines define two subsystems, one for the **138kV voltage** system and one for the **34.5kV voltage** system:

```python
psspy.bsys(sid = 8, usekv = 1, basekv = [70, 765], numbus = len(volt_bus), buses = volt_bus)  # 138kV voltage
psspy.bsys(sid = 9, usekv = 1, basekv = [0, 69], numbus = len(volt_bus), buses = volt_bus)  # 34.5kV voltage
```

The first line sets up subsystem 8, which is associated with the 138kV voltage level, and the second line defines subsystem 9 for the 34.5kV voltage level. These subsystems are critical for defining the buses that are part of each voltage group.

---

Next, the `psspy.chsb` function is used to define **channels for monitoring** in each subsystem. For subsystems 8 and 9, the channels are configured as follows:

```python
psspy.chsb(sid = 8, all = 0, status4 = 1, status5 = 13, status6 = 1)
psspy.chsb(sid = 9, all = 0, status4 = 1, status5 = 13, status6 = 1)
```

These two lines of code create channels to monitor the **voltage per unit (pu)** at the buses in subsystems 8 and 9. The parameters for `psspy.chsb` are as follows:

- `sid` specifies the subsystem ID. In this case, it is either 8 or 9, depending on the voltage level (138kV or 34.5kV).
- `all = 0` indicates that specific buses (not all buses) will be monitored.
- `status4 = 1` specifies that machine, bus, load, and branch quantities will be tracked for the given subsystem. This ensures that the monitoring includes important electrical quantities for each bus.
- `status5 = 13` defines the specific parameter to monitor, which in this case is **bus voltage (per unit)**. The value 13 corresponds to the PSSE predefined constant for bus voltage.
- `status6 = 1` indicates that the monitored quantity will be the voltage at the bus level.

---

There are also some **commented-out sections** of code that define other subsystems and channels for monitoring additional quantities. For example, the code for subsystem 6, which might be related to renewable energy buses, is as follows:

```python
# psspy.bsys(sid = 6, numbus = len(ws_bus), buses = ws_bus)  # Renewable Bus
# psspy.chsb(sid = 6, all = 0, status4 = 1, status5 = 4, status6 = 1)
# psspy.chsb(sid = 6, all = 0, status4 = 1, status5 = 2, status6 = 1)
# psspy.chsb(sid = 6, all = 0, status4 = 1, status5 = 3, status6 = 1)
```

Although these lines are commented out, they show how the code could define additional subsystems (e.g., a subsystem for renewable energy buses) and monitor different electrical quantities such as terminal voltage (`status5 = 4`), active power (`status5 = 2`), and reactive power (`status5 = 3`). These settings would help track a broader range of parameters across different parts of the power system.
### Step 2: Write the Contingencies
The scripts (`Px_xx.py`) are  used to define different klind of faults in a power system model using PSSE, with fault clearing and possible reclosure. 

The advantage of writing contingencies in Python over IDV is greater flexibility and customization. In addition, with the given example of contingencies in different category, the time to repair the contingency list will be reduced.

Here's an example and breakdown of the major steps in a contigency python file and what each section does:

#### **Fault Parameters**
   - **Fault Bus Number:** Defines the bus where the fault occurs (in this case, bus `8117`).
   - **Fault Clearing Time:** The time it takes to clear the fault (in cycles and converted to time).
   - **Normal Clearing Elements:** Lists the system elements (buses, branches, transformers, generators) that are cleared after the fault occurs. These elements are cleared using the breaker-to-breaker method.

   ```python
   Fault_Bus_Number = 8117
   dist_clear_faulting_Cycles = 4.5
   normal_cleared_branches = [[8117, 8099, '1'],[8099, 8192, '1'],[8192, 8102, '1']]
   ```

#### **Simulation Settings:**
   The timing of the simulation is set, where `Fault_Time` is the time when the fault is applied, and `End_Time` is the total simulation time. The simulation progresses in steps defined by `log_per_step` and `plot_per_step`. The simulation time step is defined by `realar`.

   ```python
   Fault_Time = 1.0
   End_Time = 20.0
   realar = [0.5,_f, 0.00104165, _f,_f,_f,_f,_f]
   ```

#### **Fault Injection and Clearing:**
   The fault is applied at the specified bus (`Fault_Bus_Number`), and the system is run for the duration of the fault (`Fault_Time`). After that, the fault is cleared, and the system continues for the defined `dist_clear_faulting_Time`.

   ```python
   ierr = psspy.dist_bus_fault(Fault_Bus_Number)
   if ierr != 0:
       psspy.stop_2()
   ```

   After this, the branches and other elements cleared by normal clearing (like buses, generators, etc.) are tripped.

   ```python
   for [fromBusNumber, toBusNumber, cktID] in normal_cleared_branches:
       ierr = psspy.dist_branch_trip(fromBusNumber, toBusNumber, cktID)
       if ierr > 0 and ierr < 4:
           psspy.progress(" Error Code %d in tripping %d %d %s!\n" % (ierr, fromBusNumber, toBusNumber, cktID))
           psspy.stop_2()
   ```

### Step 3: Configure Your Simulation Files

Before running the tool, you need to prepare a **configuration file**. This file will contain the parameters for each dynamic simulation you want to run. 
#### **Preparing Files for Dynamic Simulation**
The configuration file should be in **CSV format** and typically looks like this:

```csv
studyName,Basecase,Snapshot,Channel Definition,Fault Definition,Pre-fix,User Defined Model,outFileName,logFileName
1,case.cnv,case.snp,channelDef.py,..\run\P0_01.py,addusermodel.idv,dsusr.dll,..\out\P0_01.out,	..\log\P0_01.log
```
To run the dynamic simulation, you need to prepare the following files:

1. **Basecase** (`*.sav`,`*.cnv`): These are the initial power system states.
2. **Snapshot** (`*.snp`): These represent system states at specific time points.
3. **Channel Definitions** (`*.py`, `*.idv`): Files defining communication channels for the simulation.
4. **Fault Definitions** (`*.py`, `*.idv`): These files describe fault events for simulations.
5. **Pre-fix** (`*.idv`): These files add the user defined model or other adjustments to the case.
6. **User-defined Models** (`*.dll`): User defined models used for the simulation.

The dynamic simulation configuration script (`Prepare_DynSimuConfig.py`) facilitates selecting and organizing these files into a configuration for the dynamic simulation.

---

#### **Detailed Breakdown of the Script**

#### Importing Required Libraries

The script begins by importing the libraries necessary for its operation.

```python
import os, sys, re, collections, time, datetime, csv, getopt, wx, signal
import win32pdh, string, win32api
```

- **os**: Interacts with the operating system, handling directories and file paths.
- **sys**: Manages command-line arguments and the Python environment.
- **re**: Used for regular expressions (not used directly in this code, but typically helpful for pattern matching in filenames).
- **wx**: wxPython library for creating GUI-based file dialogs to select input files.
- **csv**: Writes configuration data into a CSV format.
- **getopt**: Handles command-line arguments.
- **signal**: Used for handling process signals.
- **win32pdh, win32api**: Windows-specific modules for interacting with system performance data and API calls.

---

#### Main Function: Collecting Input Files

The **main function** orchestrates the entire process, guiding the user through selecting the necessary files for the simulation.

```python
def main():
    # parse command line options
    try:
        opts, args = getopt.getopt(sys.argv[1:], "h", ["help"])
    except getopt.error, msg:
        print msg
        print "for help use --help"
        sys.exit(2)
```

- This block parses the command-line options, specifically checking if the `--help` flag is used. If so, it displays the documentation and exits.

```python
    app = wx.App()
    app.MainLoop()
```

- Initializes a wxPython app for GUI-based file selection. The `MainLoop()` starts the wxPython event loop, allowing the user to interact with the dialog boxes.

---

#### File Selection Dialogs

The script opens multiple **file selection dialogs** to allow the user to choose the necessary files:

1. **Basecase Selection**: Allows the user to select one or more `*.cnv` files that contain the basecases.
2. **Snapshot Selection**: Allows the user to select `*.snp` files, which contain the system snapshots.
3. **Fault Definition Selection**: Allows the user to select fault definition files (`*.py`, `*.idv`).
4. **Channel Definition Selection**: Allows the user to select channel definition files (`*.py`, `*.idv`).
5. **User-defined Model Selection**: Lets the user choose custom model DLLs (`*.DLL`).

Each dialog presents the user with a file picker, and the selected paths are added to respective lists.

```python
    caseList = []
    dialog = wx.FileDialog(parent = None,
                           message = "Please Select .cnv Cases to study:",
                           defaultDir = os.getcwd(),
                           style = wx.FD_OPEN|wx.FD_MULTIPLE|wx.FD_CHANGE_DIR,
                           wildcard = "PSSE savecases (*.cnv)|*.cnv",
                           )
```

---

#### Generating the Configuration File

Once the files are selected, the script creates a **CSV configuration file** (`DynSimuConfig.csv`). This file contains the details about each simulation case, including basecases, snapshots, fault definitions, and the output filenames for logs and results.

```python
    with open(dynSimuConfigFileName, 'wb') as csvfile:
        writer = csv.writer(csvfile, dialect='excel')
        writer.writerow(['Study Name', 'Basecase', 'Snapshot', 'Channel Definition', 'Event Definition', 'Pre-fix(Separated by ";")', 'User Defined Model(Separated by ";")', '.out File', '.log File'])
```

The script iterates through each possible combination of basecase, snapshot, fault, and other parameters and writes a row in the CSV file.

Each combination generates a **study name** (e.g., "001", "002") and associated output and log filenames, allowing the dynamic simulation to run for different configurations.

---

#### Finalizing the Configuration

The script completes by writing the configuration data into the CSV file. The generated file will be used by the dynamic simulation script to automate the simulation process for multiple cases.

```python
    with open(dynSimuConfigFileName, 'wb') as csvfile:
        writer = csv.writer(csvfile, dialect='excel')
        writer.writerow([...])
        # Writing each simulation configuration
```
---

#### **Troubleshooting**

- **File Selection Issues**: Ensure that the correct file types are selected (e.g., `*.cnv` for basecases, `*.snp` for snapshots, etc.). If a file is not selected, the script will return `False` and terminate.
- **Missing Libraries**: Ensure that all required libraries (`wxPython`, `pywin32`, etc.) are installed properly. Use `pip` to install them if necessary.
- **Permission Issues**: If the script is not able to create the configuration file or write output, check your directory permissions.

---

You should place this CSV file in the same directory as the Python script. Once the configuration file has been generated, it can be passed to the dynamic simulation script (`dynamicSimulation.py`). This script will read the configuration file and execute the dynamic simulations for each combination of input files and parameters. 

### Step 4: Test the Script

To use the tool, follow these steps:

1. **Download the script**: Get the Python script (`dynamicSimulation.py`) from the provided source and place it in a folder on your computer.
2. **Prepare the configuration file**: Create or download a CSV file that contains all the simulation cases you want to run.
3. **Run the script**:
   - Open the command prompt or terminal.
   - Navigate to the directory where the Python script and configuration file are located.
   - Execute the script with the following command:
     ```bash
     python dynamicSimulation.py
     ```

When you run the script, it will prompt you to select the configuration file, and it will then proceed to run the simulations as described. You can test the flat start simulation and check the out file and log file in the same folder.

#### **Detailed Breakdown of the Script**

The `main()` function is the core of the script. It handles command-line arguments, loads configuration files, and orchestrates the entire simulation process.

```python
def main():
    try:
        opts, args = getopt.getopt(sys.argv[1:], "h", ["help"])
    except getopt.error as msg:
        print(msg)
        print("for help use --help")
        sys.exit(2)
```
- **Command-line Parsing**: Uses `getopt` to parse command-line arguments, allowing the user to specify options (e.g., `--help`).

#### Batch Mode vs. Single Mode
```python
if batch_mode:
    configFileName = args[0]
    studyName = args[1]
    # Load the configuration file and extract simulation parameters
else:
    batch_mode = False
    # Interactively prompt for file inputs in single mode
```
- **Batch Mode**: If running in batch mode, the script loads a configuration file and extracts parameters for the study.
- **Single Mode**: If running interactively, the script prompts the user for file selections (e.g., event files, prefix files).

#### Loading Simulation Files
```python
psspy.case(basecaseFileName)
psspy.rstr(snapshotFileName)
```
- **Base Case**: Loads the base case (`.sav` file) into PSSE.
- **Snapshot**: Loads a snapshot file (`.snp`).

#### Pre-Processing (User-Defined Models, Prefix Files)

The program can optionally load user-defined models (UDMs) and execute additional scripts (e.g., `.py` files) to adjust the system before the dynamic simulation.

```python
for each in prefixFileNameList:
    psspy.progress("\n Run %s ...\n" % (each))
    exec(compile(open(each.strip(), "r").read(), each.strip(), 'exec'))
```
- **Prefix Files**: These are additional files (e.g., scripts or response files) that can adjust the system before the main dynamic simulation.

#### Solving Power Flow

The program ensures that the power flow is solved and that the system is in a valid operating state before running the dynamics.

```python
psspy.fnsl([0, 0, 0, 1, 1, 0, 0, 0])
```
- **Power Flow**: Solves the power flow problem using `fnsl()`.

#### Channel Definition Files
```python
if chnlDefFileName.lower().endswith(".py"):
    exec(compile(open(chnlDefFileName, "r").read(), chnlDefFileName, 'exec'))
```
- **Channel Setup**: Loads channel definitions from a Python or IDV file.

#### Dynamic Simulation Start
```python
ierr = psspy.strt_2(1, outFileName)
```
- **Dynamic Simulation Initialization**: Starts the dynamic simulation with the specified output file.

#### Event Simulation (Contingencies)
```python
if eventFileName.lower().endswith(".py"):
    exec(compile(open(eventFileName, "r").read(), eventFileName, 'exec'))
```
- **Contingencies**: Executes event files (e.g., .idv or .py files).
---
### Step 5: Run Multiprocess
Once the `dynamicSimulation.py` is working, you can run the multiprocess by script `Run_dynamicSimulation_MultiProcess.py`.
#### **Why using multiprocess?**
1. **Faster Simulation Throughput**: Running multiple simulations in parallel significantly reduces the overall simulation time, especially when handling large batches of dynamic simulations or testing multiple scenarios (e.g., different P6 contingencies).

2. **Better Utilization of Hardware**: Multiprocessing takes full advantage of multi-core processors or distributed systems, allowing you to leverage available computational resources effectively, speeding up simulations and improving performance.
#### **Detailed Breakdown of the Script**

#### Main Function

The `main()` function is the entry point for running the tool. It processes any command-line arguments and initializes the graphical user interface (GUI). It also determines the working directory for the script and ensures everything is set up before running the simulations.

```python
def main():
    try:
        opts, args = getopt.getopt(sys.argv[1:], "h", ["help"])
    except getopt.error as msg:
        print(msg)
        print("for help use --help")
        sys.exit(2)
    for o, a in opts:
        if o in ("-h", "--help"):
            print(__doc__)
            sys.exit(0)

    app = wx.App()
    app.MainLoop()
    dirname = os.getcwd()
```

#### The `writeRunSimulationbatFile()` Function

This function generates a batch file that will run the dynamic simulation. The batch file includes the command to invoke the simulation, including the necessary paths and parameters.

```python
def writeRunSimulationbatFile(RunsimulationBatFileName, dynSimuEngine, configurationFile, studyName):
    RunSimulation = open(RunsimulationBatFileName, "w")
    RunSimulation.write('"C:\\PythonPath\\Python\\Python39\\python.exe" "%s" "%s" "%s"\n' % (dynSimuEngine, configurationFile, studyName))
    RunSimulation.close()
    return True
```

#### The `RunSimulationProcess()` Function

This function executes the batch file and waits for the simulation to complete. If the simulation finishes successfully, the batch file is removed.

```python
def RunSimulationProcess(RunsimulationBatFileName):
    ierr = subprocess.call([RunsimulationBatFileName])
    if ierr != 0:
        return False
    os.remove(RunsimulationBatFileName)
    return True
```

#### The `RunSimulationProcessParallel()` Function

This function enables the tool to run multiple simulations at the same time (in parallel), significantly speeding up the overall process.

```python
def RunSimulationProcessParallel(batfilenameList, max_processes=6, timeThreshold=600.0):
    processList = []
    procRuntimeDict = {}
    for each in batfilenameList:
        proc = subprocess.Popen(args=each, shell=False)
        processList.append(proc)
    while len(processList) >= max_processes:
        time.sleep(1)
        for eachP in processList:
            retCode = eachP.poll()
            if retCode is not None:
                processList.remove(eachP)
    return True
```

### Step 6: Plot results
#### The `create_batchidv_file_with_out_files()` function
This function automates the process of creating an IDV file for batch plotting .out files found in the output folder. Then you can run the Batch plot file in `PSSPLT` to plot results in a EPS file.
#### The `convert_eps_to_pdf()` function

This function uses **Ghostscript** to convert EPS files (used for graphical outputs) to PDF format for easy viewing and sharing.

---

## Conclusion

This tool is an efficient and user-friendly way to automate dynamic simulations, especially in the context of PSSE. It streamlines the simulation process, enabling parallel execution and easy conversion of output files to PDF. 

1. Defining channels and writing contingencies in Python offers significant advantages over traditional IDV files, primarily through increased flexibility, automation, and customization, the time required for preparation is notably reduced. 
2. The use of multiprocessing further enhances simulation efficiency by enabling parallel execution, which reduces simulation time and optimizes the utilization of computational resources. 
3. Together, these approaches contribute to more efficient, scalable, and adaptable simulation practices, ultimately improving performance and facilitating more complex analysis.

Whether you're a beginner or an experienced user, the tool will save you time and improve the accuracy of your simulations.