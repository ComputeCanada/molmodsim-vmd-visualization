---
title: "Continuum Electrostatics Calculations"
teaching: 10
exercises: 0
questions:
- "How to compute and visualize electrostatic potential?"
objectives:
- "How to use APBS electrostatics plugin?"
keypoints:
- ""
---

### APBS electrostatics
Surfaces, isosurfaces, and other representations can be colored by electrostatic potential. Any other volumetric properties such as density can be also used for coloring. Electrostatic potential calculations in VMD are done with the Adaptive Poisson-Boltzmann Solver (APBS). APBS solves the equations of continuum electrostatics for biomolecular systems. This means that it computes electrostatic potential for a solvated protein This program must be made available to VMD by loading the `apbs` module.

To compute electrostatic potential we need a pqr file which is basically pdb file with charges and radii. This file can be made using PDB2PQR web server or utility programs from the `ambertools` module.

Considering that we will be learning AMBER, let's use ambertools. There are two steps involved in creating pqr files from pdb files.

1. Using a force field and a pdb file, create a topology file
   ~~~
   module purge
   module load StdEnv/2020 gcc ambertools
   cd ~/workshop/pqr
   tleap -f leaprc.RNA.OL3
   ~~~
   {: .language-bash}
   ~~~
   rna=loadpdb bcl2-1.pdb
   saveamberparm rna bcl2-1.parm7 bcl2-1.rst7
   quit 
   ~~~
   {: .leap}
2. Use the pdb and the topology file to create the pqr file.
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

3. Load apbs and vmd modules
~~~
module purge 
module load StdEnv/2020 apbs vmd
~~~
{: .language-bash}

VMD
1. Load `bcl2-1.pqr.pqr` 
2. Create a `QuickSurf` or `Surf` representation
3. `Extensions` -> `Analysis` -> `APBS electrostatics`. Under `Edit` you can change calculation settings such as temperature, ion concentration, and dielectric constants. Then `Run APBS`
4. When prompted choose `Load APBS into top molecule`
5. Select coloring method `Volume`
6. Adjust `Color scale data range` in the `Trajectory` tab. Try [-10 10]
7. By default potential map `pot.dx` is saved in `/tmp/apbs.xxxxx`. You can change the directory in the top bar `Edit`->`Settings`.

### References
[VMD Introductory tutorial](https://doi.org/10.1002/0471250953.bi0507s24)

{% include links.md %}


