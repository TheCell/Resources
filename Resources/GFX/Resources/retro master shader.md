---
tags:
  - gfx
  - vfx
up: "[[VFX]]"
---
[https://andicraft.itch.io/retro-master-shader](https://andicraft.itch.io/retro-master-shader)
![[retro master shader.png]]

- Blend Modes:
    - Opaque
    - Alpha Cutoff
    - Transparent (Alpha Blend)
        - With refraction!
- PSX-style vertex snapping and texture mapping
- Texture Filtering:
    - Linear
    - Nearest
    - N64-style
- 3 lighting modes:
    - Per-Pixel
    - Per-Texel
        - Snaps lighting to texels for a crisp pixel art look
        - Supports baked lights too!
    - Per-Vertex
        - More powerful than regular vertex lights!
        - Real-time vertex shadows!
        - Specular highlights!
        - Vertex-based lightmaps!
- SSAO Support
    - In Per-Texel and Per-Pixel modes
- 4 different specular modes
    - Standard (URP default)
    - Phong
    - Blinn-Phong
    - Disabled
- Half-Lambert diffuse shading
- Toggleable reflections
- 2 Vertex Color modes
    - Multiply Vertex Color
    - Vertex Color as Emissive
- Dithered lighting
    - Diffuse, Specular, and Ambient can be toggled separately
    - Double-size dither for that low res feel
- Tint masking
    - Color your object selectively using a texture mask
- Toon ramp support
    - Separate ramps for specular and diffuse lighting
    - Works in all three lighting modes
    - Generate ramps directly in the editor with [Shader Graph Markdown](https://github.com/needle-tools/shadergraph-markdown)!
