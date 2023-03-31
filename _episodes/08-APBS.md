---
title: "Continuum Electrostatics Calculations"
teaching: 20
exercises: 0
questions:
- "How to use sequence viewer?"
- "How to calculate RMSD between molecules? "
- "How to make trajectory movies?"
- "How to visualize electrostatic potential?"
- "How to make photorealistic pictures?"
objectives:
- "Learn some commonly used VMD extensions"
keypoints:
- ""
---

### APBS electrostatics
Surfaces, isosurfaces, and other representations can be colored by electrostatic potential. Any other volumetric properties such as density can be also used for coloring. Electrostatic potential calculations in VMD are done with APBS - Adaptive Poisson-Boltzmann Solver. APBS solves the equations of continuum electrostatics for biomolecular systems. This program must be made available to VMD by loading the `apbs` module. Below are the commands to make .pqr file from AMBER topology and restart files. These files are available in the workshop data. We will go though all the steps of system preparation later. 

APBS requires a PDB file with atomic partial charges and radii as an input. Such file can be easily prepared from a simulation topology file. 

~~~
module load vmd apbs ambertools
source $EBROOTAMBERTOOLS/amber.sh
cd ~/scratch/workshop/pdb/6N4O/simulation/setup
cpptraj prmtop.parm7
~~~
{: .language-bash}

~~~
trajin inpcrd.rst7
strip !(:860-898)
trajout nucleic.pqr pdb dumpq
go
~~~
{: .cpptraj}

VMD
1. Load .pqr file
2. Representation Surf or QuickSurf 
3. `Extensions` -> `Analysis` -> `APBS electrostatics` ->  `Run APBS`
4. When prompted `Load ABBS into top molecule`
5. Select coloring method `Volume`
6. Adjust `Color scale data range` in the `Trajectory` tab. Try [-50 50]
7. Electrostatic potential map pot.dx is saved in /tmp/apbs.xxxxx

To compute electrostatic potential we need a PQR file which is basically PDB file with charges and radii. This file can be made using PDB2PQR web server or utility programs from AMBERTOOLS module.

Considering that we will be learning AMBER, let's use AMBERTOOLS. There are two steps involved in creating PQR files from PDB files.

1. Using a force field and a pdb file, create a topology file
   ~~~
   module load gcc ambertools
   tleap -f leaprc.RNA.OL3
   ~~~
   {: .language-bash}
   ~~~
   rna=loadpdb bcl2-1.pdb
   saveamberparm rna bcl2-1.parm7 bcl2-1.rst7
   quit 
   ~~~
   {: .leap}
2. Utilizing the PDB and the topology file, create the PQR file.
   ~~~
   cpptraj bcl2-1.parm7
   ~~~
   {: .language-bash}
   ~~~
   trajin bcl2-1.rst7
   trajout  bcl2-1.pqr pdb dumpq
   go
   quit
   ~~~
   {: .leap}

### References
[VMD Introductory tutorial](https://doi.org/10.1002/0471250953.bi0507s24)

{% include links.md %}


