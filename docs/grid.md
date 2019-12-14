---
title: "Grid"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Grid is a mandatory element of a 3D scene. Before anything, it arranges your perception of the 3D world acting as a virtual ground. Also, they are handy in two major ways. Seeing where your objects are and their alignment with the scene.

The scene has a pre-defined Grid. It is 20x20 in size and there is 1 unit of gap between each line.

If you are confused about the "units", you can check [Payton Scene 101](/docs/scene) page.

To change the grid layout of a scene, you can use the `resize` method as:

{{< highlight python >}}
from payton.scene import Scene

scene = Scene()
scene.grid.resize(30, 30, 0.5)

scene.run()
{{< /highlight >}}

This will resize the grid for 30x30 parallel/perpendicular lines with 0.5 units as gap.

### Colored lines on the grid

![Basic](/01_basic.png)

As you can see from the image above, Grid has 3 colored lines starting from the center and going to 3 different directions.

- **Red Line**: Goes along **X-Axis**
- **Green Line**: Goes along **Y-Axis**
- **Blue Line**: Goes along **Z-Axis**

So, it is easier to see how your objects are aligned in the scene by comparing them to coordinate axis lines.
