Readme file for SCALE-SDM to reproduce the results of AAS paper: *Resolving Entrainment–Mixing in Marine Stratocumulus: The Role of LES Grid Resolution and Super-Droplet Number* and GMD paper: *Simulation of marine stratocumulus using the super-droplet method: Numerical convergence and comparison to a double-moment bulk scheme using SCALE-SDM 5.2.6-2.3.1*.

Corresponding Author: Shin-ichiro Shima (s_shima@gsis.u-hyogo.ac.jp)

# General description
SCALE (Scalable Computing for Advanced Library and Environment), which stands for Scalable Computing for Advanced Library and Environment, is a basic library for weather and climate model of the earth and planets aimed to be widely used in various models.
The SCALE library is developed with co-design by researchers of computational science and computer science.
(http://scale.aics.riken.jp/)

The paths of the model setups for the sensitivity tests used in the paper are respectively as follows:

- [Oringinal SDM setup for grid convergence and SD number convergence tests](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai)
* [Oringinal SN14 setup for grid convergence tests](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_SN14_hokudai)
+ [No long wave radiation test](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_no_radiation)
- [Stronger long wave radiative cooling test](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_rad110)
* [SDM with artificial noise on SD motions](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02_add_noise/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_add_noise)
+ [SN14 with supersaturation limiter test](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02_SN14_Slimiter/scale-rm/test/case/shallowcloud/dycoms2_rf02_SN14_Slimiter)
- [SDM without supersaturation limiter test](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02_no_Slimiter/scale-rm/test/case/shallowcloud/dycoms2_rf02_SDM_no_Slimiter)
* [Adjustment of latent heat release](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02_LHmod/scale-rm/test/case/shallowcloud/dycoms2_rf02_SDM_LHmod)
+ [Turning off sedimentation](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02_no_sed/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_no_sed) (please keep `doprecipitation  = .true.` and `doautoconversion  = .true.` in [run.conf](https://github.com/wangyouyue/GMD_2022_code/blob/SDM_DYCOMSII-RF02_no_sed/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_no_sed/run.conf#L111)
- To turn off Collision-coalescence, just replacing `doautoconversion  = .true.` with `doautoconversion  = .false.` in [run.conf](https://github.com/wangyouyue/GMD_2022_code/blob/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai/run.conf#L198)
* To adjust the initial SD number per cell, just giving a positive number to the option `sdm_inisdnc` in [run.conf](https://github.com/wangyouyue/GMD_2022_code/blob/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai/run.conf#L132)
+ Adjusting intial aerosol number concentration in SN14 by change the value of the option `C_CCN` in [run.conf](https://github.com/wangyouyue/GMD_2022_code/blob/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/dycoms2_rf02_SN14/run.conf#L99)
- [SDM with horizontal SGS turbulence mixing tests](https://github.com/wangyouyue/GMD_2022_code/tree/SDM_DYCOMSII-RF02/scale-rm/test/case/shallowcloud/SDM_horiz_smg)

# Required software and supported environment
Fortran and C compiler are required to compile SCALE-SDM. MPI, NetCDF4, and HDF5 libraries are are also required.

The numerical experiments were conducted by using Intel Fortran/C compiler 2021.7.1, Intel MPI 2021.7.1, HDF5 1.14.0, NetCDF 4.9.0 and NetCDF-Fortran 4.6.0. For data analysis, NCL 6.6.2 was used.

# Set environment variable
`$ export SCALE_SYS=Linux64-intel-impi`

# Clean
```
$ cd scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai/
$ make allclean
$ make allclean SCALE_ENABLE_SDM=T SCALE_DISABLE_LOCALBIN=T SCALE_DYCOMS2_RF02_SDM=T
```

# Compile
## Using SDM
```
$ cd scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai/
$ make SCALE_ENABLE_SDM=T SCALE_DISABLE_LOCALBIN=T SCALE_DYCOMS2_RF02_SDM=T
$ ln -fsv  `grep ^TOPDIR Makefile | sed s/\)//g | awk '{print $NF}'`/bin/scale-rm* .
```

## Using the bulk model of Seiki and Nakajima (2014)
```
$ cd scale-rm/test/case/shallowcloud/dycoms2_rf02_sn14_hokudai/
$ make SCALE_DISABLE_LOCALBIN=T
$ ln -fsv  `grep ^TOPDIR Makefile | sed s/\)//g | awk '{print $NF}'`/bin/scale-rm* .
```

# Run a batch job on Hokudai supercomputer
Modify the job script according to your system. The job scheduler on Hokudai supercomputer is PJM ([for more information](https://www.hucc.hokudai.ac.jp/en_supercomputer/basic/en_job_execution/)).

`$ pjsub hokudai_run.sh`

# Run analysis program
## For SDM
```
$ cd scale-rm/test/case/shallowcloud/dycoms2_rf02_sdm_hokudai/
$ pjsub --step --sparam "sn=1" ncl.sh
$ pjsub --step --sparam "jid=JOB_ID, sn=2, sd=ec!=0:after:1" merge.sh
```

## For SN14
```
$ cd scale-rm/test/case/shallowcloud/dycoms2_rf02_sn14_hokudai/
$ pjsub --step --sparam "sn=1" ncl.sh
$ pjsub --step --sparam "jid=JOB_ID, sn=2, sd=ec!=0:after:1" merge.sh
```
