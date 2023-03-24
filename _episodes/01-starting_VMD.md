---
title: "Using VMD on Digital Research Alliance of Canada systems"
teaching: 10
exercises: 0
questions:
- "How to use VMD on Compute Canada clusters?"
objectives:
- "Learn how to run vmd on a compute node"   
- "Learn how to connect graphically to VMD running on a compute node"
keypoints:
- "Connect to a compute node using TigerVNC client and SSH tunnel"
---

## Introduction

{: .instructor_notes} 
Molecular modelling and simulations are widely used in structural biology, chemistry, drug design, materials science and many other fields of science. Visualization is one of the most useful means for evaluating the quality of molecular models. The ability to visualize atomic details is crucial to understanding molecular functions. 

Visualizing macromolecular structures is challenging, and it requires specialized software. VMD is one of the molecular visualization packages.
There are several molecular visualization software alternatives, including [UCSF Chimera](https://www.cgl.ucsf.edu/chimera/), [PyMol](https://pymol.org/2/), and [JMol](https://jmol.sourceforge.net/demo/)

VMD (Visual Molecular Dynamics) is a software package for the 3D visualization, modeling and analysis of molecular systems. It is developed and freely distributed by the Theoretical and Computational Biophysics Group at the University of Illinois at Urbana-Champaign.
{: .instructor_notes} 

{: .self_study_text} 
- VMD (Visual Molecular Dynamics) is a software package for the 3D visualization, modeling and analysis of molecular systems.
{: .self_study_text} 

### VMD features
- Ability to work very efficiently with huge MD trajectories.
- Wide variety of supported file formats
- Numerous built-in tools for analysis of structures and trajectories
- Powerful scripting capability
- Publication quality photorealistic ray-tracing rendering
- Ability to make custom trajectory movies

## Connecting to a remote computer running VMD
To use VMD GUI on Alliance clusters interactively you need to establish graphical connection. Currently there are two options: remote desktop with VNC or JupyterHub.

### Connecting graphically to a cluster with JupyterHub 
JupyterHub provides remote desktop via noVNC (the open source VNC client). JupyterHub runs in any browser. It is convenient to use as it allocates resources and launches remote desktop in one step without requiring any additional software.

Here is the list of [JupyterHubs on clusters](https://docs.computecanada.ca/wiki/JupyterHub#JupyterHub_on_clusters).

Steps to connect to a Jupyter Hub:
1. Login with your CC credentials
2. Request resources and spin up a Jupyter server
3. Choose **'Desktop'** in JupyterLab Launcher

The drawbacks:
- If cluster usage is high the Jupyter server may fail to start due to a timeout.

### Connecting graphically to a cluster using TigerVNC
#### Connecting to a visualization node on Graham
Graham has dedicated visualization nodes **gra-vdi.computecanada.ca**. You need to install TigerVNC Viewer to use them (RealVNC or any other client will not work). 

To start using a dedicated visualization node simply connect TigerVNC viewer to gra-vdi.computecanada.ca. 

Advantages:
- Direct connection from your laptop with TigerVNC Viewer.
- Can use GPU    

Drawbacks:
- Available only on Graham
- Visualization node is shared between all logged in users, may be lagging depending on the workload.

No modules are loaded on VDI nodes by default. Before you can use central modules you need to load CcEnv and StdEnv modules:

~~~
module load CcEnv StdEnv/2020 cuda vmd
~~~
{: .language-bash}


If graphical window is off screen, you can reposition and resize it. The following commands should work for most of you:

~~~
display reposition 400 400
display resize 600 600
~~~
{: .vmd}

You can save your settings in the initialization file. I'll show you how to do it later.

#### Connecting to a compute node. 
We will use Beluga as an example to illustrate connecting VNC to a compute node.

##### 1. Connect to Beluga with SSH. 
~~~
ssh user@beluga.computecanada.ca
~~~
{: .language-bash}

##### 2. Allocate some resources:  
~~~
salloc -c2 --mem-per-cpu=1000 --time=3:0:0
~~~
{: .language-bash}
~~~
...
salloc: Granted job allocation 35464975
salloc: Waiting for resource configuration
salloc: Nodes bg11308 are ready for job
[user@bg11308 ~]$ 
~~~
{: .output}

##### 3. Start VNC server
~~~
vncserver
~~~
{: .language-bash}

Enter your new VNC password when prompted. Answer 'no' on the question about view-only password.  

Server will display a line like this in its output after it has started:

~~~
New 'bg11308.int.ets1.calculquebec.ca:1 (user)' desktop is bg11308.int.ets1.calculquebec.ca:1
~~~
{: .output}

Here <mark>bg11308</mark> is the hostname of the node where the server is running and <mark>:1</mark> is the number of VNC session.  
TigerVNC sessions are listening on port 5900 plus the session number, so in this example port number is 5900 + 1 = 5901. You will need host name and port number to connect your computer to the remote VNC session. 

##### 4. Open SSH tunnel to the remote computer
{: .instructor_notes} 
Now you need to connect your local computer to the node where the VNC server is listening. In order to access compute nodes, you must go through a login node as they are located on an internal network.

We can use SSH client program to connect port 5901 of bg11308 directly to our local computer. This type of connection is called "SSH tunneling" or "SSH port forwarding". 
{: .instructor_notes} 

The following SSH command creates a tunnel between port 5901 on bg11308 and port 5901 on your local computer.
~~~
ssh user@beluga.computecanada.ca -L 5901:bg11308:5901
~~~
{: .language-bash}

The format of the host:port specification is `local_port:remote_host:remote_port`. You can use any free local port.

The tunnel is active only while the session is running. Do not close this window and do not logout, this will close the tunnel and disconnect your laptop. 

##### 5. Connect the local computer to the remote computer
Start VNC viewer on your local computer and connect it to `localhost:5901`.

##### 6. When you are done close VNC session on the remote:
~~~
vncserver -kill :1
~~~
{: .language-bash}

If you don't terminate VNC sessions old .log and .pid files will be accumulating in the directory ~/.vnc, but don't worry, it is easy to clean them up:
~~~
rm ~/.vnc/*.log
rm ~/.vnc/*.pid
~~~
{: .language-bash}

>## Challenge
> What ssh command should user21 use to connect port 5999 of his laptop to VNC session :5 running on node2 of the cluster moledyn.ace-net.training?  
>Answers:
>1.  ssh user21@moledyn.ace-net.training -L 5999:localhost:5905
>2.  ssh user21@moledyn.ace-net.training -L 5905:localhost:5999
>3.  ssh user21@moledyn.ace-net.training -L 5999:node2:5905
>4.  ssh user21@node2.ace-net.training -L 5999:localhost:5905
>
>> ## Solution
>> ~~~
>> ssh user21@moledyn.ace-net.training -L 5999:node2:5905
>> ~~~
>> {: .language-bash}
> {: .solution}
{: .challenge}

{% include links.md %}
