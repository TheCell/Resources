---
tags:
  - godot
  - addon
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/cloudofoz/godot-deformablemesh](https://github.com/cloudofoz/godot-deformablemesh)

## godot-deformablemesh
**This addon allows to deform 3D meshes using a stack of customizable deformers at run-time**
![[dm_screen_v04_1.gif]]
![[dm_screen_v03_1.gif]]
![[dm_example_scene_scr.jpg]]

## Main Features
Use the default deformers:
- `SphericalDeformer` 
- `StandardDeformer`  (Bend, Twist, and Taper)
- `DragDeformer` (In **Rest Pose Mode**, position the deformer, toggle it off, and deform the mesh by moving the node.)

Or **easily create your own** by extending the base class and overriding just a couple of methods in `dm_deformer.gd`.

## Example Project
1. [**You can download here**](https://github.com/cloudofoz/godot-deformablemesh/blob/main/media/dm_example_scene.zip) an example project that shows the basic functionalities of `DeformableMesh`.
2. Unzip the file
3. Import the project with Godot Engine 4+
4. Open the scene `dm_example_scene_v030.tscn` (if it's not already opened)

You can now try tweaking the deformer parameters. Some effects are also controlled by the positions and the rotations of the deformer nodes.

`DeformableMesh` can apply multiple deformers like in a stack, so the order is important to achieve the correct effect. You need also to specify the correct deformation axis (for some effects like bending, but it's not important with spherical deformers).

## Known Limitations
- This addon is designed with simplicity and versatility as primary goals, making it well-suited for simple, standard use cases. However, it is not optimized for specialized use cases, such as higher-density meshes (and, in some cases, multiple surfaces, which may also impact performance) in performance-critical applications. Users are encouraged to thoroughly test the addon to ensure it meets their specific requirements.
    
- While other deformers support deforming multiple meshes at once, a `DragDeformer` can only be tied to a single mesh at a time.
    

## Credits
I wish to thank the community for any contribution or feedback. Special thanks to:

- **Kevin Loustau**, for sparking the idea behind the `DragDeformer` feature.