---
title: "Visualizing MD trajectories"
teaching: 20
exercises: 0
questions:
- "How to load structure files and trajectories into VMD?"
- "How to animate trajectories?"
- "How to generate RMSD data from a trajectory?"
objectives:
- "Learn how to load molecular structure files and MD trajectories."
- "Learn how to animate trajectories?"
keypoints:
- ""
---

### Get workshop example data
On the training cluster:
~~~
cd ~/scratch
cp /tmp/workshop.tar.gz .
tar -xf workshop.tar.gz 
~~~
{: .language-bash}

On any other computer:
~~~
curl -OL https://github.com/ComputeCanada/molmodsim-amber-md-lesson/releases/download/workshop-2021-04/workshop.tar.gz
~~~
{: .language-bash}

### Loading trajectory files
A trajectory file contains the coordinates for all atoms over the course of a simulation. Normally, not every time step of the simulation is saved, since it would create a huge file. It is common to save coordinates every 1000 to 5000 steps. All of these coordinates allow us to measure the dynamic properties of our simulation experiments, including secondary-structure evolution, diffusion constants, correlations between groups, etc

#### Loading structure files
To load a trajectory, we need both the structure and the trajectory file. 
First load a structure file as a new molecule. VMD can read structure files in different formats, such as AMBER7 Parm, XPLOR PSF, GROMACS GRO, PDB, etc.). File types are recognized by extension. If a file has a non-standard extension you can select format manually.

It is best to use structure files that contain connectivity (topology, mol2) information whenever possible. In the absence of connectivity information, VMD uses distances between atoms to determine which ones are connected. It does not work perfectly all the time. Stretched bonds may go undetected, and there may be incorrect bonds formed between non-bonded atoms that clash. If you use the wrong bonds, your visualization will be incorrect. The automatic bond determination can be disabled when loading structure files:

~~~
mol new 1si4.pdb autobonds off
~~~
{: .vmd}

Our training dataset is located in the directory `workshop/pdb/6N4O/simulation/sim_pmemd/4-production`. As a structure file we will be using AMBER7 parameter file `prmtop_nowat.parm7`. Change into this directory and load the topology file.

Once a molecular structure has been loaded you can add a trajectory to it: highlight the molecule, go to `File`->`Load Data into Molecule` and choose `mdcrd_nowat.xtc`. It is a long trajectory with 3000 frames. To make loading faster you can load every 5th frame.

#### Loading trajectory using commands on MAC/Linux
~~~
cd ~/scratch/workshop/pdb/6N4O/simulation/sim_pmemd/4-production
ml vmd
vmd
~~~
{: .language-bash}
~~~
mol new prmtop_nowat.parm7
mol addfile mdcrd_nowat.xtc step 5
~~~
{: .vmd}

#### Viewing AMBER-NetCDF trajectories on Windows and MAC.
NetCDF trajectory files, which are AMBER's default format, can only be loaded on Linux. You can convert them to GROMACS XTC format by using the CPPTRAJ program from AMBER.

~~~
module load ambertools
cpptraj prmtop_nowat.parm7
~~~
{: .language-bash}
~~~
trajin mdcrd_nowat.nc
trajout mdcrd_nowat.xtc
go
~~~
{: .cpptraj}

#### Visualizing trajectories
- To make trajectory animation run smoother you can interpolate coordinates:   
`Graphical representations`->`Trajectory`->`Trajectory Smoothing Window Size`    
- You can also visualize periodic images:   
`Graphical representations`->`Periodic`  
-  You can visualize multiple frames and color them by trajectory step. Try resid 809 to 859.
![](../fig/multiple_frames.png){:width="480"}

### RMSD Trajectory Analysis
The RMSD is a numerical measurement of the difference between two structures: a target structure and a reference structure. Our interest in molecular dynamics is in how structures and parts of structures change over time. For example, a plot of RMSD vs. time will reveal the opening and closing of gates on a protein, such as a transporter. When compared with the starting point, the RMSD can identify protein structure changes and study stability of the simulated system. As the RMSD curve flattens or levels off, that can be a sign that the system has equilibrated.

You can calculate the time dependence of RMSD in a molecular dynamics simulation using the RMSD Trajectory Tool. It is located under `Extensions` -> `Analysis` -> `RMSD Trajectory Tool`

1. Load a trajectory
1. Start `RMSD Trajectory Tool` and add a molecule: `Add active`
2. Make a selection of atoms for which you want to calculate RMSD
    - use all protein atoms (normally you don't want to include hydrogens)
    - use all nucleic acid atoms
    - use a specific part ot the system (e.g. resid 20 to 80)
    - limit selection to only backbone atoms
3. `Align` all frames (you can choose the reference frame or a reference molecule).
4. You can save rmsd in a file so you can make a nice figure with your favorite plotting software, and check `Plot` box to view the result.
 
Exercise: compute RMSD of the RNA bound to the protein. What can you tell about its dynamics from the RMSD plot?

{% include links.md %}

