Correct as of build 2517 in paid version

# Shakedown Modding

~~My goal for the game is to make modding as easy as possible, BUT it is not yet officially mod "friendly". It doesn't have in-game track or car picking, nor good error messages if you do something wrong. Nothing stops you from changing the car parameters or asset files right now as they're "out in the open" but as I am still changing everything I advise you to wait a bit as I will make your life easier.~~

Car and map modding is now officially supported (in paid version only)!

I am still working on the modding system and workflow, making it as easy and versatile as possible for anyone to make their own. That means that the modding system is not stable, but that shouldn't deter you from making your mods right now, because I will provide detailed modding upgrade notes for every new change as part of the [changelog](README.md#changelog).

## Modeling

The game loads gltf/glb files with embedded textures.

For the object to be valid, it needs to have a material and the material needs to have a texture assigned as a color map.

### glb vs gltf

Both are supported just fine, but:
* Glb files are binary files, they are smaller and load faster, it's recommended to use them for release.
* gltf are easier to debug as they are text-based json-style files so can be opened in any text editor for inspection.

## Car

(Since build 2436 in paid version):

There are 2 parts of a car, the `.car.ini` config file, which defines the parameters, and the `.gltf`/`.glb` model file.

#### Car Config

For now, only one car is loaded at a time in game. Which car loads is defined in `config.ini` -> `[car]` -> `car =` parameter. To load different cars you need to change this parameter to a certain `.car.ini` path and restart the game.

The `.car.ini` file contains all the parameters for the car. It is recommended that you put it in a unique folder in `res/cars`, such as `res/cars/yourcar/yourcar.car.ini`. You can copy one of the existing cars to get started, and just modify parameters to your liking. Alternatively, you can edit most of the parameters live in game in the dev gui (by pressing `~`), in `Shakedown` window -> `Car Parameters` segment. After you've finished changing the values, press the `File > Save Config` in the toolbar and it will be saved into your currently loaded `.car.ini`.

#### Car Model

The `model =` parameter in the `.car.ini` points into the path of the model file (relative to the `.car.ini`), such as `model = yourcar.gltf`.

The model must contain objects whose names end on:
* `_body` - the rendered body
* `_wheel` - used for all wheels, a single wheel model will be copied to all sides
* `_hull` - used for convex mesh collider (not rendered)

optionally, it can also have:
* `_headlights` - glow as headlights
* `_reverselights` - glow when in reverse
* `_brakelights` - glow as brakes

If you have any multiple objects with the same ending, the game will pick the last one. Any objects that do not have those endings will simply be hidden and not used.

You can also open `res/spec17/spec17.gltf` in blender to check out how it's set up.

All of these objects must have a single material each. Otherwise the game will just pick one submesh (mesh with one material slot) and keep the others in the middle of track.

The pivot of body object must be exactly in the center of wheelbase and track (in the middle of all wheels). This is because the game uses just a single value for wheelBase and wheelTrack. It will place wheels at 0.5 * wheelBase back for rear wheels and same for front wheels.

The pivot of `mycar_body` is also the center of mass. So, the pivot height should be set to a realistic centre of mass height, and MUST be inside the convex mesh. If the pivot is outside, the car might render black as the pivot is also used for shadow raycasts and they might be hitting the body itself.

Car and wheels should be oriented in blender default reference system (Y should be pointing backwards and Z upwards) and have rotation at zero. In case rotations are not zeroed out, they must be applied (Ctrl+A -> Rotation).

## Map

(added in Build 2517)

Similar to cars, maps also consist of a `.map.ini` config and glb/gltf model files, located in `/res/maps`. Which map is loaded is defined in the `config.ini` -> `[map]` -> `map =`

The objects of a map model MUST have at least one layer of vertex color, which is used for baked ambient light (shadow + GI). The vertex color is multiplied with texture. If an object does not have this layer it will be rendered black.

Standard (non-splat) materials support only 1 texture and 1 uv layer per material.

To summarize, valid vertex data is:
- position - vec3 float
- uv - vec2 float
- color0 - vec3 unorm short or float
- color1 - vec3 unorm short or float

Any other attributes (like normals) will be discarded.

Scale of all objects that you wish to have colliders on MUST have unit scale (meaning the scale must be 1, 1, 1), otherwise collisions will not work correctly. You can apply scale in blender, by selecting objects then pressing ctrl+A -> Scale.

#### Surfaces

Note: I use the word "surface" to mean a physical surface - how a surface influences the car and game, e.g. ice makes the car slide, tarmac is sealed and very grippy, snow is loose and emits white particles.. Not to be confused with "material" which is graphical property of a mesh, although materials do map to surfaces, but they're not the same thing.

There are currently 3 default surfaces: tarmac, gravel, and dirt/grass. Any object primitives (submeshes) that have materials with `tarmac` or `road` (and are not splat) in their names will use tarmac surface, any that has `gravel` will use gravel, and everything else will use dirt/grass.

Splat materials have a separate surface system that blends between surfaces using vertex colors. See [Splat Material](#Splat-Material).

#### Object and Material Tags

There are some special tags you can insert into your object names to assign certain behavior to them (they are case sensitive):
- `colbox` will create a box collider with the size of the object's bounding box
- `nocol` or `tree` will not make a collider. That is, they will be graphical only and you can drive through these objects.
- objects starting with `chk_` and ending in `.001`, `.002`, `.003` etc. are checkpoints (splits). They must be in order with no interruptions. They will use a box collider (as if they had a `colbox` tag), so to be valid they should not be 0-thin in either dimension. The finish line is the last checkpoint.
- `mvbl` (or starting with `stage_sign`) will make the object into a "moveable" (rigid bodies the player can topple). Right now only a bounding box collider is supported, and will be automatically added no matter if you have a `colbox` tag or not
  - `m=NUMBER` can be added to the same object, where `NUMBER` is weight in kg. For example `bollard_mvbl_m=100` will make a 100 kg bollard. (note: parsing[^1])
- `strt` defines the starting location. (notes: override[^2], placement[^3])  The last `strt` object found will be used.
- `splatsrc:` see [Splat Material](#Splat-Material).

If none of these tags are found, an object will be static and will have a mesh collider.

There is a few material tags as well:
- `colproxy` material will be collision only but will not render. This is useful when for example you have an object made with geometry nodes in blender and you want it to have a collider that is different than the graphical object.
- `nocol` or `tree`, same as object tags ^
- `sft` makes the object's collider "soft": it will have a collider that can be partially penetrated with a weaker response spring force than normal. Used for example for soft snow cover.
- `splat:` see [Splat Material](#Splat-Material).

#### Special objects
- `suncube` is used for pointing the sun towards its -Z axis (opposite from blue arrow blender gizmo) that car uses for shadowing itself. If none is found, the game will point the sun top-down.

#### Tips
* LOOK AT THE CONSOLE! When the game starts, it loads the track and car files. Any import error will output both in the console and in the `log.txt` file. You can alt-tab to the console during gameplay. It's much easier to spot errors in the console because it will shine bright red. But in case of a crash check `log.txt` first (before complaining :P)
* Prefer using box colldiers for small objects that don't need precise collision. In Monty stage those are for example chairs and tables in the town, as mesh colliders would be too dense and unnecessarily slow down the game.
* Exporting .png images with alpha in Photoshop will by default treat alpha as transparency. To export alpha properly as a separate channel without ruining RGB channels, use the [SuperPNG extension](http://www.fnordware.com/superpng/). My personal preference for textures is png, but format itself ultimately doesn't matter if you export gltf/glb with jpeg setting on.
* Note that gltf/glb recognizes mesh instances, if multiple objects use the same mesh, it will reuse it. It can greatly reduce the size of the track and loading times especially if there are many reused objects, like trees. Although the problem with instancing is that it will not have unique vertex color baked lighting applied. To instance objects in blender (reuse the same mesh) use alt + D when copying instead of ctrl + D

### Splat Material

A special splat shader which uses the second vertex color layer can be used for [splatting](https://en.wikipedia.org/wiki/Texture_splatting), i.e. blending between 4 different textures. Materials that use splat must contain the `splat:SPLATNAME` tag where `SPLATNAME` is your custom identifier.

![Object with a splat material and its vertex color](media/splat_material.png)
<sup>Object with a splat:sweden splat material and its vertex color</sup>

Since Blender's gltf exporter cannot out-of-the-box export references to 4 textures in a single material, a proxy object that will "tell" the game which textures to use for which channel for a particular splat material must exist. This source object needs to be named in the form of `splatsrc:SPLATNAME`, where `SPLATNAME` is the same identifier you used in the splat material.

![The splat source object with 4 materials for each channel](media/splat_source.png)
<sup>The corresponding splatsrc:sweden object with 4 materials for each channel</sup>

The splat source object now should have up to 4 materials with textures assigned to the albedo. Each of these materials must contain channel tags:
- `ch:0` is the base, or pure black vertex color will render this texture,
- `ch:r` added by the red vertex color,
- `ch:g` added to green vertex color, and
- `ch:b` added to blue vertex color.

Each of the added textures (R, G & B) provided must also have an alpha. The alpha is used as a heightmap. It is VERY IMPORTANT to connect the alpha output of the texture in the shader to alpha of the BSDF, otherwise gltf won't export the alpha channel and you won't see any blending.

Using a `SPLATNAME` (custom identifier) allows you to have multiple different splat materials, for example one for the road, one for terrain.

Surfaces for splat materials are blended by vertex color. All properties are linearily interpolated. Right now the surface properties are hardcorded.

[^1]: Note that number ends with a first non-number character, which also includes `.`. So if the number is at the end of the objects name, copying the object in blender makes it a `mvbl_m=100.001`, meaning the mass will be `100.001`, so prefer not putting the tag at the end of the name.
[^2]: You can also override the starting position without changing the map model in config by setting `overrideStartingPosition = 1` and providing position and rotation. But the override is mostly used for testing and it is recommended to use the model-embedded `strt` object instead.
[^3]: You should put the `strt` object a little above the track because the car is centered around its center of mass. Putting the `strt` object too high will however make it "drop" at start.