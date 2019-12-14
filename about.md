---
title: "About"
date: 2019-11-08T21:45:01+02:00
draft: false
---

![Example image](/payton_gui_example.png)
## What is Payton?

Payton is a general-purpose 3D programming toolkit. It is designed with the
theory that we, humans, we understand better by seeing things.

* Payton is a prototyping tool. Kickstart any idea fast and easy, grow it.
* Create tools for the next step. Create map editors, small animations, small
  algorithms or artificial intelligence for your game. Whenever you need to
  try a new idea, don't bother to create a new application with all the
  details. Payton comes with all the necessary defaults and that is what makes
  it unique. Almost everything has a pre-set.
* Game engines and other libraries are way too complex and it takes a long time
  to start the initial playground.
* Payton never intends to take place as a game engine or a full-featured 3D
  environment. There is already plenty of stuff for that purpose.
* Easy to visualize what you want to achieve or do what you want to do.
* You can move forward from Payton to any other place if you like.


We draw 2D graphs and charts in reports and we generally understand much more
easily when we visualize the data. But in some cases, visualizing exceeds 2
dimensions. We require to have third and even fourth dimensions. (And on top of
those, the definition of fourth dimension as time can get foggy in terms of
relativity.)

Payton gives you the ability to extend your graphics into 4 dimensions. It is not
software but a software development toolkit/library built with Python.
This will give users the ability to read real-time data from sensors, cameras or
any other data sources in realtime and visualise them in real time. The data source
can be a thermometer, a random number generator, a toy car connected to a speed
sensor, a map, a vehicle port or anything that generates time-based 3D data.
Furthermore, it can be a time based formula. As this can get too complex,
software with that complexity will probably be too hard to use and understand
where Payton is designed to be as simple as it can be. So easy to program that
a newbie can kick-start it just by following the tutorials.

## Features:

* 3D Math Library
* Various base geometries:
  * Cube
  * Cylinder
  * Triangular Mesh
  * Plane
  * Lines
  * Point Cloud
  * Sphere
  * Dynamic Grid
* Clean default scene
* Pre-defined keyboard-mouse and camera controls
* Pre-defined environment
* Clock system for parallel tasks and time based operations
* Simple collision detection
* Extendable controllers
* Pre-defined lighting **with shadows**
* Material support
* Clickable objects and virtual planes
* Shader support
* 3D File formats:
  * Wavefront OBJ
  * Quake 2 MD2 with Animations
* Mesh Generation Tools
  * Extrude Line in 3D
  * Rotate Line around an axis in 3D
  * Fill between lines
* Mesh modifiers:
  * Merge Mesh
  * Subdivide Mesh
* GUI - Hud Elements
  * On screen text
  * On screen surfaces & materials
  * Window
    * Panels
    * Buttons
* Extensive examples for every feature and more.


## Examples:

Examples can be found at [https://github.com/sinanislekdemir/payton/tree/master/examples](https://github.com/sinanislekdemir/payton/tree/master/examples)

More information can be found in documents.

## Some Limitations:

- Currently, only the initial light source (`scene.lights[0]`) cast shadows. This is primarily for performance reasons and hardcoded.
- There can be up to 100 lights in the scene.
- Even though there is no restriction on the number of objects in the scene, it can affect the initial load time. Once it gets loaded, it should work fine as the graphics card and shader program does the heavy lifting.
- There are only two collision detection algorithms. Axis Aligned Bounding Box (AABB) is the default algorithm. Also, you can reduce it to Spherical collision detection as well, which is simpler and works faster but it just checks for the bounding spheres of objects thus makes a pretty rough assumption.

## Install and kick-start

### Requirements:

- LibSDL2 `sudo apt install libsdl2-dev` for debian/ubuntu based linux distros. For other platforms, you can see your favourite package manager.
- imagemagick `sudo apt install imagemagick` for debian/ubuntu based linux distros. For other platforms, you can see your favourite package manager.
- Python 3.7+
- A Graphics card that supports OpenGL 3.3+

### Install:

    $ pip3 install payton


Sample code:

```python

from payton.scene import Scene
from payton.scene.geometry import Cube

scene = Scene()
cube = Cube()

scene.add_object("cube", cube)

scene.run()

```

This will bring up a default scene. You can press **h** from keyboard to show help window.
