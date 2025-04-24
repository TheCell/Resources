---
tags:
  - vfx
  - unity
up: "[[VFX]]"
---
[https://github.com/dsoft20/psx_retroshader](https://github.com/dsoft20/psx_retroshader)

Shaders collection for Unity that "emulates" the rendering style of ps1

WebPlayer (it's an old build): [https://dl.dropboxusercontent.com/u/1050404/psx/psx.html](https://dl.dropboxusercontent.com/u/1050404/psx/psx.html)

You can see it in action here: [https://www.youtube.com/watch?v=MxcLA--2v-Y](https://www.youtube.com/watch?v=MxcLA--2v-Y)

![[687474703a2f2f692e696d6775722e636f6d2f4d5337736a74332e706e67.png]]
![[687474703a2f2f692e696d6775722e636f6d2f6d7934333851582e676966.gif]]

# Content & usage psx_retroshader includes 4 shaders, plus a simple posterize image effect (cPrecision.cs):

- unlit
- vertex lit
- trasparent unlit
- trasparent vertex lit
- Reflective shaders (Add & Mult variants)

Vertex lit shaders now supports spotlights too!

Example of the posterize shader:
![[687474703a2f2f692e696d6775722e636f6d2f484535667868542e706e67.png]]

All shaders supports Fog, polygon cut-out & distortion amount.

- Fog color & distance is driven by Unity fog settings (remember to set as linear fog).
- Polygon cutout is driven by tha alpha channel of Fog Color, it works by cutting every polygon that are greater in distance than fogstart+fogcolor.alpha (fog color is in range 0-1 but is multiplied in the shader by 255)
- Distortion amount is driven by the alpha channel of unity's ambient color, you can adjust it as you please.

# Warning
Like the original ps1 this shader use affine texture mapping, so if you apply a texture on a large quad you'll see it very distored. To avoid excessive distortion you have to add triangless to the mesh.

Example:
![[687474703a2f2f692e696d6775722e636f6d2f7a43325431754a2e706e67.png]]

As you can see the effect is better when the mesh is subdivided (bottom left mesh) instead of when the mesh have a low poly count (top right mesh)