# ACE
(partial) reproduction of the results outlined in R. Drautz's 2019 paper.

Dataset acquired from pACE [1].

## Overview
```Cu_FHIaims_dataset_overview.ipynb``` contains exploratory data analysis of the dataset, and preparation of the dataset for the ACE fitting model. 

To train the model, you define an input configuration `input.yaml` (currently in `model/`) and train it using the TensorFlow backend provided as part of the `pyace` package. `input.yaml` defines the model parameters (cutoffs, basis order, training weights).

Run the command `pacemaker input.yaml` in the working directory to begin training.  When the model is done training, it outputs `output_potential.yace` that can be used for further steps. 

`fitting_data_info.pckl.gzip` contains the metadata information for the model train run. 

The model is currently incompletely trained, but you can look at `current_extended_potential.yaml` for the current progress. `current_extended_potential.yaml` is an incremental checkpoint of `output_potential.yace`.

Alternatively, you can look at `toy_model/` for a working example of a smaller functional model. 

### Testing the model: 
Currently Incomplete

To export to LAMMPS to test on custom $Cu$ structures, convert the `pyace` outputted `yaml` to a `yace` format readable by LAMMPS (through the `pair_style pace` interface): 
1. Build LAMMPS with ACE
2. Run the command: 

`pace_yaml2yace current_extended_potential.yaml`, 

Where you can replace `current_extended_potential` with your (completely trained) potential file. 
3. Create an input file for LAMMPS, including your model as: 

`pair_style pace`
`pair_coeff * * potential.pace Cu`

## Citations: 
1. Lysogorskiy, Y., Oord, C.v.d., Bochkarev, A. et al. Performant implementation of the atomic cluster expansion (PACE) and application to copper and silicon. npj Comput Mater 7, 97 (2021). https://doi.org/10.1038/s41524-021-00559-9
2. Drautz, R. (2019). Atomic cluster expansion for accurate and transferable interatomic potentials. Physical Review B, 99(1), 014104. https://doi.org/10.1103/PhysRevB.99.014104
