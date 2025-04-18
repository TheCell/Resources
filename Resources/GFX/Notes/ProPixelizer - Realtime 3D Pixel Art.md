---
tags:
  - vfx
  - unity
up: "[[VFX]]"
---
[https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/propixelizer-realtime-3d-pixel-art-177877](https://assetstore.unity.com/packages/vfx/shaders/fullscreen-camera-effects/propixelizer-realtime-3d-pixel-art-177877)
![[ProPixelizer - Realtime 3D Pixel Art.webp]]

_Render 3D objects to look like 2D sprite art, in real time._

[Full feature list & playable demos](https://sites.google.com/view/propixelizer/home) | [Showcase](http://sites.google.com/view/propixelizer/showcase) | [User Guide](https://sites.google.com/view/propixelizer/user-guide)

**Features:** 
- ✔️ **Incredibly versatile**: Able to replicate a huge number of different pixel art styles. Everything is included in ProPixelizer, not spread across multiple assets.
- ✔️ **Per-object pixelization**: mix scenes of pixelated and unpixelated objects. Individual objects may be pixelated in 'macropixels' of up to 5x5 screen pixels in size.
- ✔️ **Per-object outlines**: Outlines can be block color or tinted, and controlled separately for each object. Supports both silhouette and normal-based outlines.
- ✔️ **Creepless sub-pixel motion** when using orthographic projection. Not just for the camera, but also for object motion!
- ✔️ **Lighting and shadows**: including shadows cast between pixelated objects and unpixelated objects.
- ✔️Compatible with **ShaderGraph** - just use the provided nodes in your materials (example ShaderGraph asset included).
- ✔️**Per-object color grading and dither patterns** - achieve a retro feel by using reduced color palettes. A selection of dither patterns (or no dither) are supported. Editor tools to create dither patterns and palettes.
- ✔️**Stepped animation tool** - capture a flip-book feel using the included tool for creating stepped animation clips from your existing animations.
- ✔️Compatible with other **post processing** effects, such as depth of field, bloom, vignette.
- ✔️**WebGL** support - see the [demo on itch.io.](https://elliotb256.itch.io/propixelizer)
- ✔️**Unlike other pixelization effects**, ProPixelizer keeps the screen rendered at **full resolution**. This allows pixelated objects to move at screen resolution, without creep. It also allows seamless blending of pixelated and unpixelated objects, and use of high resolution post processing such as bloom and/or depth-of-field.
- ✔️Works well with **low-poly 3D** models.

**Requirements:**
- For **Universal Render Pipeline** only**.**
- The latest version of ProPixelizer has been tested with Unity 2020.3.48f1 (URP 10.10.1), Unity 2021.3.33f1 (URP 12.1.13), Unity 2022.3.16f1 (URP 14.0.9) and with Unity 2023.2.3f1 (URP 16.0.4).
- Performs a postprocessing effect to pixelate objects, which is implemented through an included Render Feature.
- Pixelated objects must be opaque, although alpha cutout is supported, and dithered fading.
- The post process samples the screen buffer in a 5x5 pattern. Performance may be slow on low end hardware.

_Please note - models, textures and other assets shown in the screenshots are not included. This package only contains the pixelization shaders and materials._