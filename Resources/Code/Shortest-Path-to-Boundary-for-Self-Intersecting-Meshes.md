---
tags:
  - software
  - utility
created: 22.04.2025
up: "[[Code]]"
side: "[[Gaia Physics Engine]]"
---
[https://github.com/AnkaChan/Shortest-Path-to-Boundary-for-Self-Intersecting-Meshes](https://github.com/AnkaChan/Shortest-Path-to-Boundary-for-Self-Intersecting-Meshes)

Code for the Siggraph 2023 Paper: Shortest Path to Boundary for Self-Intersecting Meshes.

Note that this is the code for the shortest path to surface algorithm. It does not include the simulation part, which will be released separately, as [Gaia physics engine](https://github.com/AnkaChan/Gaia).

## Clone and Compile
### Get the Code
Please download the code using the following command:

```shell
git clone git@github.com:AnkaChan/Shortest-Path-to-Boundary-for-Self-Intersecting-Meshes.git --recursive
```

### Dependencies
This Algorithm has the following dependencies:

- [MeshFrame2](https://github.com/AnkaChan/MeshFrame2): for mesh processing (included as submodule)
- [CuMatrix](https://github.com/AnkaChan/CuMatrix/tree/main): for geometry and matrix computation (included as submodule)
- OneTBB (included as a submodule)
- Eigen3 (tested with 3.3.7)
- Embree (tested with 3.13.1) Known issues exist for version >= 3.13.4.

You need to install Eigen3 and Embree with the required version and add attributes: "Eigen3_DIR" and "embree_DIR" whose values are their corresponding config.cmake path to your environment variable to allow CMake to find them.

### Compile
Use CMake to build the project and compile it. Currently, the code is only tested with Windows & Visual Studio.

### Common Problems
1. "tbb12.lib" is not found.  
    The reason of this bug is that Embree has already had a compilation of tbb, named tbb.lib. However, OneTBB is asking for a file called tbb12.lib. An easy solution is to duplicate that tbb.lib from embree, name it "tbb12.lib" and put it in the same repository.
2. "Host key verification failed. fatal: Could not read from remote repository."  
    This issue could be that Github isn't present in your ~/.ssh/known_hosts file. Append GitHub to the list of authorized hosts: `ssh-keyscan -H github.com >> ~/.ssh/known_hosts`

## Test and Run
After compilation, it should give you a binary file called: Shortest-Path-Test. Run it using the following command:

```shell
Shortest-Path-Test [PathToRepo]/TestData/Parameters.json [PathToRepo]/TestData/IntersectingShape.t
```

It will do discrete collision detection for this mesh and query the shortest path to surface for each of the intersecting points.