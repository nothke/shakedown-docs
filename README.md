![logo](https://img.itch.zone/aW1nLzExMTA1NzU3LnBuZw==/original/bqf3kG.png)

Welcome to the documentation for [Shakedown Rally](https://nothke.itch.io/shakedown)!

If you encountered a bug, [submit an issue here](https://github.com/nothke/shakedown-docs/issues).

Join [my discord server](https://discord.com/invite/R8jXeTg) to discuss about the game and get the latest updates.

### FAQ

#### How do I reverse?

You need to shift down through gears (`<` by default) until you reach reverse gear. Will add holding brake to reverse at some point.

#### How do I fly the camera around freely?

Switch through the cameras (with C by default) until the camera stops moving -> this is actually the free look camera! Now you can hold right click to look around and move the camera around (with numpad keys by default). Sorry that it isn't that clear, I will add some in-game tooltips in the future.


### Rebinding controls

You can set your own controls in config.ini [bindings] segment. Keys map to numbers that you can find on [this page](https://www.glfw.org/docs/3.3/group__keys.html). For example, to map throttle, break and steering to arrows:

```
steerLeft = 263
steerRight = 262
throttle = 265
brake = 264
```

<sup>(thx to [Henke](https://itch.io/t/2612242/psa-rebinding-controls))</sup> 

### Modding support

My goal for the game is to make modding as easy as possible, BUT it is not yet officially mod "friendly". It doesn't even have track or car picking, nor good error messages if you do something wrong. Nothing stops you from changing the car parameters or asset files right now as they're "out in the open" but as I am still changing everything I advise you to wait a bit as I will make your life easier.

See [Modding](modding.md) for preliminary modding tips.

### Known issues
* Trees have no collisions
* If the game stutters on laptops, make sure you play the game with your dedicated GPU as many times laptops will default to integrated GPU. You can set that in Windows "Graphics Settings"
* If the game looks blurry and you have a high-dpi screen, the OS might override the application setting. Right click on the exe -> Compatibility -> Change High DPI settings -> check "Override high DPI scaling behavior" and select Application from the drop down
* There are some known issues with Windows 7, please submit an issue with a log file if you are using one and have problems

### Changelog
#### Build 849 (version 2)
* Initial release
#### Build 869 (version 3)
* Fixed neutral gear which caused crashes due to division by zero
* HUD can now be toggled (default: tab)
* Camera movement is now bindable and defaults are numpad arrows
* Camera FoV can now be changed in any camera mode (using + and - by default)
* Auto shifting no longer upshifts from reverse
#### Build 870 (version 4)
* Fixed crash when alt-tabbing in fullscreen
#### Build 879 (version 5)
* Logger now flushes on warnings by default so that log.txt gets filled before crashes. Custom flush level can be set in config flushOnLevel, increasing the number lowers the threshold level needed for flush
* Added Windows version info to log
#### Build 902 (version 6)
* Reverse lights now work
* Onboard camera position can now be set in config
* Wheel radius is no longer hardcorded and can be set in config
* Added suspension debug lines
* Fog parameters are now in [fog] section in config