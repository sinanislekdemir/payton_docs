---
title: "Getting Help"
date: 2019-11-08T23:14:10+02:00
draft: false
---
Payton has a pre-built help window, which can be triggered by pressing `h` from the keyboard.  
If you want to show the help window from the beginning, you can try this:

{{< highlight python >}}
from payton.scene import Scene

scene = Scene()
scene.huds["_help"].show()
scene.run()
{{< /highlight >}}

![Help](/help.png)
