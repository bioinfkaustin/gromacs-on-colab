## Changelog

### 2023-10-06

New features:

* **Tutorial:** the `README.md` file now contains a quick-start guide describing how to run a protein-ligand simulation

Bugfixes:

* **System temperature in CHARMM-GUI:** when a temperature different to the default (i.e. not 303.15 K) is chosen for the protein in Solution Builder or Membrane Builder, this custom temperature is now correctly carried through to the production simulation when `GROMACS_for_CHARMM-GUI.ipynb` makes the project folder and its `grompp.mdp`

### 2023-09-29

New features in **`Build_to_Google_Drive.ipynb`**:

* **Prebuilt GROMACS**: instead of compiling GROMACS from source code, the installation notebook downloads prebuilt binaries from this GitHub repository when able, addressing issues [#1](https://github.com/bioinfkaustin/gromacs-on-colab/issues/1) and [#2](https://github.com/bioinfkaustin/gromacs-on-colab/issues/2)

In **`GROMACS_for_CHARMM-GUI.ipynb`**:

* **Double equilibration length**: option to extend equilibration to treat more difficult systems
* **Charge balancing**: if for any reason the final merged system has a net charge, $\mathrm{K}^+$ or $\mathrm{Cl}^-$ ions are automatically added to neutralise it

Updates to existing features:

* **GPU optimisation:** bonded interactions are moved to the GPU such that compatible simulations are now fully resident on the GPU; and in the production notebook, [CUDA graphs](https://developer.nvidia.com/blog/a-guide-to-cuda-graphs-in-gromacs-2023) are enabled when compatible
* **Calculate centroid structures:** a convenient `.pdb` is saved alongside the `.gro`

Bugfixes:

* **Checkpointing in Google Drive:** instead of uploading the entire trajectory each time, only "part-files" are periodically uploaded, addressing issue [#3](https://github.com/bioinfkaustin/gromacs-on-colab/issues/3)

### 2023-06-28

New features:

* **Installation notebook:** the download and installation processes for software dependencies (e.g. GROMACS) have moved from the start of each notebook to a new notebook, **`Build_for_Google_Colab.ipynb`**
* **Analysis notebook:** the tool to plot the RMSD of the ligand has moved from an automated step at the end of the production notebook to being a component of a new interactive notebook, **`Trajectory_analysis_tools.ipynb`**, which has many new features 

In **`GROMACS_for_CHARMM-GUI.ipynb`**:

* **GPU optimisation:** hydrogen interleaving to address [this update groups bug](https://gromacs.bioexcel.eu/t/gpu-update-giving-error-with-protein-ligand-complex/5925); change production `.mdp` to use the GPU compatible V-Rescale temperature coupling scheme

In **`GROMACS_for_production.ipynb`**:

* **Human-interpretable trajectory output:** at the end of a production simulation, outputs a postprocessed trajectory file, `..._reference.xtc`, in the reference frame of the protein (such that it does not translate or rotate)

In **`Trajectory_analysis_tools.ipynb`**:

* **Calculate centroid structures:** uses the built in clustering method in GROMACS to calculate the centroid structure (the most-average representative of the largest cluster) for a given timespan
* **Ligand RMSD plotting:** see below; now the reference coordinates may be provided by any frame or centroid, not just the first frame 
* **Calculate protein-ligand energy terms:** interaction energies are calculated along the trajectory outputted by the production simulation
* **Simulate a solvent-ligand system and calculate its energy terms:** the ligand is simulated in a box of water, and then interaction energies are calculated along the resulting trajectory 

Updates to existing features:

* **Membrane detection:** changed to detect lipid residues automatically based on the index groups (`.ndx`) provided by CHARMM-GUI Membrane Builder
* **Ligand coordinate merging:** changed to use [Biopython's superposition module](https://biopython.org/docs/1.75/api/Bio.PDB.Superimposer.html) instead of UCSF Chimera
* **Ligand topology generation:** use the cgenff\_charmm2gmx\_py3\_nx2 utility by default, as it is robust to a wide range of ligand chemistry
* **Improved ligand topology compatibility:** the production notebook can now handle more complex custom topologies present in the `.top` file 

Removed functionality:

* **_z_-axis walls:** since any practical use-case would require manual modification of the simulation parameters (`.mdp`), at which point the walls directive could also be added, this was not useful as an automated tool

### 2023-03-09

This first release of the toolkit has the following features.

In **`GROMACS_for_CHARMM-GUI.ipynb`**, which equilibrates systems prepared in [CHARMM-GUI](https://www.charmm-gui.org/):

* **Installation:** dependencies are loaded from Google Drive, or downloaded and installed if the notebook is being run for the first time
* **CHARMM-GUI compatibility:** a protein system is taken from a CHARMM-GUI Solution Builder or Membrane Builder archive (`.tgz`)
* **Membrane detection:** if present, names of lipid residues must be provided in order to build index groups (`.ndx`) compatible with the extended equilibration process generated by CHARMM-GUI
* **Ligand coordinate merging:** optionally, the docked coordinates file (`.mol2`) is taken from the CHARMM-GUI Ligand Reader archive (`.tgz`) and superposed onto the protein system using [UCSF Chimera](https://www.cgl.ucsf.edu/chimera/) MatchMaker, after which corresponding modifications are made to the coordinates file of the system
* **Ligand topology merging:** optionally, the topology (`.itp`) is taken from the CHARMM-GUI Ligand Reader archive (`.tgz`) and merged with the protein to create a protein-ligand complex simulation system
* **Ligand topology generation:** if CHARMM-GUI Ligand Reader was not able to generate a GROMACS-compatible ligand topology (`.itp`), the included CHARMM-compatible topology is converted using the [cgenff\_charmm2gmx\_py3\_nx2](http://mackerell.umaryland.edu/charmm_ff.shtml#gromacs) utility
* **Constrained equilibration:** if a ligand has been merged in, then it is constrained at the start of equilibration and those constraints are gradually relaxed as the simulations proceed, as per the equilibration protocol suggested by CHARMM-GUI
* **_z_-axis walls:** optionally, add graphene walls to the top and bottom of the simulation system's _z_-axis

In **`GROMACS_for_production.ipynb`**, which runs production simulations:

* **Installation:** see above
* **Checkpointing in Google Drive:** by periodically uploading a copy of the simulation output files (e.g. `.trr`, `.cpt`) to Google Drive, production simulations can be resumed or extended even if the Google Colab session ends unexpectedly
* **Early stopping:** if a specified group, typically the "LIG" residue containing a ligand molecule, exceeds a threshold RMSD when compared to its initial coordinates, then the simulation can stop early -- one might use this to optimise the [virtual screening protocol by H. Guterres & W. Im](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7534544/)
* **Ligand RMSD plotting:** relevant plots are automatically produced using Matplotlib

