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
### Starting VMD
Open a terminal: 
`Applications` --> `System Tools` --> `Mate Terminal`
~~~
module load cuda vmd
vmd
~~~
{: .language-bash}

Three windows will open: `VMD OpenGL Display`, `VMD Main`, and `VMD command window`. Do not close any of them.

### Working with PDB files
As an example will be using X-ray crystallographic structure of human hemoglobin 1SI4.

####  Downloading files from Protein Data Bank
VMD can directly query PDB database.
![VMD-Main > Extensions > Data > PDB Database Query]({{ page.root }}/fig/pdb_tool.png){:width="400"}

Enter "1si4" and click `Load into new molecule in VMD`.  

![Hemoglobin rendered as sticks]({{ page.root }}/fig/hemoglobin-1.png){:width="400"}

If you started VMD on a compute node download function will not work because compute nodes are not allowed to connect to the Internet. 

#### Opening a PDB file saved in your computer.
You can download the example PDB file from a login node:
~~~
wget https://files.rcsb.org/download/1si4.pdb
~~~
{: .language-bash}

On the `VMD Main` window menu select `File` -> `New Molecule`. This will open the `Molecule File Browser`. Choose a molecule PDB file.

### VMD command-line interface.
In text mode you interact with VMD using commands. The command-line interface is more flexible than GUI. With commands, you can use options than are not available in GUI. Command-line access to VMD functions is very useful in HPC environment for batch processing. The text mode is typically used for analyzing MD simulations and rendering animations.

In addition to commands, VMD offers the built-in TCL programming language. TCL interpreter available from the command-line allows you to write processing and visualization scripts utilizing any existing VMD functions and variables. 

Commands can be entered in the terminal window where you typed "vmd", or in `TCL console` available in the `Extensions` tool bar. 



Let's use our first command `pdbload`. This command will download and open a PDB file:

~~~
mol pdbload 1si4
~~~
{: .vmd}

Note: loading files from PDB is not possible from compute nodes on Graham and Beluga because they are not connected to the Internet. Use the login nodes on these systems to load PDB files.

#### Getting help on text commands
You can get help with text commands in several ways: 
1. This [link](https://www.ks.uiuc.edu/Research/vmd/vmd-1.9.4/ug/node121.html) provides a summary of basic text commands in VMD.  
2. You can find a detailed description of all commands, as well as their syntax in the online [VMD manual](https://www.ks.uiuc.edu/Research/vmd/current/ug/node117.html).  
3. Printing a command without any arguments displays a short version of help.


### Customizing VMD sessions
At startup VMD load a set of default options. You may find that they are not optimal for your work, and you need to change them every time you start a new visualization. Once you have changed settings to your liking you can make them permanent. 

#### Saving current settings 
Using the following steps, you can save current settings:
`Extensions`->`VMD Preferences` -> `Write setting to VMDRC`

#### VMD parameters that are often changed from their default values:

#### `Display rendermode GLSL` 
The GLSL mode turns on OpenGL extensions implementing programmable shading and transparency for higher quality molecular graphics. This mode renders faster and produces better images than "Normal" mode.

Load 6n4o.pdb file, and create a `NewCartoon` representation. Try rotating molecule and observe rendering speed. Is reasonable in `Normal` mode. 

Add `licorice` representation. Rendering is very slow now. 

Switch to GLSL mode. Performance is better now but still slow. It will improve if more CPUs are available, but the best of course is to use a GPU.  

#### `Display Projection orthographic` 
The orthographic projection allows us to get a better sense of the distance between objects and their relative sizes. The angle of view, distance, and focal length of a camera lens make lines of identical length appear different in perspective mode. Due to this, it is difficult to determine the relative size and dimensions of distant objects in perspective mode.

#### `Axes location off` 
In most cases you don't want axes to appear in figures prepared for publication.

####  `Display depthcue off` 
The use of depth cues can enhance perception of depth and distance. This is achieved by blending distant objects into the background color. Most of the time, it is not desirable and there are better alternatives.

#### `Display ambientocclusion on` and `Mol default material Diffuse`
These ray-tracing options enable photorealistic rendering. More about this later.

#### Managing VMDRC files
VMD preferences are saved in ~/.vmdrc file. VMD creates a very long file containing hundreds of all available settings, even the ones that are using global default values. You may find useful to create a short version of .vmdrc containing only settings that you want to change. A .vmdrc file is just a text file and it can easily be edited manually with any text editor. 

Let's open it. Settings are organized in sections. You don't want to change default colors, materials or menus. So you can safely delete these sections and leave only display parameters and molecular representations.

As an example, a .vmdrc file might look like this:
~~~   
      # VMD settings: file ~/.vmdrc
      menu main on
      display reposition 100 600
      display resize 672 682
      display projection Orthographic
      display depthcue off
      display rendermode GLSL
      display ambientocclusion on
      axes location Off
      mol default material Diffuse
~~~
{: .file-content}

### Interacting with molecules
#### Obtaining good views for molecules
Rotate, zoom in/out, translate, and set rotation center to get a desired view. Pressing any of these keys switches the mode of interaction. For example, when `s` key is pressed mouse cursor will change. In this mode click, hold and move on a trackpad/mouse will zoom in/out.   
If you are lost simply press the `=` key to reset view.

| Action        | Hot keys 
----------------|---------
| Rotate        |     r      
| Zoom (Scale)  |     s                  
| Translate     |     t      
| Reset View    |     =      
| Set Center    |     c

#### Working with Graphical Representations  
##### Creating and modifying graphical representations
- Navigate to `Graphics`->`Representations`  
- Change `Drawing Method` to `New Cartoon`   
- Change `Coloring Method` to `Chain`  
- Try to get a better illumination by moving lights: `Mouse`->`Move Light`

![Hemoglobin rendered as "New Cartoon"]({{ page.root }}/fig/hemoglobin-2.png){:width="400"}

- Changing draw styles. Try draw styles `Tube`.
- Create a representation, change its style to `QuickSurf`.
- Make it transparent by changing material to `GlassBubble` 
- Double click to turn representations on/off without deleting them. 
- Most useful draw styles: `NewCartoon`, `QuickSurf`, `Surf`, `Licorice`, `VDW`, `HBonds`

![Hemoglobin rendered as backbone with transparent surface]({{ page.root }}/fig/hemoglobin-3.png){:width="400"}

##### Selecting atoms 
- Selecting atoms, lots of selection options are available. 
- Most often used selection keywords are `noh`, `backbone`, `protein`, `nucleic`, `resname`, `resid`, `index`
- Selecting a residue: `residue` (starts from 0), `resid` (starts from 1)
- Selecting an atom: `index` (starts from 0), `serial` (starts from 1)
- Selecting hetero atoms: not (protein or nucleic or resname HOH)
- Limit selection to one monomer: not (protein or nucleic or resname HOH)
- Use logical operators (and, or, not) for complex selections
- Selecting atoms based on distance: 
   - `within 3 of serial 93` - selects all atoms located within 3 Angstrom of atom #93
   - `same residue as within 3 of serial 93` - selects whole residues if any atom is within 3 Angstrom of atom #93. 
- Selecting groups of atoms or residues
   - `residue 0 to 10, 14, 15, 20 to 30`
   
#### What selections are available in the loaded molecule?
 In `Graphical representations window` go to `Selections`-->`chain`. 
 VMD will display all chains present in the file (A,B,C,D).  
 Double click on keyword and value to select them.  

>## Challenge
>Reproduce the following figure. In this image we see chain A of the pdb entry 1SI4 . The figure shows protein, residue HEM, atom FE, and HEM ligands (residues HIS, CYN). 
>
>![Hemoglobin protein with ligand]({{ page.root }}/fig/hemoglobin.png){: width="480"}
> Use the following selection keywords: `chain`, `name`, `resname`, `same residue as within`. You will also need to use logical operators `and`, `not`. 
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
>>  
>>- save your work:  `File`->`Save visualization state`.
>{: .solution}
{: .challenge}

#### Changing default colors
....

#### Saving your work
- Saving VMD scene: `File`->`Save visualization state`.  
- Restoring a visualization state: `File`->`Load visualization state`.


#### Get workshop example data
On training cluster:
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

#### Loading structure files
- More than 70 supported file types.
- File types are recognized by extension. If a file has a non-standard extension select format manually.

### Visualizing MD trajectories.
- First load a structure or a parameter file as a new molecule (for example AMBER7 Parm, XPLOR PSF, GROMACS GRO, PDB, ... ).   
- Then add a trajectory to the molecule: highlight the molecule, go to `File`->`Load Data into Molecule` and navigate to the trajectory file that you want to visualize).

#### Load topology and trajectory files
~~~
cd ~/scratch/workshop/pdb/6N4O/simulation/sim_pmemd/4-production
ml vmd
vmd
~~~
{: .language-bash}

~~~
mol new prmtop_nowat.parm7
mol addfile mdcrd_nowat.nc
~~~
{: .vmd}

#### Viewing AMBER NETCDF-formatted trajectories on Windows and Mac.
- NETCDF trajectory files, which are AMBER's default format, can only be loaded on Linux. You can convert them to GROMACS XTC format by using the CPPTRAJ program from AMBER.

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
- Interpolating coordinates to make trajectory animation run smoother:   
`Graphical representations`->`Trajectory`->`Trajectory Smoothing Window Size`    
- Visualizing periodic images:   
`Graphical representations`->`Periodic`  


#### Measuring distances, angles, and dihedrals
Use hot keys to measure distances, angles and torsions.

| Action        | Hot keys   
----------------|---------
| Select atom      |     1
| Select bond      |     2  
| Select angle     |     3  
| Select dihedral  |     4  

#### How to delete or hide Labels?
Labels can quickly clutter display. How to delete or hide Labels when you don't need them anymore?  
`Graphics` --> `Labels` --> `Delete`

#### Photorealistic Rendering
As you may have noticed by now rendered images do not look realistic, they lack 3D feel, they look flat and it is impossible to see surface details clearly.

For example, if you look at the real model of a protein with a cavity you'll see that the cavity is darker than the exposed outer surface, and becomes darker the deeper inside the cavity one goes.

To simulate such effects one needs to use ray-tracing technique called Ambient Occlusion. The ambient occlusion technique simulates the soft shadows that should naturally occur when indirect or ambient lighting is cast out onto your scene to make 3D objects look more realistic. 

Another technique helping to simulate a photorealistic image is Depth of Field focal blur.

Let's enable ambient occlusion and depth of field focal blur:

- `Display settings` --> `Ray-Tracing Options` and enable `Ambient Occlusion`, `Shading`, `DoF` 
- Change material to Diffuse or an AO-optimized material. 
- Use Tachyon, an AO-capable ray tracer.

VMD provides several implementations of Tachyon:
- Standalone Tachyon, supports rendering with both CPUs and GPUs.  
- Internal Tachyon using Intel OSPRay ray-tracing engine (CPU, AVX-accelerated). Best for Intel laptops with integrated graphics.
- Internal Tachyon using NVidia OptiX ray-tracing engine. Best for computers with NVidia GPUs.
- Interactive Tachyon OptiX allows to setup scene interactively before rendering the final image.

#### Tachyon OptiX interactive ray tracer
- On Alliance systems available by loading `cuda` and `vmd/1.9.4a43` modules.

Compare normal, glsl and OptiX rendering modes.

#### Real Time Ray Tracing
Real Time Ray Tracing using NVidia RTX cores is supported in version 1.9.4a55 of VMD. 

Installation:

~~~
# Configure where to install vmd
export VMDINSTALLBINDIR=$HOME/scratch/VMD 
# There is no need to change anything below this line 
export VMDINSTALLLIBRARYDIR=$VMDINSTALLBINDIR/lib 
wget https://www.ks.uiuc.edu/Research/vmd/vmd-1.9.4/files/alpha/vmd-1.9.4a55.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185-RTXRTRT.opengl.tar.gz
tar -xf vmd-1.9.4a55.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185-RTXRTRT.opengl.tar.gz
cd vmd-1.9.4a55 && ./configure 
cd src && make install
~~~
{: .language-bash}

Full-time ray tracing is available as a special rendering mode: `rendermode Tachyon RTX RTRT`

- Enables quick creation of photorealistic animations.
- Greatly simplifies creation of impressive images allowing for an instant feedback. 

#### Tachyon standalone (legacy)  
The standalone version of Tachyon provides greater control over ray-tracing than the built-in version. 
- You can improve the visual appeal of transparent surfaces by controlling the number of surfaces, for example. 
- The standalone Tachyon may also be used to render multiple trajectory frames in parallel to accelerate production of high-quality and high-definition animations.

Standalone Tachyon can be installed using the following commands:
~~~
export INSTALLDIR=$HOME/bin
mkdir -p $INSTALLDIR
git clone https://github.com/thesketh/Tachyon
cd Tachyon/unix/
make linux-64-thr && ln -s ../compile/linux-64-thr/tachyon $INSTALLDIR
~~~
{: .language-bash}

- Check out [Publication Figure Rendering With Tachyon](https://www.ks.uiuc.edu/Research/vmd/minitutorials/tachyonao/) for more details.


### Visualizing Volumetric Data.
VMD has the ability to compute and display volumetric data. Volumetric data is data that is a function of position, e.g. density, potential, solvent accessibility, stored as a 3-D grid of values. VMD can display volumetric data in a few different ways. 


Load file:
workshop/pdb/6N4O/RNA_models/modeRNA/chain_D_model_B.pdb

Compute density map:
`Extensions`-->`Analysis`-->`Volmap Tool`
Change:
- selection all
- resolution 0.5

Press `Create Map`

Our molecule now has one volumetric data set associated with it, and isosurface representation of this map is created.

- select `VolumeSlice` in `Drawing Method`
- select `Volume` in `Coloring Method`
- select `Slice angle`-->`Y`

The color scale can be set in `Graphics`-->`Colors`--> `Color Scale`. Select `Sequential`-->`Plasma` in `Method`.

Play with the `Slice Offset` slider, which adjusts the y-coordinate of the slice. Different colors represent different numerical values of the volumetric data, in this case mass density. 

Make slices for the x- and z-directions as well, and rotate you view with the mouse so that all three planes are visible. 

For 3D way of representing the volumetric data create a new representation using the `Isosurface` drawing method. In the Draw menu, select Solid Surface. Now, you can see a surface of constant volumetric value, chosen using the Isovalue slider. As you choose higher values, you see the surface shrinks down around a core where the most average mass was located. Fig. 7(b) shows such an isosurface, represented by both a wire mesh and a transparent surface.

Select isovalue 0.1, make it `Glass bubble`. Then create another Isosurface with isovalue 2.2 and make it different colour.

Another common way to represent volumetric data is by coloring other representations based on it. For example you can color molecular surface by electrostatic potential.

I used pdb2pqr server to prepare pdb file with charges and radii needed for calculation of electrostatic potential.
https://server.poissonboltzmann.org


{% include links.md %}
