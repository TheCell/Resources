---
tags:
  - cpp
  - utility
created: 24.04.2025
up: "[[Code]]"
---
[https://github.com/EricLengyel/Terathon-Math-Library](https://github.com/EricLengyel/Terathon-Math-Library)

This is a C++ math library containing classes for vectors, matrices, quaternions, and elements of projective geometric algebra. The specific classes are the following:

- **Vector2D** – A 2D vector (_x_, _y_) that extends to four dimensions as (_x_, _y_, 0, 0).
- **Vector3D** – A 3D vector (_x_, _y_, _z_) that extends to four dimensions as (_x_, _y_, _z_, 0).
- **Vector4D** – A 4D vector (_x_, _y_, _z_, _w_).
- **Point2D** – A 2D point (_x_, _y_) that extends to four dimensions as (_x_, _y_, 0, 1).
- **Point3D** – A 3D point (_x_, _y_, _z_) that extends to four dimensions as (_x_, _y_, _z_, 1).
- **Bivector3D** – A 3D bivector _x_ **e**23 + _y_ **e**31 + _z_ **e**12.
- **Matrix2D** – A 2×2 matrix.
- **Matrix3D** – A 3×3 matrix.
- **Matrix4D** – A 4×4 matrix.
- **Transform2D** – A 3×3 matrix with fourth row always (0, 0, 1).
- **Transform3D** – A 4×4 matrix with fourth row always (0, 0, 0, 1).
- **Quaternion** – A conventional quaternion _x_**i** + _y_**j** + _z_**k** + _w_.
- **DualNum** – A dual number _s_ + _tε_.

2D rigid geometric algebra

- **FlatPoint2D** – A 2D flat point _x_ **e**1 + _y_ **e**2 + _z_ **e**3.
- **Line2D** – A 2D line _x_ **e**23 + _y_ **e**31 + _z_ **e**12.
- **Motor2D** – A 2D motion operator _Qx_ **e**1 + _Qy_ **e**2 + _Qz_ **e**3 + _Qw_ 𝟙.
- **Flector2D** - A 2D reflection operator _Fx_ **e**23 + _Fy_ **e**31 + _Fz_ **e**12 + _Fw_ **1**.

3D rigid geometric algebra

- **FlatPoint3D** – A 3D [flat point](https://rigidgeometricalgebra.org/wiki/index.php?title=Point) _x_ **e**1 + _y_ **e**2 + _z_ **e**3 + _w_ **e**4.
- **Line3D** – A 3D [line](https://rigidgeometricalgebra.org/wiki/index.php?title=Line) _lvx_ **e**41 + _lvy_ **e**42 + _lvz_ **e**43 + _lmx_ **e**23 + _lmy_ **e**31 + _lmz_ **e**12.
- **Plane3D** – A 3D [plane](https://rigidgeometricalgebra.org/wiki/index.php?title=Plane) _x_ **e**234 + _y_ **e**314 + _z_ **e**124 + _w_ **e**321.
- **Motor3D** – A 3D [motion operator](https://rigidgeometricalgebra.org/wiki/index.php?title=Motor) _Qvx_ **e**41 + _Qvy_ **e**42 + _Qvz_ **e**43 + _Qvw_ 𝟙 + _Qmx_ **e**23 + _Qmy_ **e**31 + _Qmz_ **e**12 + _Qmw_ **1**.
- **Flector3D** - A 3D [reflection operator](https://rigidgeometricalgebra.org/wiki/index.php?title=Flector) _Fpx_ **e**1 + _Fpy_ **e**2 + _Fpz_ **e**3 + _Fpw_ **e**4 + _Fgx_ **e**423 + _Fgy_ **e**431 + _Fgz_ **e**412 + _Fgw_ **e**321.

2D conformal geometric algebra

- **RoundPoint2D** – A 2D round point _x_ **e**1 + _y_ **e**2 + _z_ **e**3 + _w_ **e**4.
- **Dipole2D** – A 2D dipole _dgx_ **e**23 + _dgy_ **e**31 + _dgz_ **e**12 + _dpx_ **e**41 + _dpy_ **e**42 + _dpz_ **e**43.
- **Circle2D** – A 2D circle _w_ **e**321 + _x_ **e**423 + _y_ **e**431 + _z_ **e**412.

3D conformal geometric algebra

- **RoundPoint3D** – A 3D [round point](https://conformalgeometricalgebra.org/wiki/index.php?title=Round_point) _x_ **e**1 + _y_ **e**2 + _z_ **e**3 + _w_ **e**4 + _u_ **e**5.
- **Dipole3D** – A 3D [dipole](https://conformalgeometricalgebra.org/wiki/index.php?title=Dipole) _dvx_ **e**41 + _dvy_ **e**42 + _dvz_ **e**43 + _dmx_ **e**23 + _dmy_ **e**31 + _dmz_ **e**12 + _dpx_ **e**15 + _dpy_ **e**25 + _dpz_ **e**35 + _dpw_ **e**45.
- **Circle3D** – A 3D [circle](https://conformalgeometricalgebra.org/wiki/index.php?title=Circle) _cgx_ **e**423 + _cgy_ **e**431 + _cgz_ **e**412 + _cgw_ **e**321 + _cvx_ **e**415 + _cvy_ **e**425 + _cvz_ **e**435 + _cmx_ **e**235 + _cmy_ **e**315 + _cmz_ **e**125.
- **Sphere3D** – A 3D [sphere](https://conformalgeometricalgebra.org/wiki/index.php?title=Sphere) _u_ **e**1234 + _x_ **e**4235 + _y_ **e**4315 + _z_ **e**4125 + _w_ **e**3215.

## Component Swizzling
Vector components can be swizzled using shading-language syntax. As an example, the following expressions are all valid for a `Vector3D` object `v`:

- `v.x` – The _x_ component of `v`.
- `v.xy` – A 2D vector having the _x_ and _y_ components of `v`.
- `v.yzx` – A 3D vector having the components of `v` in the order (_y_, _z_, _x_).

Support for repeated components in a swizzle can be enabled by defining `TERATHON_SWIZZLE_REPEAT`. This is disabled by default because the large number of additional swizzling possibilities increases compile times substantially. Swizzles with repeated components are always `const` so that it's not possible to assign to them.

Rows, columns, and submatrices can be extracted from matrix objects using a similar syntax. As an example, the following expressions are all valid for a `Matrix3D` object `m`:

- `m.m12` – The (1,2) entry of `m`.
- `m.row0` – The first row of `m`.
- `m.col1` – The second column of `m`.
- `m.matrix2D` – The upper-left 2×2 submatrix of `m`.
- `m.transpose` – The transpose of `m`.

All of the above are generally _free operations_, with no copying, when their results are consumed by an expression. For more information, see Eric Lengyel's 2018 GDC talk [Linear Algebra Upgraded](https://terathon.com/gdc18_lengyel.pdf).

## Geometric Algebra
The `^` operator is overloaded for cases in which the wedge or antiwedge product can be applied between vectors, bivectors, flat points, lines, planes, round points, dipoles, circles, and spheres. (Note that `^` has lower precedence than just about everything else, so parentheses will be necessary.)

The library does not provide operators that directly calculate the geometric product and antiproduct because they would tend to generate inefficient code and produce intermediate results having unnecessary types when something like the sandwich product **Q** ⟇ _p_ ⟇ **Q̰** appears in an expression. Instead, there are `Transform()` functions that take some object _p_ for the first parameter and the motor **Q** with which to transform it for the second parameter.

See Eric Lengyel's [Projective Geometric Algebra website](https://projectivegeometricalgebra.org) for more information about operations among these types.

## API Documentation
There is API documentation embedded in the header files. The formatted equivalent can be found in the [C4 Engine documentation](https://c4engine.com/docs/Math/index.html).

## Mathematica Packages
Mathematica packages for rigid, conformal, and spacetime geometric algebras are available in the [Geometric Algebra](https://github.com/EricLengyel/Geometric-Algebra/) repository.

## Licensing
Separate proprietary licenses are available from Terathon Software. Please [send an email](mailto:service@terathon.com) with details about your particular use case if you are interested.