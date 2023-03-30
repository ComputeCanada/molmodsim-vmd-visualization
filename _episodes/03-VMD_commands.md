---
title: "A first look at the VMD command-line interface"
teaching: 20
exercises: 0
questions:
- "How to start VMD in text mode?"
- "How to use VMD commands?"
objectives:
- "Learn how to use basic VMD commands"
- "Learn to write loops in VMD"
- "Learn how to get help on VMD commands"
keypoints:
- ""
---

5a31 - a good example of multichain protein complex

## Introducing VMD commands
### Starting VMD in text mode.
The VMD program can also be used in text mode. Everything you can do in VMD interactively can also be done with commands and scripts. In text mode you interact with VMD using commands. The command-line interface is more flexible than GUI and it allows VMD to read commands from script files. With commands, you can use options than are not available in GUI, and you can run VMD non-interactively. Command-line access to VMD functions is very useful in HPC environment for batch jobs. The text mode is typically used for analyzing MD simulations and rendering animations. 

How to start vmd in text mode? If graphical display is not available VMD will automatically fallback into text mode. When you are running analysis scripts on a system with graphical display you may want to enforce text mode to prevent VMD from opening GUI. To do so start vmd with the option `-dispdev text`  

### Entering commands
Commands can be entered in two ways: 
- in the `VMD command window`  
- in `Tcl console` available under the `Extensions` menu

VMD command window is very basic, you can only type commands, and it is not possible to edit command lines. Tcl console offers history, autocompletion, and syntax highlighting.

Let's run our first command.
~~~
rotate
~~~
{: .vmd}

~~~
rotate usage:
rotate stop -- stop current rotation
rotate [x | y | z] by <angle> -- rotate in one step
rotate [x | y | z] by <angle> <increment> -- smooth transition
~~~
{: .output}

If a command is entered without any argument, VMD displays short instructions on how to use it.

~~~
rotate x by 90 1
~~~
{: .vmd}

A smooth rotation of the scene around the x axis will be performed with an increment of 1 degree by this command. The advantage of using commands to rotate is that you can precisely specify the angle by which you want to rotate. For example to get a lateral view of a membrane system with membrane in x-y plane you would type `rotate x by 90` or `rotate y by 90`.

Try continuous rotation:
~~~
rock x by 1 180
rock off
~~~
{: .vmd}

Sometimes it is useful to set the exact dimensions of the graphical window. For example you want to make a movie in a standard HD format.

~~~
display resize 1080 720 
~~~
{: .vmd}

Sometimes when you start VMD graphical window may be positioned incorrectly so that its menu bar is off screen and you can't move with mouse. In this case you can reposition it with the command:

~~~
display reposition 100 700
~~~
{: .vmd}

Display position 0 0 corresponds to the lower left display corner.


{% include links.md %}
