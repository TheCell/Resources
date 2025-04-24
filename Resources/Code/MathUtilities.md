---
tags:
  - unity
  - utility
created: 24.04.2025
up: "[[Code]]"
---
[https://github.com/zalo/MathUtilities](https://github.com/zalo/MathUtilities)

A grab bag of some of the neat math and physics tricks that I've amassed over the last few years, implemented in Unity's C#. You're free to use the code here however you like.

## [Generalized Mesh Deformation](https://github.com/zalo/MathUtilities/tree/master/Assets/Deform)
![[68747470733a2f2f692e696d6775722e636f6d2f387162387335742e676966.gif]]

An extremely fast, general mesh-deformation algorithm with arbitrary point-placement and configurable rigidity. Has the desirable property that it acts like a rigid Kabsch when the "weight" (ductility) is set to 0, but smoothly blends in deformation as the weight is increased.

The implementation is similar to Linear Blend Skinning, but the skinning weights are automatically calculated with [Inverse Distance Weighting](https://en.wikipedia.org/wiki/Inverse_distance_weighting) (in cartesian _or_ surface space), and the "bone rotations" are calculated to optimally preserve the angular relationships between the control points.

## [Signed Distance Field Texture Rendering](https://github.com/zalo/MathUtilities/tree/master/Assets/Volume)
![[68747470733a2f2f692e696d6775722e636f6d2f6b374a3049376b2e676966.gif]]

A simple example for raymarching and painting/blitting-to volumetric distance field textures. Distance Fields are regions of space that store the distance to the nearest surface at each point in space. This property allows them to represent and render solid shapes of arbitrary geometry and topology.

This particular implementation also stores the normalized vector toward the nearest point (the normal or gradient of the field). This is useful for physics queries and lighting (without taking the numerical derivative).

## [Kabsch](https://github.com/zalo/MathUtilities/blob/master/Assets/Kabsch/Kabsch.cs)
![[687474703a2f2f692e696d6775722e636f6d2f327168526d744e2e676966.gif]]

Also known as Procrustes Analysis, this algorithm can take in an arbitrary set of point-pairs and find the globally optimal rigid translation and rotation to minimize the distance between those point pairs. Incredibly useful, and very cheap. Uses Matthias Muller's polar decomposition solver in place of SVD, as outlined here: [https://animation.rwth-aachen.de/media/papers/2016-MIG-StableRotation.pdf](https://animation.rwth-aachen.de/media/papers/2016-MIG-StableRotation.pdf)

Update: Added an example for averaging arbitrary numbers of quaternions; possibly more accurate than a normalized lerp (averaging the quaternion components in linear space and then normalizing).

## [Least Squares Line/Plane Fitting](https://github.com/zalo/MathUtilities/blob/master/Assets/LeastSquares/LeastSquaresFitting.cs#L24) [[Pic]](https://i.imgur.com/xTGKnx5)
![[68747470733a2f2f692e696d6775722e636f6d2f4e4274596949712e676966.gif]]

A generic helper utility for fitting lines and planes to point-sets in 3D. Uses a novel* matrix-less formulation to solve for the orthogonal lines and planes of best fit; algorithm is without singularities and is extensible to arbitrary dimensionality.

*(as far as I know; I have not seen anyone attempt orthogonal regression without using an SVD (which may be unnecessarily expensive for just the line/plane of best fit))

## [Stereographic/Fisheye Camera](https://github.com/zalo/MathUtilities/tree/master/Assets/StereographicCamera) [[Wiki]](https://en.wikipedia.org/wiki/Stereographic_projection)
![[687474703a2f2f692e696d6775722e636f6d2f4d4f36524c5a712e676966.gif]]

A prefab that concatenates and warps the images from four cameras into one 180 degree fisheye view, projected stereographically.

## [Verlet Softbody, Rigidbody, and Chain](https://github.com/zalo/MathUtilities/tree/master/Assets/Verlet)
![[687474703a2f2f692e696d6775722e636f6d2f79316a59417a772e676966.gif]]
![[687474703a2f2f692e696d6775722e636f6d2f786c41686b4c342e676966.gif]]

Numerous examples of using verlet integration ([a subset of Position Based Dynamics](http://matthias-mueller-fischer.ch/publications/posBasedDyn.pdf)) to simulate soft/rigid bodies, cloth, and particle linkage chains.

## [Kalman Filter](https://github.com/zalo/MathUtilities/blob/master/Assets/Kalman/KalmanFilter.cs) [[Wiki]](https://en.wikipedia.org/wiki/Kalman_filter#Details)
![[687474703a2f2f692e696d6775722e636f6d2f534c354a4a4d762e676966.gif]]

A textbook implementation of a Kalman filter (transcribed from wikipedia) Kalman filters are a form of bayesian filtering; they are capable of taking in information from multiple sources and "fusing" them into a signal that is cleaner/more accurate than any of the constituent signals. A properly tuned Kalman filter is the mathematically "optimal" technique for turning noisy data into clean data in real time (as long as the noise follows a gaussian distribution and the data varies linearly). In practice one must deal with biases and non-linearly varying quantities.

## [Constraints](https://github.com/zalo/MathUtilities/tree/master/Assets/Constraints)/[Inverse Kinematics](https://github.com/zalo/MathUtilities/blob/master/Assets/IK/CCDIK/CCDIKJoint.cs)
![[687474703a2f2f692e696d6775722e636f6d2f75796d4a66314c2e676966.gif]]
![[687474703a2f2f692e696d6775722e636f6d2f6f7635386851482e676966.gif]]

A set of constraint functions that can be used to build [an iterative inverse kinematics solver.](https://makeshifted.itch.io/dexter-arm-ik)

[Nice introductory Tutorial to the concept behind CCDIK and the Kinematic Jacobian](http://www.elysium-labs.com/robotics-corner/learn-robotics/introduction-to-robotics/kinematic-jacobian/)

### Inverse-Kinematics: [CCDIK Illustration](https://github.com/zalo/MathUtilities/tree/master/Assets/IK/Tutorials/CCDIK) vs [FABRIK Illustration](https://github.com/zalo/MathUtilities/tree/master/Assets/IK/Tutorials/FABRIK)
![[68747470733a2f2f692e696d6775722e636f6d2f7832416b5458322e676966.gif]]
![[68747470733a2f2f692e696d6775722e636f6d2f6b5077534855302e676966.gif]]

### [Robotic Configuration Space Visualization](https://github.com/zalo/MathUtilities/tree/master/Assets/IK/CCDIK/Configuration) [[Pic]](https://i.imgur.com/kHdEZto.png) and [Collision-Aware IK](https://github.com/zalo/MathUtilities/blob/master/Assets/IK/CCDIK/Collision/CCCDIK.cs) [[Gif]](https://i.imgur.com/2BGw3vt.gif)

[](https://github.com/zalo/MathUtilities#robotic-configuration-space-visualization-pic-and-collision-aware-ik-gif)

The "Configuration Space" can be visualized by graphing the penetration of a robot with it's environment (and itself) as a distance field, where each axis is the angle/configuration of an individual joint. By path finding through valid regions in this space, one is actually planning the motion of the robot from one configuration to another. The gradient of the configuration space can also be used for light depenetration of the robot from invalid configurations. However, because precomputing the configuration space is slow (and must be redone for objects in the environment), I developed a variant of CCDIK (CCCDIK :) ) which iteratively depenetrates itself from the environment by temporarily treating the contact point as a new end-effector.

## Other experiments:

[](https://github.com/zalo/MathUtilities#other-experiments)

### [Nelder-Mead (Amoeba) Numerical Optimizer](https://github.com/zalo/MathUtilities/blob/master/Assets/Amoeba/NelderMead.cs) [[Wiki]](https://en.wikipedia.org/wiki/Nelder-Mead_method)
![[68747470733a2f2f692e696d6775722e636f6d2f494273466f4d642e676966.gif]]

A general, n-dimensional implementation of Nelder and Mead's gradient-less numerical optimization method for minimizing cost functions. This is a popular optimization technique for problems with high-dimensionality and no gradient information. Included is an example of optimizing a 5-DoF IK system (far less efficient than CCDIK, but more flexible overall). Also contains a numerical gradient descent optimizer for comparison.

### [Linear Assignment](https://github.com/zalo/MathUtilities/tree/master/Assets/Assignment)
A port of Roy Jonker's famous solution to the [Linear Assignment Problem](https://en.wikipedia.org/wiki/Assignment_problem). Allows you to take two arbitrary lists of objects (with a cost to pair objects in each of them to each other), and to find the globally optimal pairing betweeing objects in these lists. Extremely handy.

See the source file for Commercial Licensing Details.

### [Spatial Hashing](https://github.com/zalo/MathUtilities/blob/master/Assets/SpatialHashing/SpatialHashing.cs)
A reimplementation of [Matthias Muller Fischer's O(1) Spatial Hashing system](https://www.youtube.com/watch?v=D2M8jTtKi44) for Particle-Particle collision lookups. Generally results in a >20x speedup over naive O(n^2) collision checking.

### [Linear Blend Skinning](https://github.com/zalo/MathUtilities/blob/master/Assets/Skinning/LinearBlendSkinning.cs)
A reference implementation that demonstrates how to apply bone motions to a model using the data contained within a skinned mesh renderer. As they say, there is more than one way to skin a mesh.

### [Per-Pixel Texture Reprojection](https://github.com/zalo/MathUtilities/tree/master/Assets/Projection)
Demonstrates how to set up a shader to project a texture onto scene geometry using a (disabled) camera as the projector frustum. Useful for [illusions](https://github.com/zalo/MathUtilities/tree/master/Assets/Projection/Illusion).

### [Quasirandom Point Generation](https://github.com/zalo/MathUtilities/blob/master/Assets/Quasirandom/QuasirandomSequence.cs#L16)
![[68747470733a2f2f692e696d6775722e636f6d2f6a6471345172622e676966.gif]]

Contains a class for generating a near-uniform quasirandom distribution of points for arbitrary dimensionality and point counts. Based off of the article here: [http://extremelearning.com.au/unreasonable-effectiveness-of-quasirandom-sequences/](http://extremelearning.com.au/unreasonable-effectiveness-of-quasirandom-sequences/)

### [Distance Field Particle Displacement](https://github.com/zalo/MathUtilities/blob/master/Assets/VectorField/ParticleDistanceField.cs#L10) [[Gif]](https://imgur.com/0kWoSfJ)
Shows an interesting technique for displacing particles to flow around distance fields (would work very well in UE4's Niagara).

### [Minkowski Difference Visualizer](https://github.com/zalo/MathUtilities/tree/master/Assets/Minkowski)
![[68747470733a2f2f692e696d6775722e636f6d2f5a6d415433536d2e676966 1.gif]]

Uses a compute shader to draw arbitrary 2D Minkowski "Differences" in real time. The Minkowski Sum (and its modification, the "Minkowski Difference") is a core operation in collision detection. This concept allows for a fully generalized way of determining whether any two objects are intersecting, and what the minimum translation is that separates them. The key is determining whether the origin (of the coordinate system used in the operation) is inside of the resulting Minkowski Difference shape. GJK is a collision detection technique that implements this check quickly for convex objects, with only a few samples of the implicit Minkowski Difference.

### [Bidirectional Raycasting](https://github.com/zalo/MathUtilities/tree/master/Assets/Raycast)
![[68747470733a2f2f692e696d6775722e636f6d2f755463595950462e676966.gif]]

Operates like a standard raycast, but returns both entry and exit information. Useful for building wires that wrap around the environment and bullet entry/exit effects.

### [Bang-Bang Kinetic Time-Optimal Movement Controller](https://github.com/zalo/MathUtilities/tree/master/Assets/Control) [[Gif]](https://i.imgur.com/ptwHgew.gif)
A special heuristic formula to compute the time-optimal movement trajectory for a double-integrating mass with limited thrust.

Formula taken from this excellent course: [http://underactuated.csail.mit.edu/underactuated.html?chapter=9](http://underactuated.csail.mit.edu/underactuated.html?chapter=9)

### [B�zier Trajectory Deformation](https://github.com/zalo/MathUtilities/tree/master/Assets/Bezier)
![[68747470733a2f2f692e696d6775722e636f6d2f546d33613962792e676966.gif]]

This is a technique for augmenting the end-point of trajectories composed of discrete segments, using a rigid-as-possible/"Blossoming" (B�zier-like) interpolation scheme. Includes an implementation for both 3D and 6D trajectories.

Inspired by this paper: [https://april.eecs.umich.edu/media/pdfs/olson2006icra.pdf](https://april.eecs.umich.edu/media/pdfs/olson2006icra.pdf)

### [Rigid Transform Pivot Point](https://github.com/zalo/MathUtilities/tree/master/Assets/Pivot) [[Gif]](https://imgur.com/5iCUe2A)
What appears to be a unique geometric solution for calculating the pivot point of a rigid transformation in 2D and 3D (where applicable). Is computationally efficient, geometrically intuitive, and can be extended to arbitrary dimensions.

### [Weighted Average Smoothing Spline](https://github.com/zalo/MathUtilities/tree/master/Assets/WeightedAverage) [[Gif]](https://i.imgur.com/NgNkZJ2.gif)
An example demonstrating how using a sliding window weighted average can yield a continuous smoothing spline for noisy 6 DoF data.

### [Thick Tesellated Plane Generator](https://github.com/zalo/MathUtilities/tree/master/Assets/ThickPlane)
Useful for generating solid meshes that are essentially deformed planes (shapes that are common in free-form optics).

### [Capsule Mesh Generator](https://github.com/zalo/MathUtilities/tree/master/Assets/Capsule) [[Shadertoy]](https://www.shadertoy.com/view/4l2cRW)
Useful for generating capsule meshes with an arbitrary length and radius.

### [Raytraced Sphere](https://github.com/zalo/MathUtilities/tree/master/Assets/Sphere)
A little shader that raytraces a pixel-perfect sphere on your object/billboard, for when you want to be pixel-bound rather than vertex bound...

Also contains a small demo for finding tangential points to circles and spheres.

### [Runtime .json GameObject Serialization](https://github.com/zalo/MathUtilities/blob/master/Assets/Serialization/SerializedGameObject.cs)
A serialization utility that smartly serializes and deserializes game object hierarchies, engine components, monobehaviours, references, and more at runtime using reflection. Useful for a save/load system or a runtime editor.

### [Matrix Class](https://github.com/zalo/MathUtilities/blob/master/Assets/Kalman/Matrix.cs)
A generic matrix class and a set of basic matrix operations (multiplication, addition, subtraction, inversion, cholesky decomposition, and more). Written to support the implementation of the Kalman Filter. Usage of any of these operations will allocate a new array (garbage), so be careful about using this on performance constrained systems.

### [Haptic Probe](https://github.com/zalo/MathUtilities/tree/master/Assets/Probe) [[Gif]](http://i.imgur.com/Ljy3U8y.gif)
An example implementing a haptic probe, where the force on the effector of the haptic controller is equal to the linear and angular displacement of the green cube to the gray one.

### [Bundle Adjustment](https://github.com/zalo/MathUtilities/tree/master/Assets/BundleAdustment) [[Gif]](https://i.imgur.com/rRBd6g9.gif)
My attempts at implementing [Bundle Adjustment](https://en.wikipedia.org/wiki/Bundle_adjustment) (an algorithm which attempts to solve for the relative motion between two camera images, given the motion of a set of feature-points between the images). The stereo-case now converges to a unique 6-DoF pose (in most situations). This implementation is highly parallelizable.

### [Torque Extension for HingeJoints](https://github.com/zalo/MathUtilities/tree/master/Assets/Torque)
This adds a function to apply torque "through" HingeJoints and to Rigidbodies in general.

### [n-Torus Encoding](https://github.com/zalo/MathUtilities/blob/master/Assets/Torus/Torus.cs#L21)
An untested function for encoding an n-dimensional vector to the surface of an n+1 dimensional torus. Originally intended to allow Neural Networks to learn continuous cyclic functions without ballooning the dimensionality to 2n (ie trivial x -> (sin(x), cos(x)) case).

### [Acceleration Dampened Rigidbodies](https://github.com/zalo/MathUtilities/tree/master/Assets/AccelerationDamping)
Attempts to simulate soft-spongy contact by damping the accelerations that are be applied to an object. Phenomena like friction cause it to exhibit strange artifacts...

### [2D Platforming Character](https://github.com/zalo/MathUtilities/tree/master/Assets/Platformer)
![[687474703a2f2f692e696d6775722e636f6d2f7749654b7178702e676966.gif]]

Uses a neat trick where, if the anchor of a spring joint is moved, both connected rigidbodies are physically affected. This allows one to easily simulate "muscles".

### [Rolling Cubes](https://github.com/zalo/MathUtilities/tree/master/Assets/Rolling)
![[687474703a2f2f692e696d6775722e636f6d2f5433703845704b2e676966.gif]]

Fun for the whole family!