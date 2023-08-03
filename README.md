![logo](https://img.itch.zone/aW1nLzExMTA1NzU3LnBuZw==/original/bqf3kG.png)

Welcome to the documentation for [Shakedown Rally](https://nothke.itch.io/shakedown)!

If you encountered a bug, [submit an issue here](https://github.com/nothke/shakedown-docs/issues).

Join [my discord server](https://discord.com/invite/R8jXeTg) to discuss about the game and get the latest updates.

### FAQ

#### How do I reverse?

You need to shift down through gears (`<` by default) until you reach reverse gear. Will add holding brake to reverse at some point.

#### How do I fly the camera around freely?

Switch through the cameras (with C by default) until the camera stops moving -> this is actually the free look camera! Now you can hold right click to look around and move the camera around (with numpad keys by default). Sorry that it isn't that clear, I will add some in-game tooltips in the future.

### Default controls

- `W`, `S`, `A`, `D` - Throttle, Brake and Steer
- `Shift` - Clutch (not needed when changing gears)
- `Enter` - Toggle between auto and manual gear shifting
- `<`, `>` - switch gears
- `N` - Set to neutral gear
- `F` - Unflips the car and adds 10 seconds
- `C` - Switch camera
- `Esc` - Restart stage
- `tab` - Toggle HUD
- `Ctrl` - Toggle mouse lock
- `Numpad +` - Decrease FoV (zoom in)
- `Numpad -` - Increase FoV (zoom out)
- `Numpad 8`, `Numpad 5`, `Numpad 4`, `Numpad 6` - Move camera forward, back, left, right
- `~` - Toggle dev GUI 

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
* If you have at least one controller connected, it will override keyboard input and steering, throttle and brake on the keyboard will not work.
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
#### Build 1068 (version 7)
- Reduced body collision friction from 1x to 0.7x to reduce chance of car flying when hitting small bumps and slowing down when bottoming out. Can now be set in config.
- Automatic shader reloading on window refocus is no longer enabled by default (can be turned on in config: `hotReloadShaders = 1`)
- Ambient light color, fog color and shadow color are now exposed in config
- Engine can now load meshes with 32bit indices (instead of previously only 16bit), which removes the mesh triangle limit
- Basic setup for splat (mixed) materials, but they are a bit broken so not in use yet
#### Build 1068 (asset changes only) (version 8)
- Monty track update:
  - Patched all holes visible from the stage
  - Added 2 new retaining walls in the serpentine section to make it more clear where is the edge
  - Flattened sides of the big tunnel entry a bit
  - Fixed lighting of birches at the last hairpin
  - Grounded some trees that were floating
- Shader cleanup (no visible differences)

#### Build 1258 (version 9)
- Analogue axis support for gamepads! Setup axes in bindings in config. By default uses Xbox controller setup. To be done:
  - No gamepad button support yet. 
  - Deadzone setting does nothing yet.
- Physics changes: 
  - Changed steering behavior to support gamepad. Steering is more reactive now. (will be tweaked later)
  - Default car now has lower and stiffer tarmac-like suspension
  - Suspension force now dependent on surface normal
- Default car now has 18 inch tarmac wheels (graphics change only)
- Added headlights. Toggle on `L` by default.
- Blended tansparency support. Spec17 car now has windows.
- Splat materials support. But none in use yet. Hardcorded surface physics for now. Documentation for how to use it coming soon.
- Changed the smooth follow camera (3rd person). It now has split horizontal and distance constraints, so the camera won't get so far from the car when at high speed and will still be "cool" in the powerslides
- Added `-cfg "your_path.ini"` to CL args to support loading different configs

#### Build 1276 (version 10)
- Joypad button support. Mapped to 500+ keys in bindings config
- Added outputKeyEvents parameter. Set it to 1 in config to ouput keys that you press/release in the console.
- Analogue axis deadzone settings now work
- Exposed "smooth follow" camera parameters in config
- Car's rear lights are now lit up when headlights are on
- Removed annoying bright white pixels from rear lights on default car
- Physics: Reduced steering stiffness to be closer to the old steering physics
- Exposed gravity parameter in config

#### Build 1591 (version 11/12)
- Controller presence no longer disables keyboard input. Axes inputs of both now add up and work simultaneously
- Physics changes:
  - Drive friction is now applied to the drivetrain not to the whole car
  - Brakes now exclusively apply friction to the drivetrain (previously there was an extra legacy braking force). Expect much weaker brakes now.
- Resizing the window in windowed mode now correctly rebuilds the framebuffer (game doesn't get squashed)
- Headlights are now conical spotlights (instead of spherical blobs), they are much more powerful and add light to environment in a more correct way
- Brakes now cast light onto environment when headlights are off
- Fixed crash when splat materials are used but no splatsrc object exists
- Added axes input HUD
- Moved timing window to the upper left corner
- Particles!
  - Grass now emits particles
  - Particles are affected by light and receive shadows
- Graphical wheel movement height is now clamped so it doesn't penetrate fenders. Can be set with `wheelMaxHeight` in config. Can be debugged and live edited in dev GUI (~) and visible in gizmos, toggle on F12 (as thin yellow line)
- No timing and countdown happens on free roam tracks (where checkpoints are not found)
- Tracks can now embed starting positions in gltf/glb files with a dummy object containing `strt` in their name. Start position can still be overriden in config if `overrideStartPosition = 1` is set
- Improved surfaces, added more parameters: (hardcorded for now)
  - `friction` - applies braking force to the wheels
  - `tireCMult` - changes Pacejka Parameter C
  - `steerMult` - multiplies steering forces
- Added surfaces: gravel, snow, ice, deep_snow (not used in game yet)

#### Build 1679 (version 13)
- Fixed a memory leak caused by the new input GUI
- Added to config:
  - Gear ratios are now finally exposed! (`gearRatios` and `gearFinalRatio`)
  - `maximized` - maximizes window on start
  - `fov` - field of view
  - `torque_table` now in one line
  - `lightsIntensity` and positions of lights (`frontLightsPosition` and `rearLightsPosition`)
- Debug menu bar in dev GUI (~) for quicker saving of parameters
- Dev GUI windows can now be toggled individually

#### Build 1786 (version 14)
- Fixed major issue with collision response that caused the car to jump on polygon edges
  - ^ Except for the wooden guardrail in Monty, the car will still stick into that one
- Increased collision friction again to reduce wallriding exploit, since previously mentioned collision issue was fixed
- Added message queue. It only shows info about camera switching and switching between auto shifting modes for now
- Split timings now show only temporarily after passing through checkpoints
- Config now supports `#` as comment delimiter. Note that end of line comments are not supported (and never were).
- Reorganized config `[car]` section to be easier to read
- Automatic gearbox no longer gets stuck in high gear when braking to a stop
- Added credits.txt

#### Build 1951 (version 15)
- Added moveable (rigid body) objects:
  - Stage signs, stage arrows, chairs and tables are now destructible in Monty
- Fixed reverse. No longer turns into the same direction on both left and right.
- Slightly decreased turning when skidding on the spot
- Key events now output to ingame message queue when `outputKeyEvents` is enabled
- Pressing `G` or `T` no longer sends test messages.
- Wooden guardrails in Monty now have smoother collisions and will not get the cars stuck
- Modding: `colproxy` named material can now be set to a submesh for when a collision-only part of an object is needed
- Shadow rays now pass through checkpoint triggers. A workaround where triggers had to be in unshadowed areas to not brighten the car is no longer needed.
- Modding: Added `mvbl` tag for moveables (rigid bodies). For now only a mesh-bounding box collider is used. Objects containing `stage_sign` are also moveable
  - `m=NUMBER` can be added to the same object to set the mass of a moveable object. For example `m=100` would make it weigh 100kg
- Optimized box colliders by reusing those with the same dimensions

#### Build 1999 (version 16)
- Engine optimizations
- Monty:
  - One chair was unmoveable, now it is
  - Cars are now moveable as well
  - Fixed holes in town and after the first tunnel
- Renamed `monty_track` to `monty`

#### Build 2061 (version 17)
- Game now automatically makes sure it runs on the dedicated GPU on PCs with multiple GPUs. Setting manual override in Windows settings is no longer required.
- Automatic gearbox no longer drops into lower gears suddenly when sliding sideways. It now uses total velocity, instead of forward velocity to calculate when to switch gears.
- Added "slew mode" as dev utility, toggled by pressing "Y". In slew mode you can fly around the map when in orbit camera mode. Hold shift and scroll to change speed. Times will be invalidated if you activate it during a timed run.
- More engine optimizations and cleanup
- Added GLEW failed to initialize check

#### Build 2436 (version 18, demo version 1)
- Added replay system:
  - Replay is recorded automatically every run (but can be turned off in config with `autoRecordReplay` if it causes problems on low memory/spec machines)
  - Replay can be played back from the new in-game menu, by hitting Esc -> R
- Added in-game menu. Esc now opens the menu and pauses the game.
  - Restarting the run is now Esc -> Space key combo, instead of just Esc
  - You can finally quit the game PROPERLY, through the menu (Esc -> Enter) instead of just alt+F4 (although they work in the same way).
- Timing is now calculated in physics update, making it more correct. Previously, times were longer if physics update couldn't keep up, potentially on slower machines.
- Cars no longer slide off slopes when stopped
- Fixed a huge drivetrain instability issue that caused all parameters to fluctuate when very small
- Game can now also be paused by pressing P, you can then change the cameras as you wish and use it as a photo mode (don't forget Tab toggles the HUD by default, so you can take some screenshots)
- Added helicopter camera
- Renamed Smooth Follow camera to Chase
- Mouse now automatically hides when idle, instead of having to press ctrl to toggle
- Config changes: 
  - Gravity is now part of `[enviro]` and starting position override parameters are now part of `[track]`. Previously both were in `[car]`
  - Added particle color parameters to config (temporary, until separate track splat system configuration)
  - You can now offset the orbit camera pivot for each car, by setting `orbitCamOffset=`
  - Exposed rollbar stiffness parameter in car config
- Fixed orbit camera flipping 180 degrees
- For modders: `wheel` material is no longer necessary as part of the car model, texture does not need to be named `spec17_body` any more (see [modding](modding.md/#car) updates)
- This is the first version that splits paid and demo versions. The only difference between the two versions right now is that in the paid version:
  - The car configs is now split from the main config, allowing for easy car modding. Now you can load different cars with different parameters by switching the `car =` parameter in main config. See [modding](modding.md/#car) for changes (in demo version the config is still combined and model hardcoded to `spec17`)
  - Folder structure has changed, cars are now in `res/cars/` subfolder
- Added smoothing and noise to freelook camera
- Rearranged car parameters for easier modification
- Wheel base and track are now modifiable in GUI
- Removed legacy HPM and PLY loading from engine

#### Build 2437 (version 19, demo version 2)
- Game now starts in fullscreen again by default (broken in previous version)
- Exposed freelook camera parameters in config
- Mouse hiding logic is now using unscaled deltatime

#### Build 2465 (version 20, demo version 3)
- Fixed bug where times would not be registered when playing a replay after finishing the stage
- Tracks are now called "maps"
- In paid version:
  - maps are now also split from main config and load from `.map.ini` files and have been moved to `res/maps/` folder (similar to cars)
  - Removed demo-only params from config
- Added `startWithHeadlightsOn=` parameter for night maps
- Rearranged config parameters:
  - `[enviro]` is now `[colors]`
  - `[scene]` and `[track]` params merged under `[map]`
  - particle and start params now have their own segments
- Split timings window no longer pops out when HUD is hidden
- Changed "Loading.." screen to show Shakedown Rally