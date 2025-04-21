---
tags:
  - unity
  - addon
created: 22.04.2025
up: "[[Code]]"
---
[https://github.com/KristinLague/Mesh-Cutting](https://github.com/KristinLague/Mesh-Cutting)

Unity Version - 2021.2.7

**How does it work?**

The Mouse Input is used to draw a cut line onto the screen. I then use this lines normal to spawn an endless plane that goes right through the meshes that are being cut. The plane is used to separate the original mesh and its submeshes into a left and a right mesh. I evaluate which vertices of the original mesh belong to which new mesh by utilizing the Plane.GetSide() function and assign them accordingly. Lastly I am filling the cut sides of the new meshes by adding new vertices in code. Lastly I am adding a little bit of force to one of the new meshes to visualise the cut.