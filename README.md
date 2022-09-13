# TauLock_CESM

This directory provides modified source code files for CESM 1.2.2.1 to implement the wind stress overriding protocol introduced in Luongo et al. (2022). DOI: https://doi.org/10.1175/JCLI-D-21-0950.1. When using this source code, please cite the following reference:

Luongo, Matthew T., Shang-Ping Xie, and Ian Eisenman. "Buoyancy Forcing Dominates the Cross-Equatorial Ocean Heat Transport Response to Northern Hemisphere Extratropical Cooling." Journal of Climate (2022): 1-46.

Here, I provide a quick README to make heads or tails of these mods. If you want to see the few dozen lines out of the many thousands of lines of base code that I changed to create this protocol, I suggest just searching "Luongo" in the files. I tried to comment near each line with my name and the date. 

First, you'll notice there are two sub-directories, CPLOutput and TauLock. I'll go through each of these below sequentially. 

1. CPLOutput: In order to lock wind stress in CESM, you need to output wind stress from a case first. Let's say you call this case "ControlCase." After you run ./cesm_setup but before you run ./ControlCase.clean_build, you need to CPLOutput/src.drv/ccsm_comp_mod.F90 to your SourceMods/src.drv directory in the case directory. After you build and run this case, the introduced mod should output a file every simulated day called ControlCase.cpl.x2o_oo.YY-MM-DD-01800.nc. These files will be in your /glade/scratch/USER/ControlCase/run/ directory, are about 35M per day, and include all variables that the coupler passes to the ocean. 

2. TauLock: Now that you've output daily wind stress from ControlCase, you'll want to input it into ExperimentalCase. After set-up, copy TauLock/src.drv and TauLock/src.pop2 to your SourceMods/src.drv and SourceMods/src.pop2 directories in the caseroot, respectively. Keep in mind that there are some specifics you'll need to change in ccsm_comp_mod.F90 -- I've written USER and OUTPUTCASENAME in the file, but you'll need to correct the path and the file names for your case. These mods will take in the output wind stress from ControlCase at each coupling step (around 12:30am in the modeled day I believe) and exactly specify wind stress over the ocean in ExperimentalCase. 

Note that these mods of course don't change some of the other specifics for your CESM1.2 case (resubmission, run type, compset, etc.). I usually write a big bash script to do it all in a streamlined flow.

Please reach out to me at mluongo(at)ucsd(dot)edu if you have any questions about these code modifications or the workflow.

Matt Luongo
Scripps Institution of Oceanography
University of California, San Diego

09/13/2022
La Jolla, CA 

