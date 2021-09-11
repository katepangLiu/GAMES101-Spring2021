# Setup Optix  Environment On Win10

## My Environment

- Win10 x64
- GTX 950M



## Downloads

[optix/download](https://developer.nvidia.com/designworks/optix/download)



- Driver
  - [Driver 471.96/471.96-notebook-win10](https://us.download.nvidia.cn/Windows/471.96/471.96-notebook-win10-win11-64bit-international-dch-whql.exe)
- C++ Compiler
  - [Visual Studio Community 2019](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16#)
  - [CMake 3.21.2](https://github.com/Kitware/CMake/releases/download/v3.21.2/cmake-3.21.2-windows-x86_64.msi) for compile optix samples
- [CUDA tookit 11.4.2](https://developer.download.nvidia.cn/compute/cuda/11.4.2/local_installers/cuda_11.4.2_471.41_win10.exe)
- [Optix SDK 7.3](https://developer.nvidia.com/optix/downloads/7.3.0/win64)





## CUDA

```shell
Installed:
     - Nsight for Visual Studio 2019
     - Nsight Monitor
Not Installed:
     - Nsight for Visual Studio 2017
       Reason: VS2017 was not found
     - Integrated Graphics Frame Debugger and Profiler
       Reason: see https://developer.nvidia.com/nsight-vstools
     - Integrated CUDA Profilers
       Reason: see https://developer.nvidia.com/nsight-vstools
```



## Optix Samples Compile

### With CMake GUI

```
In order to compile the samples in the SDK you need the following.

1. Visual Studio. Supported versions are listed in the release notes.

2. CUDA Toolkit. Supported versions are listed in the release notes.

3. CMake 3.0 minimum (http://www.cmake.org/cmake/resources/software.html).
I suggest the executable installer.


Instructions for building.

1. Start up cmake-gui from the Start Menu.

2. Select the C:\ProgramData\NVIDIA Corporation\OptiX SDK <version>\SDK directory
   from the installation for the source file location.

3. Create a build directory that isn't the same as the source directory.  For
   example, C:\ProgramData\NVIDIA Corporation\OptiX SDK <version>\SDK\build.
   If you don't have permissions to write into the this directory (writing into
   the "C:/Program Files" directory can be restricted in some cases), pick a different
   directory where you do have write permissions.  If you type in the directory
   (instead of using the "Browse Build..." button), CMake will ask you at the
   next step to create the directory for you if it doesn't already exist.

4. Press "Configure" button and select the version of Visual Studio you wish to
   use.  Note that the 64-bit compiles are separate from the 32-bit compiles.
   The difference between the bitnesses of the build will be marked with "Win64"
   for 64 bit builds (e.g. "Visual Studio 14 2015 Win64" for 64 bit builds and
   "Visual Studio 14 2013" for 32 bit builds).  Also note that OptiX only
   supports 64 bit builds, and CMake defaults to 32 bit configures, so you must
   change it.  Leave all other options on their default.  Press "OK".  This can
   take awhile while source level dependencies for CUDA files are computed.

5. Press "Configure" again.  Followed by "Generate".

6. Open the OptiX-Samples.sln solution file in the build directory you created.

7. Select "Build Solution" from the IDE.

8. Right click on one of the sample program targets in the solution explorer and
   select "Set as start up project".

9. Run the sample.  "q" or "Esc" will close the window.

Note that due to the way dependencies are automatically handled for CUDA
compilation in Visual Studio, if you build again Visual Studio will likely ask
you to reload your projects.  Please do so.  Subsequent compiles should not
result in reloading unless you change the files that are included by a CUDA
file.

Further instructions regarding the build system can be found in comments in the
SDK's CMakeLists.txt file.

```



Error: Invalid target architecture. Maximum feasible for current context: sm_50, found: sm_60

Fix: https://forums.developer.nvidia.com/t/optix-7-1-issue-with-running-samples-on-a-maxwell-card/140118

```
configure
replace all sm_60 with sm_50
replace all compute_60 with compute_50
generation
```

## Start

Source Arch:

- Device Code

  - run on GPU(CUDA code

  - Programs
    - Ray-Generation
    - Intersection
    - Closest-Hit
    - Miss
    - Other
      - Any-hit
      - Exception
      - Callable

- Host Code
  - run on CPU(cpp code)
    - Device Contexts
    - Acceleration Structures
    - Modules, Program Groups and Pipelines
    - Shader Binding Table 
      - consists of a set of shader function handles and embedded parameters for these functions
    - Launching an OptiX Pipeline





## Ref:

https://developer.nvidia.com/blog/how-to-get-started-with-optix-7/

https://www.willusher.io/graphics/2019/11/20/the-sbt-three-ways

