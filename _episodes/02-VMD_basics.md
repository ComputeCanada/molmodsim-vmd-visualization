---
title: "Loading Structure Files and Interacting with Molecules"
teaching: 20
exercises: 0
questions:
- "How to load PDB files and trajectories into VMD?"
- "How to view molecules interactively?"
- "How to measure distances, angles and dihedrals?"
objectives:
- "Learn VMD basics"
keypoints:
- ""
---

### Opening PDB files
As an example will be using X-ray crystallographic structure of human hemoglobin 1SI4.

####  Downloading files from Protein Data Bank
VMD can directly query PDB database.
![VMD-Main > Extensions > Data > PDB Database Query]({{ page.root }}/fig/pdb_tool.png){:width="400"}

Enter "1si4" and click `Load into new molecule in VMD`.  

![Hemoglobin rendered as sticks]({{ page.root }}/fig/hemoglobin-1.png){:width="400"}

#### Using commands
All functions available from GUI and much more are available as commands. The command line interface allows to use more options than available from GUI. VMD has built-in TCL programming language, which is available from the command line. This interpreter gives you ability to write your own processing and visualization functions utilizing any existing VMD functions and variables. 

Commands can be entered in the terminal window where you typed "vmd", or in `TC console` available in the `Extensions` tool bar. For description of all available commands see [Help on Tcl Text Commands](https://www.ks.uiuc.edu/Research/vmd/vmd-1.9.4/ug/node121.html)

Let's use our first command `pdbload`. This command will download and open a PDB file:

~~~
mol pdbload 1si4
~~~
{: .vmd}

Note: loading files from PDB is not possible from compute nodes on Graham and Beluga because they are not connected to the Internet. Use the login nodes on these systems to load PDB files.

#### Opening a PDB file saved in your computer.
On the `VMD Main` window menu select `File` -> `New Molecule`. This will open the `Molecule File Browser`. Choose a molecule file.

### Interacting with molecules
#### Obtaining good views for molecules
Rotate, zoom in/out, translate, and set rotation center to get a desired view. Pressing any of these keys switches the mode of interaction. For example, when `s` key is pressed mouse cursor will change. In this mode click, hold and move on a trackpad/mouse will zoom in/out.   
If you are lost simply press the `=` key to reset view.

| Action        | Hot keys   ||||||||||||||||||||||
----------------|---------
| Rotate        |     r      
| Zoom          |     s                  
| Translate     |     t      
| Reset View    |     =      
| Set Center    |     c

- Important Display parameters:
   - Rendermode. The GLSL mode turns on OpenGL extensions implementing programmable shading and transparency for higher quality molecular graphics. These features are not enabled by default.  

   - Projection

#### Working with Graphical Representations  
- Navigate to `Graphics`->`Representations`  
- Change `Drawing Method` to `New Cartoon`   
- Change `Coloring Method` to `Chain`  
- Try to get a better illumination by moving lights: `Mouse`->`Move Light`

![Hemoglobin rendered as "New Cartoon"]({{ page.root }}/fig/hemoglobin-2.png){:width="400"}

- Create a representation
- Double click to turn representations on/off without deleting them
- Changing draw styles 
- Most useful draw styles: `NewCartoon`, `QuickSurf`, `Surf`, `Licorice`, `VDW`, `HBonds`
- Selecting atoms, lots of selection options are available. 
- Most often used selection keywords are `noh`, `backbone`, `protein`, `nucleic`, `resname`, `resid`, `index`
- Selecting a a residue: `residue` (starts from 0), `resid` (starts from 1)
- Selecting an atom: `index` (starts from 0), `serial` (starts from 1)

![Hemoglobin rendered as backbone with transparent surface]({{ page.root }}/fig/hemoglobin-3.png){:width="400"}

>## Challenge
>Reproduce the following figure
>
>![Hemoglobin protein with ligand]({{ page.root }}/fig/hemoglobin.png){: width="480"}
>>## Solution
>>select chain A  
>>`chain A` 
>>new cartoon, iceblue, aochalky  
>>
>>select HEM  
>>`chain A and resname HEM`  
>>licorice, colorid 3, aochalky  
>>
>>Select atom FE  
>>`chain A and name FE`  
>>VDW scale 0.4, colorid 1, aochalky  
>>
>>Select only FE ligands   
>>`not resname HEM and same residue as within 3 of name FE and chain A`  
>>licorice, colorid 4, aochalky  
>>
>>The previous command selected two FE ligands. How do we select only the small 2-atom FE ligand?  
>>Pick any atom from this molecule. All information about the atom will be printed in the VMD terminal window.
>>
>>select CYN    
>>`chain A and resname CYN`  
>>surf, colorid 4, aochalky  
>{: .solution}
{: .challenge}

- Loading structure files
   - More than 70 supported file types.
   - File types are recognized by extension. If a file has a non-standard extension select format manually.

- Loading trajectories.
    - First load a structure or a parameter file as a new molecule (for example AMBER7 Parm, XPLOR PSF, GROMACS GRO, PDB, ... ).   
    - Then add a trajectory to the molecule: highlight the molecule, go to   
     `File`->`Load Data into Molecule` and navigate to the trajectory file that you want to visualize).

~~~
cd  ~/scratch/workshop/pdb/6N4O/simulation/sim_pmemd/4-production
vmd
~~~
{: .language-bash}

~~~
mol new prmtop.parm7
mol add file mdcrd_nowat.nc
~~~
{: .vmd}

- Viewing trajectories
   - Interpolating coordinates to make trajectory animation run smoother:   
`Graphical representations`->`Trajectory`->`Trajectory Smoothing Window Size`    
   - Visualizing periodic images:   
`Graphical representations`->`Periodic`  

- Saving VMD scene: `File`->`Save visualizations state`.
- Restoring a visualization state: `File`->`Load visualizations state`.

#### Measuring distances, angles, and dihedrals
Use hot keys to measure distances, angles and torsions.

| Action        | Hot keys   |||||||||||||||||||||||
----------------|---------
| Select atom      |     1
| Select bond      |     2  
| Select angle     |     3  
| Select dihedral  |     4  

{% include links.md %}
