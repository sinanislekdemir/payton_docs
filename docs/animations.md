---
title: "Animations and Clocks"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Animating in Payton is easier than it looks. Payton has a built-in Clock. By using the clock, you can create any time-based animations for yourself.

The clock is an asynchronous Python thread and it runs parallel to your scene render cycle.

Ideally, you just need one clock in your scene and you can do a state-based animation, but you can have as many clocks as you wish. Just keep in mind that each thread will affect on the performance due to the global interrupt lock of Python. [read more here](https://wiki.python.org/moin/GlobalInterpreterLock)

Clock requires three parameters

* Name
* Period
* Callback

Initially, all clocks are in a paused state in the scene. You can hit the Space key from the keyboard to unpause all clocks in the scene.

In each "tick" of the clock, Payton will call the callback function with two arguments:

- period
- total time

The period is exactly what you define while creating the clock. Total time is the time passed from the first unpause in seconds.

A simple example using the state:

Cube 1 hits Cube 2. Waits for 1 second after the impact. Cube 2 continues to move forward.


<video controls="controls">
  <source type="video/mp4" src="/animations.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>


```python
from payton.scene import Scene
from payton.scene.collision import CollisionTest
from payton.scene.geometry import Cube, Plane

scene = Scene()
state = 0
hit_time = 0


def hit(collision, pairs):
    # we don't have to use these for now
    global state
    scene.objects["cube_1"].forward(-0.01)
    collision.resolve(*pairs[0])
    state = 1


def animation(period, total):
    global state, hit_time
    if state == 0:
        scene.objects["cube_1"].forward(0.01)
    if state == 1:
        hit_time = total
        state = 2
    if state == 2 and total - hit_time > 1.0:
        state = 3
    if state == 3:
        scene.objects["cube_2"].forward(0.01)


ground = Plane(10, 10)
cube_1 = Cube()
cube_1.position = [0, 0, 0.5]

cube_2 = Cube()
cube_2.position = [0, 2.0, 0.5]


scene.add_object("ground", ground)
scene.add_object("cube_1", cube_1)
scene.add_object("cube_2", cube_2)
scene.create_clock("animation", 0.02, animation)
scene.add_collision_test("test", CollisionTest(callback=hit, objects=[cube_1, cube_2]))

scene.run()
```
