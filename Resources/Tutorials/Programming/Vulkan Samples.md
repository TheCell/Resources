---
tags:
  - vulkan
  - samples
  - cpp
created: 18.04.2025
up: "[[Tutorials]]"
---
[https://github.com/KhronosGroup/Vulkan-Samples](https://github.com/KhronosGroup/Vulkan-Samples)

One stop solution for all Vulkan samples

## Vulkan Documentation Site
Documentation for the samples is best viewed at the new [Vulkan Documentation Site](https://docs.vulkan.org/samples/latest/README.html). The documentation uses AsciiDoc which isn’t fully supported by github.

## Introduction
The Vulkan Samples is collection of resources to help you develop optimized Vulkan applications.

If you are new to Vulkan the [API samples](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/samples/api/README.adoc) are the right place to start. Additionally you may find the following links useful:

- [Vulkan Guide](https://docs.vulkan.org/guide/latest/index.html)
    
- [Get Started in Vulkan](https://docs.vulkan.org/tutorial/latest/index.html)
    

[Performance samples](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/samples/performance/README.adoc) show the recommended best practice together with real-time profiling information. They are more advanced but also contain a detailed tutorial with more in-detail explanations.

### Goals
- Create a collection of resources that demonstrate best-practice recommendations in Vulkan
    
- Create tutorials that explain the implementation of best-practices and include performance analysis guides
    
- Create a [framework](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/framework/README.adoc) that can be used as reference material and also as a sandbox for advanced experimentation with Vulkan
    

## Samples
- [Listing of all samples available in this repository](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/samples/README.adoc)
    

## General information
- **Project Basics**
    
    - [Controls](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/misc.adoc#controls)
        
    - [Debug window](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/misc.adoc#debug-window)
        
    - [Create a Sample](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/scripts/README.adoc)
        
    
- **Vulkan Essentials**
    
    - [How does Vulkan compare to OpenGL ES? What should you expect when targeting Vulkan?](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/samples/vulkan_basics.adoc)
        
    
- **Misc**
    
    - [Driver version](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/misc.adoc#driver-version)
        
    - [Memory limits](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/memory_limits.adoc)
        
    

## Setup
Prerequisites: [git](https://git-scm.com/downloads) with [git large file storage (git-lfs)](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage).

Clone the repo with submodules using the following command:

git clone --recurse-submodules https://github.com/KhronosGroup/Vulkan-Samples.git
cd Vulkan-Samples

Follow build instructions for your platform below.

|   |   |
|---|---|
|Note|The full repository is very large, and some ISPs appear to have trouble providing a robust connection to github while the clone is being made.<br><br>If you notice problems such as submodules downloading at reported rates in the tens of kB/s, or fatal timeout errors occurring, these may be due to network routing issues to github within your ISP’s internal network, rather than anything wrong in your own networking setup.<br><br>It can be very difficult to get ISPs to acknowledge such problems exist, much less to fix them.<br><br>One workaround is to switch the repository to use ssh protocol prior to the submodule download, which can be done via e.g.<br><br>```shell<br>git clone git@github.com:KhronosGroup/Vulkan-Samples.git<br>cd Vulkan-Samples<br>perl -i -p -e 's\|https://(.*?)/\|git@\1:\|g' .gitmodules<br>git submodule sync<br>git submodule update<br>```|

|   |
|---|
|While this can be a good alternative if you are running into this connection issue, you must have GitHub ssh key authentication setup to use ssh protocol - see [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) for details. So it is a not a solution we can implement as the repository default.<br><br>Another option which may help is to run github through a VPN service.|

## Build
### Supported Platforms
- Windows - [Build Guide](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.adoc#windows)
    
- Linux - [Build Guide](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.adoc#linux)
    
- Android - [Build Guide](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.adoc#android)
    
- macOS - [Build Guide](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.adoc#macos)
    
- iOS - [Build Guide](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.adoc#ios)
    

## Usage
The following shows some example command line usage on how to configure and run the Vulkan Samples.

> Make sure that you are running the samples from the root directory of the repository. Otherwise the samples will not be able to find the assets. ./build/app/bin/<BuildType>/<Arch>/vulkan_samples

# For the entire usage use
vulkan_samples --help

# For subcommand usage use
vulkan_samples <sub_command> --help

# Run Swapchain Images sample
vulkan_samples sample swapchain_images

# Run AFBC sample in benchmark mode for 5000 frames
vulkan_samples sample afbc --benchmark --stop-after-frame 5000

# Run compute nbody using headless_surface and take a screenshot of frame 5
# Note: headless_surface uses VK_EXT_headless_surface.
# This will create a surface and a Swapchain, but present will be a no op.
# The extension is supported by Swiftshader(https://github.com/google/swiftshader).
# It allows to quickly test content in environments without a GPU.
vulkan_samples sample compute_nbody --headless_surface -screenshot 5

# Run all the performance samples for 10 seconds in each configuration
vulkan_samples batch --category performance --duration 10

# Run Swapchain Images sample on an Android device
adb shell am start-activity -n com.khronos.vulkan_samples/com.khronos.vulkan_samples.SampleLauncherActivity -e sample swapchain_images