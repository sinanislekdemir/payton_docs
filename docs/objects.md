---
title: "Working with Objects"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Objects are basically geometrical shapes in 3D.

All objects share the same base class which is:

```python
from payton.scene.geometry.base import Object
```

This is a very generic class that can handle all kinds of object drawing operations from lines to points, triangular meshes to On Screen Display elements like Window or Text.


There are two types of objects in Payton:

- Primitives:
  - Cube
  - Cylinder
  - Line
  - Plane
    - MatrixPlane
  - PointCloud
  - Sphere
- Mesh:
  - Mesh  
  - Quake2 - MD2
  - Wavefront
  - Ragdoll
  
In this section, I will try to explain common use-cases, properties of all objects. But before digging into that, here is a small example on how to create each of the objects:

```python
import math

from payton.scene import Scene
from payton.scene.geometry import (MD2, Cube, Cylinder, Line, MatrixPlane,
                                   Mesh, Plane, PointCloud, RagDoll, Sphere,
                                   Wavefront)

scene = Scene()
scene.lights[0].position = [1, 2, 10]

ground = Plane(10, 10)
cube = Cube(width=1.0, depth=0.5, height=0.8)
cube.position = [-4, -4, 0.4]

cylinder = Cylinder(
    bottom_radius=0.5, top_radius=0.3, meridians=24, height=1.0
)
cylinder.position = [-2, -4, 0.5]

md2 = MD2(
    "/home/sinan/workspace/personal/payton/examples/basics/infantry/tris.md2"
)
md2.position = [-1, 1, 0]

line = Line()
line.append(
    vertices=[
        [0, 0, 0],
        [0, 0, 1],
        [0.5, 0, 1.5],
        [1, 0, 1],
        [0, 0, 1],
        [1, 0, 0],
        [0, 0, 0],
        [1, 0, 1],
        [1, 0, 0],
    ]
)
line.position = [1, -4, 0.0]
line.material.color = [1, 0, 0]

matrix_plane = MatrixPlane(width=2.0, height=2.0, x=10, y=10)
matrix_plane.position = [3.5, -4, 0.0]

for i in range(10):
    for j in range(10):
        matrix_plane.grid[i][j] = math.sin(math.radians(i * 18))
matrix_plane.update_grid()

mesh = Mesh()
mesh.add_triangle(
    [[0, 0, 0], [2, 0, 0], [2, 2, 0]], texcoords=[[0, 0], [1, 0], [1, 1]]
)
mesh.add_triangle(
    [[0, 0, 0.0], [2, 2, 0.0], [0, 2, 0.5]], texcoords=[[0, 0], [1, 1], [0, 1]]
)
mesh.position = [-4, -2, 1]

plane = Plane(width=2.0, height=1.0)
plane.rotate_around_x(math.radians(90))
plane.position = [-1, -2, 0.5]

point_cloud = PointCloud()
point_cloud.add([[0, 0, 0], [1, 0, 1], [0, 1, 1], [0.5, 0.5, 0.5]])
point_cloud.position = [2, -2, 1.5]

ragdoll = RagDoll()
ragdoll.position = [4, -2, 2]

sphere = Sphere(radius=0.5, parallels=12, meridians=12)
sphere.position = [-2, 3, 0.5]

wavefront = Wavefront(
    "/home/sinan/workspace/personal/payton/examples/basics/monkey.obj"
)
wavefront.position = [2, 2, 1]

scene.background.set_time(12, 0)

scene.add_object("ground", ground)
scene.add_object("cube", cube)
scene.add_object("cylinder", cylinder)
scene.add_object("md2", md2)
scene.add_object("line", line)
scene.add_object("matrix_plane", matrix_plane)
scene.add_object("mesh", mesh)
scene.add_object("plane", plane)
scene.add_object("point_cloud", point_cloud)
scene.add_object("ragdoll", ragdoll)
scene.add_object("wavefront", wavefront)
scene.add_object("sphere", sphere)

scene.run()

```

![all_objects](/all_objects.png)

# Object Structure and properties:

## children

`Dict[str, Object]` type. Each object can have many child objects. Each child's coordinate system is relative to its parent. This means, a child will always follow its parent object and it will move together.

The best way to describe this is: Earth is the main object and Moon is its child. Moon will continue rotating around Earth while Earth is rotating around Sun. There is no need to calculate the Moon's position relative to Sun. Just keep The Moon moving around Earth and it will be following Earth's motion around Sun.

Names of the children within the same object should be unique!

Example usage:

```python
from payton.scene import Scene
from payton.scene.geometry import Cube

scene = Scene()
main_object = Cube()
child_object = Cube()
child_object.position = [2, 0, 0]

main_object.add_child("cube_at_2", child_object)
scene.add_object("main_object", main_object)

scene.run()
```

The example `05_children.py` can give more clarity on that:

<video controls="controls" width="100%">
  <source type="video/mp4" src="/05children.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>


## materials:

See the [materials](/docs/materials) section for details.

## static

Static is an optimization parameter. It is set to `True` by default. If an object is defined as `Static` then it's virtual buffer objects are deleted from the graphics memory after building the vertex array object and also the object gets reported to the graphics card as a static object. This way, the graphics card can internally optimize itself.

### visible

Default: `True`. Defines the visibility of an object. 

To hide an object on runtime, you can use:

```python
sphere_object.hide()
```

and to show it back:

```python
shere_object.show()
```

This is the correct way to show and hide an object rather than changing its opacity to `0`

# Some practices on Objects

## Turning your object towards a direction (object following a point)

This is a very useful practice, especially for gaming. When you want to turn your objects towards a point in space, you usually need to fiddle with the matrix of the object. But you don't have to know about all the vector and matrix operations, therefore Payton Objects come with a basic method called `direct_to`

```python
from payton.scene import Scene
from payton.scene.geometry import Cube

scene = Scene()
cube = Cube()
cube.direct_to([4, 4, 5])

scene.add_object('cube', cube)
scene.run()
```

## Moving your object

Moving your object is about setting the position of it. But in many cases, calculating **the next position** can be trickier than setting the position itself. For this reason, with the general assumption that "objects usually move towards their direction", Payton Objects come with a method called `forward`. `forward` method moves the object towards its direction by the given amount.

```python
from payton.scene import Scene
from payton.scene.geometry import Cube

scene = Scene()
cube = Cube()
cube.direct_to([4, 4, 5])

def move(period, total):
    cube.forward(0.01)
    if cube.position[2] > 4:
        cube.position = [0, 0, 0]

scene.add_object('cube', cube)
scene.create_clock('move', 0.01, move)
scene.run()

```

<video controls="controls" width="100%">
  <source type="video/mp4" src="/forward.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

## Tracking your objects

Payton objects come with a handy option. Object tracking. Basically, it is a line that shows the motion path of an object. This is super handy if you want to trace back your object in time. 

This is a disabled property by default. It has effects on the performance as the system tries to determine the matrix changes and updates the track history accordingly. Therefore, to enable object tracking, you need to create your object with `track_motion=True` argument. 

Please note that each matrix change is stored in RAM and gets pushed into the graphics card in every render cycle. So long-running processes and tracking too many objects for long changes will cause a performance drop.

```python
import logging
import math

from payton.scene import SHADOW_HIGH, Scene
from payton.scene.geometry import Plane, Sphere
from payton.scene.gui import info_box

logging.basicConfig(level=logging.DEBUG)


LAUNCH_ANGLE = math.radians(30)  # 30 Degrees
GRAVITY = 9.8
INITIAL_VELOCITY = 20  # meters/seconds


def projectile_motion(period, total):
    # projectile motion.
    # y = v0 * t * cos(a)
    # z = v0 * t * sin(a) - 1/2 * g * t^2
    global scene
    position = scene.objects["ball"].position
    if position[2] < 0:
        # Do not continue simulation if we hit the ground.
        scene.clocks["motion"].kill()  # We do not need this clock anymore
        return None

    # Go towards -Y direction.
    position[1] = -(INITIAL_VELOCITY * total * math.cos(LAUNCH_ANGLE))
    position[2] = INITIAL_VELOCITY * total * math.sin(
        LAUNCH_ANGLE
    ) - 0.5 * GRAVITY * (total ** 2)
    scene.objects["ball"].position = position
    return None


#  Definitions
scene = Scene()
scene.shadow_quality = SHADOW_HIGH

ball = Sphere(radius=1, track_motion=True)

# Add ball to the scene
scene.add_object("ball", ball)

scene.grid.resize(30, 30, 2)
ground = Plane(80, 80)
scene.add_object("gr", ground)

scene.create_clock("motion", 0.005, projectile_motion)

scene.add_object(
    "info",
    info_box(
        left=10,
        top=10,
        width=220,
        height=100,
        label="Hit SPACE\nto start animation",
    ),
)

scene.run()
```

<video controls="controls" width="100%">
  <source type="video/mp4" src="/track.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

## Local to Absolute coordinates! And why you might need them.

<video controls="controls" width="100%">
  <source type="video/mp4" src="/spotlight.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>


As you can see from the example, the light seems to be following the spotlight. But there is a catch. Lights can't have a parent object in Payton. _At least not yet_

So, how can I make the light follow the spotlight easily? The answer is to get the coordinates myself while the spotlight is moving.

There are two ways to do this. The first way is to calculate the position of the light with respect to angular changes in the spotlight. The second way is to pick a "local** coordinate from the spotlight and convert it to absolute, then set it to lights position.

***Local coordinates:*** Local coordinates are relative to objects matrix.

Assume that you have a bike and a biker. And you don't want biker to be a child of the bike because you need the biker elsewhere. She has to get off the bike and do something else. In this case, we can assume that the bicycle's saddle is at `[0, 1, 1]` coordinates with respect to the bike's origin.

So, while the bike is at `[0, 0, 0]` the saddle is at `[0, 1, 1]` and you can position the biker to `[0, 1, 1]` so it will look like riding the bike. But if the bike starts to move forward, the biker will remain where she is.

If bike moves 10 units along X-Axis then the saddle is now at `[10, 1, 1]` and the biker is at `[0, 1, 1]`. But if you "convert" `[0, 1, 1]` coordinate from **bike's local** to **absolute** _(Scene Root)_ coordinates, then you'll get `[10, 1, 1]` so; you can position your biker there.

In the above example, we have a code like this to make light follow the spotlight:

```python
    light_pos = scene.objects["lamp"].to_absolute([0, 0, -3.4])
    scene.lights[0].position = light_pos
```

So, we are not trying to recalculate the light position. As `[0, 0, -3.4]` is not changing in objects local, we can simply convert it to absolute coordinates and let things roll.


## Scaling your objects.

Sometimes, especially when you are importing into Payton, objects might look too big or too small and you might want to scale them. You can use the simple `scale` method for that.

```python
object3d.scale(5, 5, 5)
```

will scale your object 5 times. Note that, object scaling requires 3 parameters for `X, Y, Z` axis. You can scale your object in any axis you'd like. If you just want to change the height of your object:

```python
object3d.scale(1, 1, 5)
```

would do the trick. 

**Important!** Scaling is a multiplication operation on the objects matrix. Therefore, if you don't want to change the scale in an axis, you will have to use `1.0` as the scaling factor. As in `10*1 = 10` so the size won't change.

## Rotating your objects.

Rotation is quite easy with Payton. There are 3 pre-defined methods to create a rotation matrix and they multiply objects matrix with the rotation matrix.

```python
object3d.rotate_around_x(math.radians(30))
object3d.rotate_around_y(math.radians(30))
object3d.rotate_around_z(math.radians(45))
```

will rotate the object around its X axis by 30 degrees, then around Y axis by 30 and finally 45 degrees around Z axis.

## Object Picking

Object Picking is one of the hardest things to accomplish if you are not familiar with projection matrices and matrix conversions. 

Payton handles object picking for both Isometric and Perspective view modes.

Payton uses a bounding sphere intersection test by default. This is not an accurate picking method especially if your object is longer in one axis, like a stick. But as the triangle intersection test is costly in terms of an interpreted language, this optimization is acceptable.

In addition, Payton returns a list of selected/picked objects.

### Important Note

**Payton returns a list of all objects in the picking point. It is up to you to decide which object is the nearest to the camera. You can do a little check as:**


<video controls="controls" width="100%">
  <source type="video/mp4" src="/picking.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>


```python
import random

from payton.math.vector import distance
from payton.scene import Scene
from payton.scene.geometry import Cube
from payton.scene.gui import info_box

scene = Scene()


def select(list):
    # Check for the nearest selected object
    d = distance(list[0].position, scene.active_observer.position)
    selected = list[0]
    for obj in list:
        test = distance(obj.position, scene.active_observer.position)
        if test < d:
            selected = obj
    selected.material.color = [1, 1, 1]


scene.on_select = select

for i in range(10):
    x = random.randint(-5, 5)
    y = random.randint(-5, 5)
    z = random.randint(-5, 5)
    r = random.randint(0, 255) / 255.0
    g = random.randint(0, 255) / 255.0
    b = random.randint(0, 255) / 255.0
    cube = Cube()
    cube.material.color = [r, g, b]
    cube.position = [x, y, z]
    scene.add_object("cube_{}".format(i), cube)

scene.add_object(
    "info",
    info_box(
        left=10, top=10, width=220, height=100, label="Try clicking cubes"
    ),
)


scene.run()

```
## Show-Hide Object

Showing and hiding objects are pretty easy. 

```python
object.hide()
```

hides an object.

To show an object:

```python
object.show()
```
