---
title: "Mesh"
date: 2019-11-08T23:13:18+02:00
draft: false
---
Many objects in Payton are inherited from the mesh class. Mesh class is inherited from the base object.

Mesh is a triangulated object with extra methods to support triangle operations and building triangulated objects.

## Generating an object using Mesh

A Mesh is made of triangles and for each triangle, you need to define 3 points in space.

A Triangle in 3D has:

- **Three Vertices**: Three points in space that define a triangle. (P1=[x1, y1, z1], P2=[x2, y2, z2], P3=[x3, y3, z3])
- **Three Normals** (Optional, can be auto-generated): Normal of a vertex tells us the direction of the face. Is it looking up? `(0, 0, 1)` or is it looking left? `(1, 0, 0)`
- **Three Texture coordinates** (Optional unless you want to use textures): see [materials](/docs/materials) for details.
- **Three colors**: Optional, but you can define a different color to each vertex and create a gradient look.

### Important Note:

The order of vertices is important. There are two directions to define a triangle. Your vertices can follow a Clockwise or Counter-Clockwise direction. 

![Clockwise](/cw.png)

Payton uses clockwise rotation to calculate the normals. If your triangle follows a counter-clockwise rotation, then it won't be responding to light properly. 

![Basic Mesh](/mesh_1.png)

```python
from payton.scene import Scene
from payton.scene.geometry import Mesh


scene = Scene()
mesh = Mesh()
mesh.add_triangle(
    vertices=[
        [0, 0, 0],
        [1, 0, 0],
        [2, 1, 0],
    ],
    normals=[
        [0, 0, 1],
        [0, 0, 1],
        [0, 0, 1],
    ],
    texcoords=[
        [0, 0],
        [0.5, 0],
        [1, 1],
    ],
    colors=[
        [1, 0, 0],
        [0, 1, 0],
        [0, 0, 1]
    ]
)

mesh.add_triangle(
    vertices=[
        [1, 0, 0],
        [2, 1, 2],
        [2, 1, 0],
    ],
    normals=[
        [0, 0, 1],
        [0, 0, 1],
        [0, 0, 1],
    ],
    texcoords=[
        [0, 0],
        [0.5, 0],
        [1, 1],
    ],
    colors=[
        [1, 0, 0],
        [0, 1, 0],
        [0, 0, 1]
    ]
)
scene.add_object('mesh', mesh)

scene.run()
```

## Generating Normals:

Three are three ways to generate normals for your triangle.

The first way is to leave the `normals` argument empty. In this case, Payton will calculate the face normal and assign the face normal to each vertex in the triangle.

![Auto normals](/mesh_2.png)

```python
from payton.scene import Scene
from payton.scene.geometry import Mesh


scene = Scene()
scene.lights[0].position = [-4, 4, 10]
mesh = Mesh()
mesh.add_triangle(
    vertices=[
        [0, 0, 0],
        [1, 0, 0],
        [2, 1, 0],
    ],
)

mesh.add_triangle(
    vertices=[
        [1, 0, 0],
        [2, 1, 2],
        [2, 1, 0],
    ],
)

scene.add_object('mesh', mesh)

scene.run()
```


The second way is to calculate them yourself. Payton's math library has a function to do this.

```python
from payton.math.vector import plane_normal
from payton.scene import Scene
from payton.scene.geometry import Mesh

normal_1 = plane_normal(
    [0, 0, 0],
    [1, 0, 0],
    [2, 1, 0]
)

normal_2 = plane_normal(
    [1, 0, 0],
    [2, 1, 2],
    [2, 1, 0],
)

scene = Scene()
scene.lights[0].position = [-4, 4, 10]
mesh = Mesh()
mesh.add_triangle(
    vertices=[
        [0, 0, 0],
        [1, 0, 0],
        [2, 1, 0],
    ],
    normals=[normal_1, normal_1, normal_1]
)

mesh.add_triangle(
    vertices=[
        [1, 0, 0],
        [2, 1, 2],
        [2, 1, 0],
    ],
    normals=[normal_2, normal_2, normal_2]
)

scene.add_object('mesh', mesh)

scene.run()

```

And the third way is to call the `fix_normals` method after adding all triangles.

```python
from payton.scene import Scene
from payton.scene.geometry import Mesh

scene = Scene()
scene.lights[0].position = [-4, 4, 10]
mesh = Mesh()
mesh.add_triangle(
    vertices=[
        [0, 0, 0],
        [1, 0, 0],
        [2, 1, 0],
    ],
)

mesh.add_triangle(
    vertices=[
        [1, 0, 0],
        [2, 1, 2],
        [2, 1, 0],
    ],
)

mesh.fix_normals()

scene.add_object('mesh', mesh)

scene.run()

```

This is a pretty handy method. In some cases, your object's normals can be broken due to a miscalculation or an invalid external format. So you can just fix them in this way.


## Generating Texture Coordinates:

In some cases, creating the texture coordinates might be hard to do and you just want to see the texture in some way. So, Payton Mesh has a method called `fix_texcoords` just like `fix_normals`. It does a cube-map projection onto your object.

![TexCoords](/mesh_3.png)

```python
from payton.math.vector import plane_normal
from payton.scene import Scene
from payton.scene.geometry import Mesh

normal_1 = plane_normal(
    [0, 0, 0],
    [1, 0, 0],
    [2, 1, 0]
)

normal_2 = plane_normal(
    [1, 0, 0],
    [2, 1, 2],
    [2, 1, 0],
)

scene = Scene()
scene.lights[0].position = [-4, 4, 10]
mesh = Mesh()
mesh.add_triangle(
    vertices=[
        [0, 0, 0],
        [1, 0, 0],
        [2, 1, 0],
    ],
)

mesh.add_triangle(
    vertices=[
        [1, 0, 0],
        [2, 1, 2],
        [2, 1, 0],
    ],
)

mesh.fix_texcoords()
mesh.material.texture = '~/barrel.jpg'

scene.add_object('mesh', mesh)

scene.run()

```
