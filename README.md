## GROMACS-on-Colab

This repository provides four notebooks for running molecular dynamics simulations on Google Colab.
* `Build_to_Google_Drive.ipynb` \
  Installs GROMACS to your Google Drive, where it can be loaded by the other notebooks. \
  [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bioinfkaustin/gromacs-on-colab/blob/main/Build_to_Google_Drive.ipynb)

* `GROMACS_for_CHARMM-GUI.ipynb` \
  Equilibrates a system prepared with [CHARMM-GUI](https://www.charmm-gui.org/). This notebook unpacks a `CHARMM-GUI.tgz` archive from "Solution Builder." Optionally, it can merge in an archive from "Ligand Reader," allowing for piecewise preparation of protein-ligand systems. \
  [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bioinfkaustin/gromacs-on-colab/blob/main/GROMACS_for_CHARMM-GUI.ipynb)

* `GROMACS_for_production.ipynb` \
  Starts or resumes a production simulation. \
  [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bioinfkaustin/gromacs-on-colab/blob/main/GROMACS_for_production.ipynb)

* `Trajectory_analysis_tools.ipynb` \
  Calculates data that can be derived from a production simulation trajectory, such as centroid structures, RMSDs, and interaction energies. \
  [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bioinfkaustin/gromacs-on-colab/blob/main/Trajectory_analysis_tools.ipynb)
  
All inputs and outputs are stored on your Google Drive.
