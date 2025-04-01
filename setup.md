---
title: Setup
---
   
#### 1. Download [data](https://github.com/ComputeCanada/molmodsim-amber-md-lesson/releases/download/workshop-2021-04/workshop_vmd_2024.tar.gz)

#### 2.Training cluster [signup sheet](https://docs.google.com/spreadsheets/d/1OpeT4APsJgxe4iPu3snuHaAXR9cpRzXzBFgPudQZ03s/edit?usp=sharing)

#### 3. Software

For this session no software is necessary as we will use an online programming environment called JupyterHub. However, VMD is a useful program to have on your laptop. 

VMD [Version 1.9.4 LATEST ALPHA](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD)  
- __LINUX_64 (RHEL 7+) OpenGL, CUDA, OptiX RTX, OSPRay, RTX RTRT__ is recommended for Linux users with Nvidia GPUs because it provides real-time ray tracing display mode.

---
#### Optional software  
There are several ways to access remote servers graphically, and we will discuss them during the session. The following software will be useful if you decide to try some of these methods.


- [MobaXterm](https://mobaxterm.mobatek.net/)
- [TigerVNC viewer](https://sourceforge.net/projects/tigervnc/files/stable/1.15.0)
    - WINDOWS (vncviewer64-1.15.0.exe)
    - MAC (TigerVNC-1.15.0.dmg)
    - LINUX   
        - Ubuntu: sudo apt update && sudo apt install vncviewer
        - CentOS: sudo yum install tigervnc  

If you don't already have a source code editor, install one. VSCode, Atom, and Notepad++ are good choices.
For creating high qualily animations install FFMPEG. Here are the links to the installation instructions for [Windows](https://phoenixnap.com/kb/ffmpeg-windows), [Linux](https://www.tecmint.com/install-ffmpeg-in-linux/), [MacOS](https://phoenixnap.com/kb/ffmpeg-mac)

{% include links.md %}
