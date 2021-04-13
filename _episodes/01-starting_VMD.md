---
title: "Using VMD on Compute Canada Systems"
teaching: 10
exercises: 0
questions:
- "How to use VMD on Compute Canada clusters?"
objectives:
- "Learn how to run vmd on a compute node"   
- "Learn how to connect grahically to VMD running on a compute node"
keypoints:
- "Connect to a compute node using TigerVNC client and SSH tunnel"
---

## Introduction
Molecular modelling and simulations are widely used in structural biology, chemistry, drug design, materials science and many other fields of science. Visualization is a primary tool for accessing quality of molecular models, and visualization in invaluable for understanding atomic details of molecular functions. Visualization of complex macromolecular structures is challenging and requires specialized software.

VMD (Visual Molecular Dynamics) is a software package for the 3D visualization, modeling and analysis of molecular systems. It is developed and freely distributed by the Theoretical and Computational Biophysics Group at the University of Illinois at Urbana-Champaign.

#### VMD features
- Ability to work very efficiently with huge MD trajectories.
- Wide variety of supported file formats
- Numerous built-in tools for analysis of structures and trajectories
- Powerful scripting capability
- Publication quality photorealistic ray-tracing rendering
- Ability to make custom trajectory movies

### Using VMD on Compute Canada systems
To use VMD on CC systems you need to establish graphical connection. Currently there are two options: remote desktop with VNC or JupyterHub.

#### JupyterHub 
JupyterHub provides remote desktop via noVNC (the open source VNC client). It is convenient to use because it just runs in a browser without need to setup ssh tunnel. 

The drawbacks:
- It is currently available only on Beluga.
- You can't control on which of the compute nodes the remote desktop will be launched 

JupyterHub Servers:
- [Beluga](https://jupyterhub.beluga.calculcanada.ca/hub/login)

1. Login with your CC credentials
2. Spin up a server
3. Choose **'Desktop'** in JupyterLab Launcher

#### Visualization nodes on Graham
Graham has dedicated visualization nodes **gra-vdi.computecanada.ca**. To start using them you need to install TigerVNC Viewer. RealVNC or any other client will not work.  

Advantages:
- Direct connection from your laptop with TigerVNC Viewer.
- Can use GPU    

Drawbacks:
- Available only on Graham
- Visualization node is shared between all logged in users, may be lagging depending on the workload.

### Connecting to the training cluster:

[Jupyter Hub](http://jupyter.moledyn.ace-net.training)   

SSH
~~~
ssh user01@moledyn.ace-net.training
~~~
{: .bash}

Guest usernames: user[01-45]  
mistakenly.presently.innocent.bass


#### Connecting graphically to a compute node. 
Connect to the training cluster with SSH.  
~~~
ssh user01@moledyn.ace-net.training
~~~
{: .bash}

Allocate some resources:
~~~
salloc -c1 --mem-per-cpu=1000 --time=3:0:0
~~~
{: .bash}
~~~
salloc: Granted job allocation 2
salloc: Waiting for resource configuration
salloc: Nodes node1 are ready for job
[user01@node1 ~]$ 
~~~
{: .output}

Start VNC server
~~~
vncserver
~~~
{: .bash}
Enter your new VNC password when prompted. 
~~~
You will require a password to access your desktops.

Password:
Verify:
Would you like to enter a view-only password (y/n)? n
xauth:  file /home/user01/.Xauthority does not exist

New 'node1.int.novatest.ace-net.training:1 (user01)' desktop is node1.int.novatest.ace-net.training:1

Creating default startup script /home/user01/.vnc/xstartup
Creating default config /home/user01/.vnc/config
Starting applications specified in /home/user01/.vnc/xstartup
Log file is /home/user01/.vnc/node1.int.novatest.ace-net.training:1.log
~~~
{: .output}

Note the number of the .log file. The number of the log file matches the number of VNC session. You need to know it to make connection. The first session is listening at port  5901, the second at 5902 .. etc.

Ensure that the server is running:
~~~
vncserver -list
~~~
{: .bash}
~~~
TigerVNC server sessions:

X DISPLAY #	PROCESS ID
:1		13259
~~~
{: .output}

Do not close this window.

Now you need to connect your local computer to the node where the VNC server is listening. Compute nodes cannot be accessed directly from the Internet, but you can connect to the login node, and the login node can connect to any compute node. Thus, connection to a compute node should be also possible, right? How do we connect to the node1 at port 5901? We can instruct ssh client program to map port 5901 of node1 to our local computer. This type of connection is called "SSH tunneling" or "ssh port forwarding". SSH tunneling allows transporting networking data between computers over an encrypted SSH connection.

Open a new terminal tab or window and run the command:
~~~
ssh user01nova.ace-net.training -L 5901:node1:5901
~~~
{: .bash}

Replace the port number and the node name with the appropriate values.

This SSH session created tunnel from port 5901 on your local computer to  port 5901 on node1. The tunnel will be active only while the session is running. Do not close this window and do not logout, this will close the tunnel and disconnect your laptop from node1. Once connection is established start VNC viewer on your local computer and connect to localhost:5901.

When you are done close VNC session on the remote:
~~~
vncserver -kill :1
~~~
{: .bash}

>## Challenge
> How to modfy SSH command to open tunnel to the login node?
>> ## Solution
>> ~~~
>> ssh user01nova.ace-net.training -L 5901:localhost:5901
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}

### Starting VMD
Open a terminal: 
`Applications` --> `System Tools` --> `Mate Terminal`
~~~
module load StdEnv/2020 gcc apbs vmd
vmd
~~~
{: .bash}

Three windows will open: `VMD OpenGL Display`, `VMD Main`, and `VMD command window`. Do not close any of them.

{% include links.md %}

