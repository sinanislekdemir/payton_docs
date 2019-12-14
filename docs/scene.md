---
title: "Scene Class"
date: 2019-11-08T23:14:10+02:00
draft: false
---
![Basic](/01_basic.png)
`Scene` is the core class of Payton. Payton is made of small units like mesh or light but
they all come together inside Scene.

### Units:

In a 3D environment, the first question comes up to your mind is "is this meters or inches or what?". 

Actually, it is just a "unit". Just a mathematical number. That's all. The rest is just how you want to take it. 1 Units in the scene can be assumed as 1 meter but on a great scale, it can look like a millimeter. Or 1 unit can be assumed as 1 kilometer. It is all just how you scale other things in the scene.

Things make sense only when you begin to compare it with other things. Our daily-life units are not any different from that. So, while starting a new Scene, make an assumption and follow it.

If you want to create a cube 1 meter wide, use `Cube(width=1.0)` and if you want to put another cube next to it, just 10 centimeters in width, then the scale is obvious. Use `Cube(width=0.1)` and it will look like 10 centimeters next to the first cube.

For the sake of floating-point precisions, try to avoid using too small numbers. Scale-up if you need to.

### Colors:

Overall, all color definitions are made by RGB(a) notation. All elements are between 0 and 1.

Some simple examples:

```py3
RED = [1.0, 0.0, 0.0, 1.0]
GREEN = [0.0, 1.0, 0.0, 0.5]  # Half Transparent Alpha = (0.5)
BLUE = [0.0, 0.0, 1.0, 1.0]
PINK = [1.0, 0.753, 0.796, 1.0]
```


```py3
from payton.scene import Scene

scene = Scene()
scene.run()
```

simply creates and runs the initial Scene. A default scene comes with a pre-defined:

* Background
* Grid
* Axis-Coordinate Lines
* Keyboard shortcuts
* Mouse controls
* Help window triggered with `h` key

You can create a scene with some additional parameters:

Create a scene window with `1024x768` size:

```python
from payton.scene import Scene

scene = Scene(width=1024, height=768)
scene.run()
```

Enable object selection:

```python
from payton.scene import Scene

def select(selection_list):
    selected = len(selection_list)
    print(f"{selected} objects are selected")
    
scene = Scene(on_select=select)
```
