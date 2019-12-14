---
title: "Lights"
date: 2019-11-08T23:14:10+02:00
draft: false
---
<video controls="controls">
  <source type="video/mp4" src="/rotate.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

Lights are, well, they are lights to illuminate your scene. In the good old times of OpenGL, when we were working
with fixed pipelines without GLSL, there were only 8 lights. This was a hard-limitation by OpenGL. So, you had to be clever while using your lights. Now, we have GLSL and we are cursed to calculate the illumination and shading by ourselves. So we can have more lights.

But still, I wanted to do a limitation as it was giving me easier control over things. So, you can have 100 lights in a scene. Any lights added after 100 will be ignored. _I suppose. I guess I forgot to test what happens at the 101st light_

Light has two parameters:

- Position
- Color

That's all. In a more complex environment, like a game engine editor or mightly Blender 3D or other software, lights are more complicated. They can be directional (like the sun) or point, or spotlight. Or they can have more values like intensity or maximum distance to illuminate. But here in Payton, we only have a point light. Adding more lighting options to a scene just seemed to get things more complicated and hard-to-understand for beginners. So if someday I get myself convinced that a newbie can work with more options, I can implement them.

Also to note, the scene has a pre-defined light source located at `x:10.0 y:7.0 z:6.0`

### Position:

Position is, again a `List[float]` with 3 elements. Default value of the position is `[10.0, 7.0, 6.0]`

You can dynamically change the position of a light source. Setting the position will already do all the necessary changes.

### Color:

Light color is also a `List[float]` type with 3 elements. By default, a light source is white. `[1.0, 1.0, 1.0]`. You can change the color of a light source as you wish.

### Shadows:

Only the initial light source defined in the scene is capable of casting shadows. This is `scene.lights[0]`. This is a performance-related restriction. For each light source in the scene, the system has to calculate 6 different matrices and render the whole scene for the light source from each matrix. This is done directly at the runtime render cycle, just before you see the scene yourself. So basically, with each shadow casting, you are reducing your frame-rate to half. (Or less, based on your shadow quality.**

**If you are craving for more performance and you don't need shadows, you can do this:**

{{< highlight python >}}
from payton.scene import SHADOW_NONE, Scene

a = Scene()
a.shadow_quality = SHADOW_NONE
{{< /highlight >}}

Setting the `shadow_quality` to `SHADOW_NONE` _(or `0`)_ will disable shadow related codes. 

Shadow Quality is an integer. There are pre-defined values for it:

    SHADOW_NONE = 0
    SHADOW_LOW = 512
    SHADOW_MID = 1024
    SHADOW_HIGH = 2048
    
The default scene shadow quality is `SHADOW_MID`. 

Shadow quality is the viewport resolution for the light source. We render the scene from the lights' point of view and generate the mapping to see which "fragments" are visible to the light source. So we can calculate those spots as bright and other places as dark. 

So, setting `shadow_quality` to a very high value can end-up with very low performance OR no shadows at all.

To add a new light source to your scene, you can do this:

{{< highlight python >}}
from payton.scene import Scene
from payton.scene.light import Light

scene = Scene()
light = Light(position=[-1, -1, -5])
scene.lights.append(light)

scene.run()
{{< /highlight >}}

Simple as that. Because `lights` is literally just a plain list.
