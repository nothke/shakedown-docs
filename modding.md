Correct as of build 879

# Shakedown Modding

My goal for the game is to make modding as easy as possible, BUT it is not yet officially mod "friendly". It doesn't even have track or car picking, nor good error messages if you do something wrong. Nothing stops you from changing the car parameters or asset files right now as they're "out in the open" but as I am still changing everything I advise you to wait a bit as I will make your life easier.

## Modeling

The game loads gltf/glb files with embedded textures. Only gltf exporter in blender version 3.3 is supported. There seems to be an issue with newer versions of blender, the vertex colors are messed up.

For the object to be valid, it needs to have a material and the material needs to have a texture assigned as a color map.

### glb vs gltf

Both are supported just fine, but:
* Glb files are binary files, they are smaller and load faster, it's recommended to use them for release.
* gltf are easier to debug as they are text-based json-style files so can be opened in any text editor for inspection.

### Car

Right now, only one car is loaded and hardcoded to `spec17.gltf`, open it to see  the objects within. Custom objects must retain the same names as they are loaded by name:

* `spec17_body` - the rendered body
* `spec17_hull` - used for convex mesh collider
* `spec17_wheel` - used for all wheels
* `spec17_reverselights`, `spec17_brakelights` - glow as brakes/reverse lights

All of these objects must have a single material each. Otherwise the game will just pick one submesh (mesh with one material slot) and keep the others in the middle of track.

The pivot of body object must be exactly in the center of wheelbase and track (in the middle of all wheels). This is because the game uses just a single value for wheelBase and wheelTrack. It will place wheels at 0.5 * wheelBase back for rear wheels and same for front wheels.

Another thing is that the pivot should be inside the convex mesh

### Stage

Object MUST have a single layer of vertex color, which is used for baked ambient light (shadow + GI). The vertex color is multiplied with texture. If an object does not have a layer it will be rendered black.

Only 1 texture per material and 1 uv layer is supported.

To summarize, valid vertex data is:
- position - vec3 float
- color - vec4 unorm short
- uv - vec2 float

Any other attributes (like normals) will be discarded.

All object's scale MUST have unit scale (meaning the scale must be 1, 1, 1), otherwise mesh colliders will not work correctly. You can apply scale in blender, by selecting objects then pressing ctrl+A -> Scale.

#### Surfaces

There are currently 2 physical surfaces: tarmac and dirt. Any object primitives (submeshes) that have materials with `tarmac` or `road` in their names will use tarmac surface, and everything else will use dirt.

#### Object tags

There are some special tags you can put in your object names (they are case sensitive):
- `colbox` will create a box collider with the size of the object's bounding box
- `nocol` or `tree` will not make a collider
- objects starting with `chk_` and ending in `.001`, `.002`, `.003` etc. are checkpoints (splits). They must be in order with no interruptions. They will use a box collider (as if they had a `colbox` tag), so to be valid they should not be 0-thin in either dimension. The finish line is the last checkpoint.

If none of these tags are found, an object will be static and will have a mesh collider.

#### Special objects
- `suncube` is used for pointing the sun towards its -Z axis (opposite from blue arrow blender gizmo) that car uses for shadowing itself. If none is found, the game will point the sun top-down.

#### Tips
* Prefer using box colldiers for small objects that don't need precise collision. In Monty stage those are for example chairs and tables, as mesh colliders would be too dense and unnecessarily slow down the game.
