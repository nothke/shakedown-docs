Some notes on using the bullet library.

## Making triggers

TODO

## Getting triangle indices from raycasts

TODO

## Selective raycasts

TODO

## Fixing jumpy collisions

By default, when a rigidbody slides along faces of a triangle mesh collider, it will produce incorrect collision as it hits each new triangle (crosses "inner edges"). This produces weird jumps, jitters, stutters, whatever you want to call them.

There is actually a solution within bullet itself to solve this problem: `btInnerEdgeUtility`, but is very poorly documented on how to use it. Not only that, but the demo has for some reason been removed out of new versions of bullet , even if the solution still works! It took me a lot of digging to find out and to understand how to use it. 

So, to help you not spend so much time digging for solutions here's how to use it:

First:
```
#include "BulletCollision/CollisionDispatch/btInternalEdgeUtility.h"
```

For each (static) triangle mesh, you need to generate the internal edge info. This info will be attached to the mesh. Docs sometimes state that this info will be assigned to a "user pointer", but it actually has no relation to the `userPointer`, it will use a special info pointer.
```
auto* infoMap = new btTriangleInfoMap;
btGenerateInternalEdgeInfo(triMeshCollider, infoMap);
```

Now to process the collisions, you need to make your own contact callback and call `btAdjustInternalEdgeContacts` within it. Here is a minimal example:

```
bool MyContactAddedCallback(
	btManifoldPoint& cp,
	const btCollisionObjectWrapper* colObj0Wrap, int partId0, int index0,
	const btCollisionObjectWrapper* colObj1Wrap, int partId1, int index1)
{
	btAdjustInternalEdgeContacts(cp, colObj1Wrap, colObj0Wrap, partId1, index1);

	return true;
}
```

Then assign that callback to `gContactAddedCallback` (yes, it's actually a global variable!):
```
gContactAddedCallback = MyContactAddedCallback;
```
Now for your rigidbody, you need to enable the custom material callback flag so that gContactAddedCallback would be called:
```
body->setCollisionFlags(body->getCollisionFlags() | btCollisionObject::CF_CUSTOM_MATERIAL_CALLBACK);
```

That's it, now your contacts should be "fixed".