---
title: "Loading Structure Files and Interacting with Molecules"
teaching: 10
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

### Opening PDB files
As an example will be using X-ray crystallographic structure of human hemoglobin 1SI4.

####  Downloading files from Protein Data Bank
VMD can directly query PDB database.
![](../fig/pdb_tool.png)

Enter "1si4" and click `Load into new molecule in VMD`.  

![](../fig/hemoglobin-1.png){:width="400"}

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

![](../fig/hemoglobin-2.png){:width="400"}

- Create a representation
- Double click to turn representations on/off without deleting them
- Changing draw styles 
- Most useful draw styles: `NewCartoon`, `QuickSurf`, `Surf`, `Licorice`, `VDW`, `HBonds`
- Selecting atoms, lots of selection options are available. 
- Most often used selection keywords are `noh`, `backbone`, `protein`, `nucleic`, `resname`, `resid`, `index`
- Selecting a a residue: `residue` (starts from 0), `resid` (starts from 1)
- Selecting an atom: `index` (starts from 0), `serial` (starts from 1)

![](../fig/hemoglobin-3.png){:width="400"}

>## Challenge
>Reproduce the following figure
>
>![](../fig/hemoglobin.png){: width="480"}
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
{:.bash}
~~~
mol new prmtop.parm7
mol add file mdcrd_nowat.nc
~~~
{:.vmd}

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

### Some commonly used VMD extensions
#### Sequence Viewer   
`Extensions`->`Analysis`->`Sequence viewer`   
Very useful to select a residue or a group of residues in a sequence and quickly visualize them.

#### RMSD Calculator    
`Extensions`->`Analysis`->`RMSD Calculator`   

>## Aligning Structures and calculating RMSD 
>1. Compute RMSD of two molecules: PDB ID 1si4 and 4n7n. Use only chain A backbone atoms for the calculation. 
>2. Why alignment fails when backbone of all chain A atoms is used for alignment? 
>3. Can you think of a way to include all common backbone atoms in the alignment? 
>
>>## Solution
>> 1. Use the atom selection: `chain A and resid 1 to 140`, and check box `Backbone only` for both alignment and RMSD calculation, RMSD = 0.92558
>> 2. The residue 141 of 1si4 molecule has the terminal oxygen atom "OXT", while it is absent in 4n7n.
>> 3. Exclude the OXT atom from the selection: `not name OXT and chain A and resid 1 to 141` 
>>
>{: .solution}
{: .challenge}


#### Movie Maker

The `Movie Maker` extension offers several types of animations. You can make a movie of rotation of rocking a static structure, or animate a trajectory with an optional viewpoint rocking. The available compression algorithm is also a very basic quality mpeg-2 encoder optimized for speed on a single computer.  

With a custom animation script you have full control of camera movements and special effects such as adding glow lights to some atoms, drawing geometrical figures, dynamic slicing volume data, etc. 

Much better image rendering can be done in a reasonable time on an HPC cluster. Typically you would use VMD to write scene description files of every trajectory frame for subsequent rendering with a ray tracing engine such as Tachyon.Then you would submit a script for rendering multiple frames in parallel on hundreds of CPU's. Then you would encode all frames in a movie with ffmpeg. Much better compression algorithms such as H.265/HEVC or Google VP9 with much higher quality settings can be used to encode an animation with `ffmpeg`.

~~~
module load StdEnv/2020 gcc vmd ffmpeg
vmd
~~~
{:.bash}
- `Extensions` -> `Visualization` -> `Movie Maker`
- `Movie settings` -> `Rotation about Y axis`
- `Format` -> `MPEG2(ffmpeg)`
- Optionally `Set working directory` 
- In the `Movie durations (seconds)` box enter 10
- Press `Make movie`


#### Electrostatic potential calculations

VMD can compute electrostatic potential using APBS and We should be OK for this session, there is some theory left and most of it will be VMD and introducing  PDB files
`Extensions`-> `Analysis`-> `APBS electrostatics`
~~~
module load StdEnv/2020 gcc vmd apbs amber
cd ~/home/svassili/scratch/workshop/pdb/6N4O/simulation/setup
cpptraj prmtop.parm7
~~~
{:.bash}

~~~
trajin inpcrd.rst7
strip !(:860-898)
trajout nucleic.pqr pdb dumpq
go
~~~
{: .leap}

VMD
1. Load .pqr file
2. Reperesentation QuickSurf [0.5 0.5 0.5, max quality]
3. `Extensions` -> `Analysis` -> `APBS electrostatics` ->  `Run APBS`
4. When prompted `Load ABBS into top molecule`
5. Select coloring method `Volume`
6. Adjust `Color scale data range` in the `Trajectory` tab. Try [-50 50]

#### Higher quality rendering
VMD is designed to generate images very fast to maximize interactivity, so in interactive mode rendering is optimized for speed and VMD does not use computationally intensive  photorealistic rendering techniques. If you want a very high definition picture, or a picture with photorealistic rendering of shadows or transparent surfaces you need to use a ray tracing engine. Instead of producing high quality images directly, VMD writes scene description files which can be used as input to several popular scanline rendering and ray tracing programsVMD ships with TACHYON for producing high quality images.

 While VMD generally produces nice looking images in its graphics window, it was designed to generate its images very rapidly to maximize interactivity, which precludes the use of photorealistic rendering techniques that would slow down the operation of whole program. Instead of producing high quality images directly, VMD writes scene description files which can be used as input to several popular scanline rendering and ray tracing programs

Compiling Tachyon
~~~
cd ~
wget http://jedi.ks.uiuc.edu/~johns/raytracer/files/0.99b6/tachyon-0.99b6.tar.gz
tar -xf tachyon-0.99b6.tar.gz
cd tachyon/unix
make linux-64-thr
ln -s ~/tachyon/compile/linux-64-thr/tachyon ~/bin/tachyon
~~~
{: .bash}

#### References
[VMD Introductory tutorial](https://doi.org/10.1002/0471250953.bi0507s24)

{% include links.md %}
