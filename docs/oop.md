---
title: "Object Oriented Payton"
date: 2019-11-08T23:14:10+02:00
draft: false
---

In basic cases, Payton gives you to setup your scene in a quick and easy way. But sometimes, you require something a bit more complex than most of the examples.

Especially, the order of defining the callback methods or having to reach global variables is a bit disturbing. 

In any case, if you want to move things to a more object-oriented approach, you can simply extend Scene for your application and it should work fine.

As an example:

<video controls="controls">
  <source type="video/mp4" src="/animations.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>


```python
from payton.scene import Scene
from payton.scene.geometry import Cube, Plane
from payton.scene.collision import CollisionTest


class App(Scene):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.state = 0
        self.hit_time = 0
        self.prepare()

    def prepare(self):
        ground = Plane(10, 10)
        cube_1 = Cube()
        cube_1.position = [0, 0, 0.5]

        cube_2 = Cube()
        cube_2.position = [0, 2.0, 0.5]

        self.add_object("cube_1", cube_1)
        self.add_object("cube_2", cube_2)
        self.add_object("ground", ground)
        self.create_clock("animation", 0.02, self.animation)
        self.add_collision_test(
            "test", CollisionTest(callback=self.hit, objects=[cube_1, cube_2])
        )

    def hit(self, collision, pairs):
        self.objects["cube_1"].forward(-0.01)
        collision.resolve(*pairs[0])
        self.state = 1

    def animation(self, period, total):
        if self.state == 0:
            self.objects["cube_1"].forward(0.01)
        if self.state == 1:
            self.hit_time = total
            self.state = 2
        if self.state == 2 and total - self.hit_time > 1.0:
            self.state = 3
        if self.state == 3:
            self.objects["cube_2"].forward(0.01)


app = App()
app.run()
```
