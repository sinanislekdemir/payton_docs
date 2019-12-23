---
title: "Quake2 - MD2"
date: 2019-11-08T23:13:18+02:00
draft: false
---

Quake 2 was one of the best games ever. It influenced many developers to create greater games. Also, as a game engine, Quake 2 had
countless excellent features and optimizations.

Even-though, there are many newer formats for animated meshes like DAE or such, MD2 had always been my favorite format. It is as
simple as it could be. It doesn't support any skeletal animations. It is simply a frame-by-frame animation, just like a cartoon or GIF

As an optimization method, not all frames exists in a md2 file. Parser needs to do some interpolations between the key-frames.
But, that part of the animation system is ignored in Payton. _At least for now_. But still gives you a decent animation.

You can find many models from: 

* [http://md2.sitters-electronics.nl/models.htm](http://md2.sitters-electronics.nl/models.htm)
* [http://planetquake.gamespy.com/View2e38.html](http://planetquake.gamespy.com/View2e38.html?view=Modeloftheweek.List&game=5&category_select_id=2)

Or from the [games built with Quake 2 Engine](https://en.wikipedia.org/wiki/Quake_II_engine**


Payton handles Quake 2 Files as a main object and it's children. Each frame inside the MD2 is a child of main object.

<video controls="controls" width="100%">
  <source type="video/mp4" src="/quake2.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

** Getting the list of possible animations

Animations are defined as a dictionary:

```python
from payton.scene.geometry import MD2

md2_model = MD2("tris.md2")
print(md2_model.animations)
```

should return a dictionary that looks like:

```python
{
  'stand': [0, 39],
  'run': [0, 5],
  'attack': [0, 7],
  'pain': [0, 11],
  'jump': [0, 5],
  'flip': [0, 11],
  'salute': [0, 10],
  'taunt': [0, 16],
  'wave': [0, 10],
  'point': [0, 11],
  'death': [0, 19]
}
```

Each key in the dictionary is the name of given animation and corresponding list of integers are starting and ending frames for the given animation.

In this example, `run` animation is defined by 5 frames. 

To animate your object, you can use this:

```python
from payton.scene import Scene
from payton.scene.geometry import MD2


class App(Scene):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        model = MD2("tris.md2")
        self.add_object("warrior", model)
        self.objects["warrior"].animate('run', 0, 5)


app = App(width=1600, height=900)
app.run()

```

In some cases, like death, your animation might not be looping. You wouldn't want to watch your character to die over
and over again. In that case, you can add `loop=False` to animate command.

```python
from payton.scene import Scene
from payton.scene.geometry import MD2


class App(Scene):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        model = MD2("tris.md2")
        self.add_object("warrior", model)
        self.objects["warrior"].animate('run', 0, 5, loop=False)


app = App(width=1600, height=900)
app.run()

```

so, it will just run the animation for once.

## Custom textures

In good old days, Quake 2 models used to have more than one textures/skins. The idea was to have a separate texture for each condition of the model.
If the model is badly hurt, then there would be a model texture covered in blood. So, it was still one texture. This is a nice way of optimization.

To have the same effect, you can explicitly reset the material by using `set_texture` method.
