There are currently two versions of the WRF setup script: bash (discontinued) and python (current).
Running the setup scripts will automatically download the initial/boundary conditions
from the NCAR Research Data Archive (RDA), setup the running directory for WRF – including
generating symbolic links to the necessary executables and files – and then (optionally) 
running WRF's preprocessing utilities (WPS) `geogrid`, `ungrib`, and `metgrid` for real WRF cases.

# Jupyter notebook overview:
Within the Jupyter notebook, there will be a section that notes where the user modifications should
be made to variables such as: the run directory, the path to the executables, the path to any met_em
files (if linking), etc. After adjusting these lines, the script should create the working directory 
with the exact setup used within the published MMC study.

# Bash version overview:
This executable will set up a WRF run from downloading the necessary
Reanalysis data, to copying or linking over any executables and 
required WRF files, and generating an Eagle submission script.

To use:
1. Set up your namelist.wps file to generate the correct domain size,
   grid spacing, location, map projection, simulation time, etc.
   - You can use "ncl plotgrids_old.ncl" to plot the domains from
       the namelist.wps file to make sure it looks like what you want.
   - Once this is done, make the changes to the
       namelist.wps.template file so that it will be included in the
       executable script. YOU MUST MAKE THE CHANGES TO THE TEMPLATE
       FILE IN ORDER FOR IT TO BE USED IN THE SIMULATION!
   
2. Set up your namelist.input.template to use the right grid numbers,
   grid spacing, parent_ij_start, and other variables from the WPS 
   step (above).
   Also, make any changes to the parameterization schemes (if desired).

3. Open the setUp_WRF.sh and change the user variables in the header
   specifying the start date, number of hours to simulate, which
   Reanalysis dataset to use, number of domains, etc.
   - Currently, the ERA-Interim and GFS datasets can be downloaded 
       automatically. For ERA-5 and NARR, you will need to download 
       the data manually.
       - NARR: https://rda.ucar.edu/datasets/ds608.0/#!access you will need an RDA account.
       - ERA-5: follow directions here:
         https://wiki.nrelcloud.org/wiki/index.php/TheWiki/WRF#Downloading_data_and_running_WRF_with_ERA5
   - Make sure the HOME_DIR, WPS_DIR, ICBC_DIR, EXE_DIR, and OUT_DIR
       are set correctly. See comments in code for descriptions of
       each path.

4. Run setUp_WRF.sh
   - This will only run the WRF Pre-processing System (WPS) steps
       and setup the simulations to run, but not run the simulations
   - To run the simulations, simply execute:
       - "sbatch submit_real.sh" to run real.exe
       - "sbatch submit_wrf.sh" to run wrf.exe
