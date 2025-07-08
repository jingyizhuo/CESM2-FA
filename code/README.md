# CESM2 Flux Adjustment Instructions
### 🌍 How to set up flux adjustment experiments

This page describes how to set up surface flux adjustment experiments using the fully coupled CESM2 as an example. The source code modifications provided here are specific to CESM2. If you wish to apply flux adjustments to CESM1 or other GCMs, the overall logic remains the same, but you will need to implement your own version of the source code modifications.

---

### 1️⃣ Step 1: Apply SST nudging, and save ```TAU_ADJUST```

The first step is to apply SST nudging or the pacemaker technique. This allows you to diagnose and save the surface wind stress adjustments (```TAU_ADJUST``` ) needed for correcting surface momentum fluxes.

In my case, I followed the [CESM2 Pacemaker Tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) to implement SST nudging. The [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) provides guidance on how to generate a custom nudging mask. In our JCL2025 paper, we applied SST nudging over the entire tropical band (30°S–30°N), you can define your own nudging region based on your specific research goals. Then you will need to prepare the SST forcing data, apply source code modifications, set up POP namelist, and then set up and build your case. Please just follow the steps 1 to 7 of [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) to finish the 1st step of flux adjustment. I’ve also included my source codes at [cesm2fa_step1](https://github.com/jingyizhuo/CESM2-FA/tree/main/code/cesm2fa_step1), but they are identical to the original version provided in the [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions).

After completing the SST nudging simulations, the model output is compared to observations to calculate the climatological momentum flux terms (taux_adj, tauy_adj), defined as the monthly climatological differences in zonal and meridional wind stress between ERA5 and the simulations. 

### 2️⃣ Step 2: Add wind stress adjustment while applying SST nudging, and save ```SST_ADJUST```

With ```TAU_ADJUST``` applied in CESM2, the model will evolve toward a new mean climatological state. Step 2 aim to obtain the climatological SST adjustment when ```TAU_ADJUST``` is applied. 

Assuming your ```TAU_ADJUST``` data from Step 1 have been saved at */my/cesm2fa_data/tau/*, then you will need to copy the necessary source code modifications into your POP SourceMods directory. Similar to Step 1, this involves modifying four POP Fortran source files: ```forcing_coupled.F90```; ```forcing.F90```; ```forcing_sfwf.F90```; ```forcing_shf.90```. In addition, you must update the POP namelist definitions file: ```namelist_definition_pop.xml```. All these files can be found at [cesm2fa_step2](https://github.com/jingyizhuo/CESM2-FA/tree/main/code/cesm2fa_step2).

Unlike in Step 1, where the wind stress adjustments are diagnosed after completing the simulation, ```SST_ADJUST``` is calculated by taking the climatological mean of the SST nudging tendency term (```HEAT_F```) saved during the model run. ```HEAT_F``` is not a default output of POP, I've modify ```forcing_coupled.F90``` accordingly to save this variable. Just make sure to include ```HEAT_F``` in your ```gx1v7_tavg_contents``` file to ensure it is properly written to the output.


### 3️⃣ Step 3: Apply both ```TAU_ADJUST``` and ```SST_ADJUST``` to get the CESM2-FA

Now, with the derived wind stress and sst adjustments, you will also need to modify your source codes, namelists and xml parameters, then finally build and submit your case as normal. 

The source codes for Step 3 can be found at [cesm2fa_step3](https://github.com/jingyizhuo/CESM2-FA/tree/main/code/cesm2fa_step3). 

---

Citation: Zhuo, J.-Y., C. Lee, A. Sobel, R. Seager, S. J. Camargo, Y. Lin, B. Fosu, and K. A. Reed, 2025: A More La Niña–Like Response to Radiative Forcing after Flux Adjustment in CESM2. *J. Climate*, **38**, 1037–1050, https://doi.org/10.1175/JCLI-D-24-0331.1







