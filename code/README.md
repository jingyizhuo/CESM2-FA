# CESM2 Flux Adjustment Instructions
### 🌍 How to set up flux adjustment experiments

This page describes how to set up surface flux adjustment experiments using the fully coupled CESM2 as an example. The source code modifications provided here are specific to CESM2. If you wish to apply flux adjustments to CESM1 or other GCMs, the overall logic remains the same, but you will need to implement your own version of the source code modifications.

### 1️⃣ Step 1: Apply SST nudging, and save ```taux_adj```, bash```taux_adj```

The first step is to apply SST nudging or the pacemaker technique. This allows you to diagnose and save the surface wind stress adjustments (bash```taux_adj``` and bash```tauy_adj```) needed for correcting surface momentum fluxes.

In my case, I followed the [CESM2 Pacemaker Tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) to implement SST nudging. The [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) provides guidance on how to generate a custom nudging mask. In our JCL2025 paper, we applied SST nudging over the entire tropical band (30°S–30°N), you can define your own nudging region based on your specific research goals. Then you will need to prepare the SST forcing data, apply source code modifications, set up POP namelist, and then set up and build your case. Please just follow the steps 1 to 7 of [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions) to finish the 1st step of flux adjustment.

Here I also attach the [source codes](https://github.com/jingyizhuo/CESM2-FA/tree/main/code) which are the same as the one used in the [tutorial](https://www.cesm.ucar.edu/working-groups/climate/simulations/cesm2-pacific-pacemaker/instructions).




