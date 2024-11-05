# Automation-Tools-in-work
Codes for automatic generation of the files and texts during work using Python.
## The Jupyter notebook "prepare_file" consists sections for:
1. Get the zone numbers in each county in Texas from ERCOT SSWG case and Planning Data Dictionary.
2. Create a class named "County" and functions needed, such as "get_area_zones". Create a dictionary named "Texas" with counties as keys.
3. Create subsystem file and monitor file for the project in a directory (create one if not existed) based on user input. Use functions defined in County class, need to run the previous section first.
4. Generate contigency file for the choosen base case year and season by selecting contingencies from ERCOT contigency files and combining by catergories. Need to run the previous section for the directory.
## The Jupyter notebook "write_report" consists sections for:
1. Find the bus information given the bus name or number in SSWG case.
2. Convert the bus SSWG name to the long name for a single bus. Need to run the first section.
3. Convert the bus SSWG name to the long name for a branch. Need to run the first section.
4. Convert contigency to SSWG form and long name form. Need to run the first section.
## The Jupyter notebook "vrt" consists sections for:
1. Check the PSSE and psspy function.
2. Create the folders needed for all VRT projects.
3. Defines functions to check if the project has storage, solar, or wind.
4. Defines function to create the ini file for the VRT in [DMView 3.3.1](https://sites.google.com/view/dmview/home). Need to run the 3rd section.
5. Define function to run the VRT in [DMView 3.3.1](https://sites.google.com/view/dmview/home) by codes. Need to run the 3rd section.
6. Define function to plot the VRT results include active power, reactive power and voltage at the project POI and terminal bus for all test settings.  Need to run the 3rd section.
7. Output the plots to Word and generate a report. Need to run the 3rd section.
