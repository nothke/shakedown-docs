Welcome to the documentation for [Shakedown Rally](https://nothke.itch.io/shakedown)!

If you encountered a bug, [submit an issue here](https://github.com/nothke/shakedown-docs/issues).

### Rebinding controls

You can set your own controls in config.ini [bindings] segment. Keys map to numbers that you can find on [this page](https://www.glfw.org/docs/3.3/group__keys.html). For example, to map throttle, break and steering to arrows:

```
steerLeft = 263
steerRight = 262
throttle = 265
brake = 264
```

<sup>(thx to [Henke](https://itch.io/t/2612242/psa-rebinding-controls))</sup> 

### Changelog
#### Build 849 (version 2)
* Initial release
#### Build 869 (version 3)
* Fixed neutral gear which caused crashes due to division by zero
* *HUD can now be toggled (default: tab)
* Camera movement is now bindable and defaults are numpad arrows
* Camera FoV can now be changed in any camera mode (using + and - by default)
* Auto shifting no longer upshifts from reverse
#### Build 870 (version 4)
* Fixed crash when alt-tabbing in fullscreen
#### Build 879 (version 5)
* Logger now flushes on warnings by default so that log.txt gets filled before crashes. Custom flush level can be set in config flushOnLevel, increasing the number lowers the threshold level needed for flush
* Added Windows version info to log