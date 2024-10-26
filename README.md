# Automation-Tools-in-work
Codes for automatic generation of the files and texts during work using Python.
## The Jupyter notebook "prepare_file" consists sections for:
1. Get the zone numbers in each county in Texas from ERCOT SSWG case and Planning Data Dictionary.
2. Create a class named "County" and functions needed, such as "get_area_zones". Create a dictionary named "Texas" with counties as keys.
3. Create subsystem file and monitor file for the project in a directory (create one if not existed) based on user input. Use functions defined in County class, need to run the previous section first.
4. Generate contigency file for the choosen base case year and season by selecting contingencies from ERCOT contigency files and combining by catergories. Need to run the previous section for the directory.
## The Jupyter notebook "write_report" consists sections for:
1. Find the bus information given the bus name or number in SSWG case.
2. Convert the bus SSWG name to the long name for a single bus.
3. Convert the bus SSWG name to the long name for a branch.
4. Convert contigency to SSWG form and long name form.
## The Jupyter notebook "vrt" consists sections for:
1. Create the folders needed for all VRT projects.
2. Defines functions to check if the project has storage, solar, or wind.
3. Defines function to create the ini file for the VRT in [DMView 3.3](https://sites.google.com/view/dmview/home)
4. Define function to run the VRT in [DMView 3.3](https://sites.google.com/view/dmview/home) by codes.
5. Define function to plot the VRT results include active power, reactive power and voltage at the project generator bus for all test settings.
6. Output the plots to Word and generate a report.
