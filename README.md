# Automation Tools in Work

This repository contains Python code and Jupyter notebooks for automating the generation of system files, contingency data, and reports for power grid analysis, particularly focused on ERCOT (Electric Reliability Council of Texas). The tools include functions to process grid data, create reports, and run simulations using VRT (Voltage and Reactive Testing) tools in DMView 3.3.1.

The project is organized into different folders and notebooks, each designed to handle specific tasks such as mapping, file preparation, report generation, and VRT simulations.

## Table of Contents
- [Automation Tools in Work](#automation-tools-in-work)
  - [Table of Contents](#table-of-contents)
  - [Installation](#installation)
  - [Overview](#overview)
  - [Folders and Notebooks](#folders-and-notebooks)
    - [AEP Steady State](#aep-steady-state)
      - [`draw_map`](#draw_map)
      - [`prepare_file`](#prepare_file)
      - [`write_report`](#write_report)
    - [AEP Dynamic](#aep-dynamic)
      - [`vrt`](#vrt)
  - [Contributing](#contributing)

## Installation

To run the code in this repository, you will need Python and several dependencies installed. The steps for installation are as follows:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Arthurliyaze/Automation-Tools-in-work.git
   cd Automation-Tools-in-work
2. **Install dependencies**:
   Install the required Python packages using pip. It is recommended to use a virtual environment.
   
   The requirements.txt file should include: 
   
   pandas, numpy, matplotlib, psspy (for PSSE functions), python-docx (for generating Word reports).
   
3. **Install DMView 3.3.1**:
   For running the VRT simulations, download and install [DMView 3.3.1](https://sites.google.com/view/dmview/home) following the instructions on their website.
   
## Overview

This project automates several tasks related to power grid modeling, focusing on the ERCOT grid. The main functionalities include:

- **Mapping**: Generate county-level maps for Texas and their adjacency information.
- **File Preparation**: Automatically create subsystem, monitoring, and contingency files for grid analysis.
- **Report Generation**: Convert bus names, find bus information, and generate contingency reports.
- **VRT Simulation**: Run voltage and reactive power testing (VRT) simulations, plot results, and generate reports.

The project is split into two primary categories: **AEP Steady State** (for static grid data) and **AEP Dynamic** (for dynamic VRT simulations).

## Folders and Notebooks

### AEP Steady State

This folder contains Jupyter notebooks for preparing static grid data and generating reports.

#### `draw_map`
This notebook automates the generation of a county map for Texas:
1. **Fetch County Data**: Retrieves information about counties in Texas from [Wikipedia](https://en.wikipedia.org/wiki/List_of_counties_in_Texas) and their adjacent counties.
2. **Create County Map**: Generates a colored study area map using [Paintmaps](https://paintmaps.com/map-charts/272/Texas-map-chart) and saves it as a PDF in the `download_map` folder.

#### `prepare_file`
This notebook helps automate the creation of files needed for grid modeling:
1. **Extract Zone Data**: Retrieves the zone numbers for each Texas county from the ERCOT SSWG case and the Planning Data Dictionary.
2. **County Class**: Defines a `County` class with functions like `get_area_zones` to manage zone data. A dictionary named `Texas` is created with counties as keys.
3. **Generate Subsystem and Monitor Files**: Based on user input, creates subsystem and monitor files for a project in a specified directory (if the directory doesn't exist, it is created). This step requires the previous section to be run.
4. **Create Contingency Files**: Generates contingency files for a selected base case year and season by extracting and combining contingency data from ERCOT files.

#### `write_report`
This notebook automates the generation of reports based on grid data:
1. **Find Bus Information**: Retrieves bus details based on bus name or number from the ERCOT SSWG case.
2. **Convert Bus Names**: Converts bus names from the SSWG format to long names for individual buses.
3. **Convert Branch Names**: Converts bus names from the SSWG format to long names for branches.
4. **Contingency Reports**: Converts contingency data to both SSWG format and long name format for inclusion in reports.

### AEP Dynamic

This folder contains Jupyter notebooks for dynamic simulation tasks related to voltage and reactive power testing (VRT).

#### `vrt`
This notebook helps automate the VRT project setup, testing, and result generation:
1. **PSSE & `psspy` Check**: Verifies that the PSSE and `psspy` functions are working correctly.
2. **Project Folder Setup**: Creates all necessary folders for the VRT project.
3. **Resource Type Check**: Defines functions to check if the project includes storage, solar, or wind resources.
4. **Generate VRT `.ini` File**: Creates the `.ini` configuration file required to run the VRT simulation in **DMView 3.3.1**. The third section must be run first.
5. **Run VRT Simulation**: Defines a function to run the VRT simulation in **DMView 3.3.1** via code, using the generated `.ini` file. The third section must be run first.
6. **Plot VRT Results**: Plots the VRT results, including active power, reactive power, and voltage at the Point of Interconnection (POI) and the terminal bus for all test settings. The third section must be run first.
7. **Generate Report**: Exports the plots to a Word document and generates a comprehensive report, including the results of the VRT simulations. The third section must be run first.

## Contributing

Contributions are welcome! To contribute to this project, please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Make your changes and commit them.
4. Push your changes to your fork and create a pull request.