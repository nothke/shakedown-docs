Welcome to the documentation for [Shakedown Rally](https://nothke.itch.io/shakedown)!

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