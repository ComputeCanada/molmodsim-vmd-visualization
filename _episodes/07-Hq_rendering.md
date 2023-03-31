---
title: "Photorealistic Rendering and Making Movies"
teaching: 20
exercises: 0
questions:
- "What is Photorealistic Rendering in VMD, and how can I use it?"
objectives:
- "Learn how to make realistically looking figures "
keypoints:
- ""
---

### How to make 3D objects look more realistic?
VMD is designed to generate images very fast to maximize interactivity, so in rendering in graphical window is optimized for speed. As you may have noticed by now rendered images do not look realistic, they lack 3D feel, they look flat and it is impossible to see surface profile clearly. For example, if you look at the real model of a protein with a cavity you'll see that the cavity is darker than the exposed outer surface, and becomes darker the deeper inside the cavity one goes.
 
To simulate such effects one needs to use ray-tracing technique called `Ambient Occlusion`. The ambient occlusion technique simulates the soft shadows that should naturally occur when indirect or ambient lighting is cast out onto your scene to make 3D objects look more realistic. Another technique helping to simulate a photorealistic image is `Depth of Field` focal blur.

These features are not enabled by default. Let's enable ambient occlusion and depth of field focal blur:

- `Display settings` --> `Ray-Tracing Options` and enable `Ambient Occlusion`, `Shading`, `DoF` 
- You also need to change material to `Diffuse` or one of the AO-optimized materials (`AoChalky`, `AoShiny`, or `AoEdgy`). These materials are optimal for rendering with ambient occlusion model. 
- Finally, use Tachyon a ray-tracing engine that is capable of handling ambient occlusion. 

|-----------------------------|-------------------------|---------------------------------------|
|:---------------------------:|:-----------------------:|:-------------------------------------:|
| ![](../fig/snapshot.png) Snapshot  | ![](../fig/ambient_occl.png) Tachyon, AoChalky | ![](../fig/ambient_occl_edgy.png) Tachyon, AoEdgy  |

### Tachyon flavours
VMD provides several implementations of Tachyon:
- Standalone Tachyon, supports rendering with both CPUs and GPUs.  
- Internal Tachyon using Intel OSPRay ray-tracing engine (CPU, AVX-accelerated). Best for Intel laptops with integrated graphics.
- Internal Tachyon using NVidia OptiX ray-tracing engine. For computers with NVidia GPUs.
- Interactive Tachyon OptiX allows to setup scene interactively before rendering the final image.
- Tachyon RTX Real Time Ray Tracing.  Best for computers with NVidia RTX GPUs (works also on NVidia GPUs without RTX cores).

#### Tachyon OptiX and Tachyon OSPRay interactive ray tracers
Nvidia OptiX and Intel OSPRay are hardware-accelerated ray tracers capable of interactively rendering photorealistic global illumination. On Alliance systems Tachyon OptiX is available by loading `cuda` and `vmd/1.9.4a43` modules. The interactive ray tracer opens a new graphical window in which you can preview ray-traced rendering and interact with it using a mouse. When you close the interactive window the final image is saved in file. It allows only for a limited interactive functionality. You can rotate, scale, translate with a mouse, and you can turn on/off AO and DoF. It not a full-featured interactive implementation, but it is useful for improving the final rendered image.

Compare normal, glsl and OptiX rendering modes.

#### Real Time Ray Tracing (RTX-RTRT)
Real Time Ray Tracing solves limitations of the Tachyon OptiX interactive ray tracer by providing full-time ray-tracing in the main OpenGL VMD window. Real Time Ray Tracing using NVidia RTX cores is supported in version 1.9.4a55 of VMD on Linux platform.  

Installation:
~~~
# Configure where to install vmd
export VMDHOME=$HOME/scratch/VMD
# There is no need to change anything below this line 
export VMDINSTALLBINDIR=$VMDHOME/bin 
export VMDINSTALLLIBRARYDIR=$VMDHOME/lib 
wget https://www.ks.uiuc.edu/Research/vmd/vmd-1.9.4/files/alpha/vmd-1.9.4a55.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185-RTXRTRT.opengl.tar.gz
tar -xf vmd-1.9.4a55.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185-RTXRTRT.opengl.tar.gz
cd vmd-1.9.4a55 && ./configure 
cd src && make install
~~~
{: .language-bash}

Full-time ray tracing is available as a special rendering mode: `rendermode Tachyon RTX RTRT`

Pros:
- Enables quick creation of photorealistic animations.
- Greatly simplifies creation of impressive images allowing for an instant feedback. 

Cons:
- Not all representations are available.
- Coloring by volume is not available

#### Tachyon (internal) and standalone
Despite the widespread use of hardware-accelerated Tachyon implementations today, the legacy CPU-only Tachyon implementation is preferred in some cases. Tachyon implementations using OSPay and OptiX libraries can not render representations such as lines, points, and volume slices because they are not available in standard OpenGL libraries.

#### Tachyon standalone 
The standalone version of Tachyon provides greater control over ray-tracing than the internal version. For example:
- You can improve the visual appeal of transparent surfaces by controlling the number of surfaces, for example (shown in Figure below). 
- The standalone Tachyon can also be used to render multiple trajectory frames in parallel to accelerate production of high-quality and high-definition animations.

|:-------------------------:|:-------------------------:|-------------------------:|
| ![](../fig/trans_snapshot.png){:width="400"} Snapshot  | ![](../fig/trans_max_1.png){:width="400"} Tachyon, BlownGlass, <br>\-\-trans_max_surfaces 1| |

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


### Making movies
#### Movie maker
The `Movie Maker` extension offers several types of animations. You can make a movie of rotation of rocking a static structure, or animate a trajectory with an optional viewpoint rocking. The default compression algorithm is also a very basic quality mpeg-2 encoder optimized for speed on a single computer.  

FFmpeg is a powerful tool that can be used to encode videos with high quality codecs. If you haven’t already installed FFmpeg, you can download and install it from [here](https://www.ffmpeg.org/download.html). FFmpeg is already installed on clusters, so you only need to load it.

~~~
module load vmd ffmpeg
vmd
~~~
{: .language-bash}

Standard options in movie maker are fairly limited. It will simply rotate a molecule or loop over all trajectory frames with a chosen step. If you want to make something more interesting such as zooming at the molecule and moving camera around it then you have to write a script.

- `Extensions` -> `Visualization` -> `Movie Maker`
- `Movie settings` -> `Rotation about Y axis`
- `Format` -> `MPEG2(ffmpeg)`
- Optionally `Set working directory` 
- In the `Movie durations (seconds)` box enter 10
- Press `Make movie`

For a trajectory movie duration is defined by the number of frames and trajectory step size. So for 3140 frames with stepsize 2 durarion is 3140/(24*2)=65 sec.

### Making movies with Tcl scripts
With a custom animation script you have full control of camera movements and special effects such as adding glow lights to some atoms, drawing geometrical figures, slicing volume data, etc. 

Much better image rendering can be done in a reasonable time on an HPC cluster. Typically you would use VMD to write scene description files of every trajectory frame for subsequent rendering with a ray tracing engine such as Tachyon. Once input files are ready you submit a script for rendering multiple frames in parallel on hundreds of CPU's. Then you encode all frames in a video with ffmpeg. Much better compression algorithms such as H.265/HEVC or Google VP9 with much higher quality settings can be used to encode an animation with `ffmpeg`.

Exercise:
Create a movie showing the diffusion of several Na+ ions. It is good to choose residues 966, 1136, 904, 903 because they display association-dissociation dynamics.

~~~
proc makemovie { start end } {
for { set i $start } { $i < $end } { incr i } {
   animate goto $i 
   display update
   puts "Rendering frame $i"
   render snapshot tmp/$i.ppm
   }
}
~~~
{: .vmd}

Make sure the directory tmp exists in the working directory!
Try adding rotation and scaling in the for loop:

~~~
rotate x by 0.5
scale by 0.995
~~~
{: .vmd}


It is better to eliminate translational/rotational motions before making an animation. Add code aligning each frame to the reference. 

#### Encoding movies with ffmpeg
- MPEG-2 I-frame only Highest Quality Encoding
~~~
ffmpeg -start_number -i %d.ppm -vcodec mpeg2video -pix_fmt yuv420p -q:v 1 -an  movie.m2v
~~~
{: .language-bash}

- H.264 I-frame only Highest Quality Encoding
~~~
ffmpeg -start_number -i %d.ppm -vcodec libx264 -pix_fmt yuv420p -crf 18 -s 1080x720 -preset veryslow movie.mp4
~~~
{: .language-bash}


{% include links.md %}