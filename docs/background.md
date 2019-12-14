---
title: "Background"
date: 2019-11-08T23:14:10+02:00
draft: false
---
<video controls="controls">
  <source type="video/mp4" src="/background.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Usually, 3D scene backgrounds are black. This is generally due-to OpenGL clear color is set that way. Also, the black background shows the objects better. I admit that. On the other hand, it looks really ugly. 

As an alternative, I decided to implement a gradient background shader.

### Changing Background Color:

Background color is again a `List[float]` type. This time with 4 elements.

{{< highlight python >}}
from payton.scene import Scene

scene = Scene()
scene.background.top_color = [0, 0, 0, 1]
scene.background.bottom_color = [0, 0, 0, 1]

scene.run()
{{< /highlight >}}


### Changing Background Color by Time:

Background supports time-based change. There are four pre-defined colors. For midnight, early morning, afternoon and evening. When you set background to an exact hour and minute, based on those pre-defined colors, the system calculates an average color for the given time.

Hour is given in the 0-23 range.

{{< highlight python >}}
from payton.scene import Scene

scene.background.set_time(13, 50)
{{< /highlight >}}
