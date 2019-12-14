---
title: "Camera"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Payton comes with a fully-configured camera. The technical term for the camera in Payton is `Observer`.  
The main reason for this unusual naming is all because of the Fringe TV Series. If you haven't watched it yet, you should.

Payton scene already includes a pre-defined camera. You can reach the active camera by:

{{< highlight python >}}
from payton.scene import Scene

scene = Scene()
camera = scene.active_observer
# OR
camera = scene.observers[0]
{{< /highlight >}}

Each camera has:

- Position, target or target object
- Camera UP vector definition
- Field of View (FOV) `default=45.0`
- Aspect Ratio (set automatically by Scene so it is not over-writeable)
- Far Plane (Max. Distance to be drawn) `default=100.0`
- Near Plane (Nearest Distance to be drawn) `default=0.1`
- Zoom Factor (For Orthographic View) `default=10`

### Position:

Position of a camera is defined as a `List[float]` with 3 elements.

{{< highlight python >}}
from payton.scene import Scene

scene.active_observer.position = [10, 10, 10]
{{< /highlight >}}


***NOTE:*** Changing the position will not affect where the camera points. In any position, the camera will continue to point towards its target.

### Up Vector:

By default, Payton is written as Z-Axis as UP Direction. This might not be the common convention but I find this easier to understand. So what is Up Vector? Simply speaking, it's which way "up" is for you when someone tells you to look "up".

### Field Of View:

Field of View is the view angle of your camera. Imagine yourself in the center of a square room, facing a wall. In a 90' FOV, 

The image below is taken from https://www.oemcameras.com/fov_comparison might give a better understanding.

![FOV](/FoV_Rule_of_Thumb_03.jpg)

### Aspect Ratio:

Aspect Ratio is the width/height ratio of the viewport. When window size changes, Scene automatically adjusts this value.

### Far Plane & Near Plane:

![Planes](/plane.gif)

*"Near, far, wherever you are..."* Yep. Near and Far planes are the restrictions on your camera. The camera will draw objects between those distances. This is good for optimization. You don't have to draw what you don't see. Also, near plane is a handy variable to cut through objects. For instance, if there are objects inside a cube, and you need to cut through the cube to show the contents, adjusting near plane is an easy way to do it.

### Zoom Factor:

Zoom Factor is used for Orthographic Projection. By default, changing the distance between camera and target has no power on getting close to target in Orthographic Projection.

As you can see from the Wikipedia Image:

![Isometric][/isometric.png]

Things are all parallel, so to make the things look "bigger" we need to scale the view.
