---
tags:
  - vfx
  - unity
up: "[[VFX]]"
---

[https://github.com/keijiro/KinoHatch](https://github.com/keijiro/KinoHatch)

![[68747470733a2f2f692e696d6775722e636f6d2f694d4d585062532e706e67.png]]

**KinoHatch** is a post processing effect that converts an image into monochrome with hatching.

## System requirements

[](https://github.com/keijiro/KinoHatch#system-requirements)

- Unity 2019.3
- HDRP 7.3

## How To Install

[](https://github.com/keijiro/KinoHatch#how-to-install)

This package uses the [scoped registry](https://docs.unity3d.com/Manual/upm-scoped.html) feature to resolve package dependencies. Please add the following sections to the manifest file (Packages/manifest.json).

To the `scopedRegistries` section:

```json
{
  "name": "Keijiro",
  "url": "https://registry.npmjs.com",
  "scopes": [ "jp.keijiro" ]
}
```

To the `dependencies` section:

```json
"jp.keijiro.kino.post-processing.hatch": "1.0.0"
```

After changes, the manifest file should look like below:

```json
{
  "scopedRegistries": [
    {
      "name": "Keijiro",
      "url": "https://registry.npmjs.com",
      "scopes": [ "jp.keijiro" ]
    }
  ],
  "dependencies": {
    "jp.keijiro.kino.post-processing.hatch": "1.0.0",
    ...
```