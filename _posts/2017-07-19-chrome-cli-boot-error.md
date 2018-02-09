---
layout: post
title: chrome在命令行下启动报错
categories: Linux
description: chrome在命令行下启动报错
keywords: linux
---

# google-chrome

Depending on the current build, if you run the container interactively you may see things like this on the console:

```
[0501/162901.033074:WARNING:audio_manager.cc(295)] Multiple instances of AudioManager detected
[0501/162901.033169:WARNING:audio_manager.cc(254)] Multiple instances of AudioManager detected
```

In most cases, these messages can be safely ignored. They will sometimes change and eventually as things are updated in the source tree, resolved.


