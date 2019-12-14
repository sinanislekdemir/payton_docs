---
title: "Materials"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Each object can have multiple materials. Materials are as simple as they can be. Therefore, it does not have any detailed customization options. As mentioned, Payton 3D does not try to replace a fully-featured game engine or design platform. It is designed for ease of use and prototyping.

Each material can have a color, display mode (SOLID, WIREFRAME, POINTS), lighting support, opacity, and texture.

# Properties of a material

## color:

The color of a material is defined as a `List[float]` with 3 elements. Each element is within the `0.0-1.0` range. They represent Red, Green and Blue (as defined in [Grid](/docs/grid) section). 

Default `color` of a material is White `[1.0, 1.0, 1.0]`

Here are some example colors defined in `payton.scene.material`

```python
RED = [1.0, 0.0, 0.0]  # type: List[float]
GREEN = [0.0, 1.0, 0.0]  # type: List[float]
BLUE = [0.0, 0.0, 1.0]  # type: List[float]
CRIMSON = [220 / 255.0, 20 / 255.0, 60 / 255.0]  # type: List[float]
PINK = [1.0, 192 / 255.0, 203 / 255.0]  # type: List[float]
VIOLET_RED = [1.0, 62 / 255.0, 150 / 255.0]  # type: List[float]
DEEP_PINK = [1.0, 20 / 255.0, 147 / 255.0]  # type: List[float]
ORCHID = [218 / 255.0, 112 / 255.0, 214 / 255.0]  # type: List[float]
PURPLE = [128 / 255.0, 0.0, 128 / 255.0]  # type: List[float]
NAVY = [0.0, 0.0, 0.5]  # type: List[float]
ROYAL_BLUE = [65 / 255.0, 105 / 255.0, 225 / 255.0]  # type: List[float]
LIGHT_STEEL_BLUE = [176 / 255.0, 196 / 255.0, 222 / 255.0]  # type: List[float]
STEEL_BLUE = [70 / 255.0, 130 / 255.0, 180 / 255.0]  # type: List[float]
TURQUOISE = [0.0, 245 / 255.0, 1.0]  # type: List[float]
YELLOW = [1.0, 1.0, 0.0]  # type: List[float]
GOLD = [1.0, 225 / 255.0, 0.0]  # type: List[float]
ORANGE = [1.0, 165 / 255.0, 0.0]  # type: List[float]
WHITE = [1.0, 1.0, 1.0]  # type: List[float]
BLACK = [0.0, 0.0, 0.0]  # type: List[float]
DARK_GRAY = [0.2, 0.2, 0.2]  # type: List[float]
LIGHT_GRAY = [0.8, 0.8, 0.8]  # type: List[float]
```

The simplest way to set the color of an object is:

```python
from payton.scene import Scene
from payton.scene.geometry import Plane, Sphere
from payton.scene.material import PINK

plane = Plane(10, 10)
scene = Scene()

scene.lights[0].position = [-5, 5, 20]

sphere_1 = Sphere(2.0)
sphere_1.position = [0, 0, 2.0]
sphere_1.material.color = PINK

scene.add_object('ground', plane)
scene.add_object('sphere_1', sphere_1)

scene.run()
```

![color_pink](/color_1.png)


## display:

The display type defines how your objects will be rendered. They can be SOLID objects, WIREFRAME (made of lines) or POINTS (only draw vertices as dots in space**

In a regular Payton Scene, keyboard shortcut `w` changes the display mode for all objects in the scene. From SOLID -> WIREFRAME -> POINTS -> SOLID ... But in some cases, you might want to disable this for your object. So, there is a little "hack" to keep your objects display mode from changing. You can overwrite `toggle_wireframe` method of the object with a dummy lambda as you will see in the example below;

```python
from payton.scene import Scene
from payton.scene.geometry import Plane, Sphere
from payton.scene.material import WIREFRAME

plane = Plane(10, 10)
scene = Scene()

scene.lights[0].position = [-5, 5, 20]

sphere_1 = Sphere(2.0)
sphere_1.position = [0, 0, 2.0]
# Set the display type
sphere_1.material.display = WIREFRAME
# Don't let user to change display type of the object
sphere_1.toggle_wireframe = lambda: None

scene.add_object('ground', plane)
scene.add_object('sphere_1', sphere_1)

scene.run()
```

![wireframe](/wireframe_1.png)

## lights:

This is a boolean argument to define if the object is affected by the lights or not. If this is set to `False` then your objects will look flat:

**With Lights**
![Lights](/withlights.png)

**Without Lights**
![Lights](/nolights.png)


You can disable lighting for material as:

```python
object.material.lights = False
```

There can be more than one material in an object and each material can be different in terms of lighting.
So if you want to disable lighting for all materials in an object:

```python
for material in object.materials.values():
    material.lights = False
```

## texture:

Textures are "wrapping papers" of your objects. You can cover your object with an image you'd like. Ready-to-use primitives come with their texture coordinates. Texture Coordinates define how your image will be positioned on the objects.

`texture` parameter of a material defines the **filename** for the image path. Payton uses `Pillow` library which uses `Imagemagick` to read image data. Also, Payton supports transparent PNG images. They are pretty handy if you want to create objects like a tree. Creating each leaf with proper shapes would require a lot of vertices/polygons. But if you draw each leaf with only one triangle and then cover them with a leaf image with transparency, then it would look much more realistic like the example below:

```python
from payton.scene import Scene
from payton.scene.geometry import Plane

plane = Plane()
scene = Scene()
plane.material.texture = 'leaf.png'

scene.add_object('leaf', plane)
scene.run()

```

![leaves](/leaves.png)


### Scaling your texture!

Sometimes, you might want to repeat the same texture on the object surface. This is especially handy if you have a wall or ground. Usually, objects are "stretched" with images. If you want to repeat an image on the surface of the object, the easiest way to accomplish that is by scaling the object's texture coordinates.

```python
from payton.scene import Scene
from payton.scene.geometry import Cube, Plane

scene = Scene()

ground = Plane(10, 10)
cube = Cube()
cube.position = [0, 0, 0.5]
cube.material.texture = "/home/sinan/workspace/personal/payton/examples/basics/barrel.jpg"
cube.scale_texture(5, 10)

scene.add_object("ground", ground)
scene.add_object("cube", cube)

scene.run()

```

![scale_texture](/scale_texture.png)

## opacity

Opacity, in other terms "transparency" defines the transparency level of your object. This is defined between the `0.0 - 1.0` range. A solid and fully opaque object `opacity` is `1.0`. And a fully transparent object has `0.0` as transparency. But if you want to "hide" an object from the scene, using opacity is not the correct way to do that :)

```python
from payton.scene import Scene
from payton.scene.geometry import Plane, Sphere

plane = Plane(10, 10)
scene = Scene()

scene.lights[0].position = [-5, 5, 20]

sphere_1 = Sphere(2.0)
sphere_2 = Sphere()
sphere_2.position = [-2, -2, 0.5]

sphere_1.position = [0, 0, 1]

amount = -0.01


def opaque(period, total):
    global amount
    sphere_1.material.opacity += amount
    if sphere_1.material.opacity <= 0.0:
        amount = 0.01
    if sphere_1.material.opacity >= 1.0:
        amount = -0.01


scene.add_object('ground', plane)
scene.add_object('sphere_2', sphere_2)
scene.add_object('sphere_1', sphere_1)

scene.create_clock('change_opacity', 0.01, opaque)
scene.run()

```

<video controls="controls">
  <source type="video/mp4" src="/opacity.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

# Adding a material to an object

```python
import os

from payton.scene import Scene
from payton.scene.geometry import Mesh
from payton.scene.material import DEFAULT, Material

scene = Scene()
mesh = Mesh()

yellow_material = Material(lights=False, color=[1.0, 1.0, 0.0, 1.0])
mesh.add_material("yellow", yellow_material)

# Regular add_triangle will add it with DEFAULT material
mesh.add_triangle(
    [[0, 0, 0], [2, 0, 0], [2, 2, 0]], texcoords=[[0, 0], [1, 0], [1, 1]]
)

# Explicit material definition
mesh.add_triangle(
    [[0, 0, 0], [2, 2, 0], [0, 2, 0]],
    texcoords=[[0, 0], [1, 1], [0, 1]],
    material="yellow",
)

texture_file = os.path.join(os.path.dirname(__file__), "cube.png")
mesh.materials[DEFAULT].texture = texture_file
# mesh.material.texture = texture_file  # implicit declaration
scene.add_object("mesh", mesh)
scene.run()

```

![materials](/materials.png)
