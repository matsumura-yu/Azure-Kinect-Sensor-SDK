# Building and Dependencies

## Support Configurations

Compilers: MSVC, Clang, GCC

Platforms: Windows, Linux

Architectures: amd64

MSVC is Windows only

Clang and GCC is Linux only

Toolchain files in [cmake/toolchains](cmake/toolchains) are mainly used for our
CI system but can be used to test specific flavors if needed.

## Dependencies

Building on Windows and Linux need an active internet connection. Part of the
build will require download dependencies from online.

### Windows Dependencies

The following tools are required to build on Windows.

* [VS2017](https://visualstudio.microsoft.com/vs/). Contains the toolchains
  for building. Please be sure to install the following workloads
  * Desktop development with C++
  * Linux development with C++

  Also make sure the following components are selected
  * Visual C++ Tools for CMake
  * Test adapter for Google Test
  * Visual C++ tools for CMake and Linux

* [cmake](https://cmake.org/download/). Add cmake to the PATH. Must be
  version 3.10 or higher. Only necessary if your version of VS2017 is less than
  15.3 or you have not installed the "Linux development with C++" workload as
  part of your VS installation.

* [ninja](https://github.com/ninja-build/ninja/releases). Add ninja to the
  PATH. Only necessary if your version of VS2017 is less than 15.3 or you have
  not installed the "Linux development with C++" workload as part of your VS
  installation.

* [tar](http://gnuwin32.sourceforge.net/packages/gtar.htm). Note, if you are
  running Windows 10 version 17063 or later, this comes installed by default.

* [python3](https://www.python.org/getit/). During the install make sure to add
  python to path

* [git lfs](https://git-lfs.github.com/). This is used to pull down binary
  artifacts used for testing.

The following tools are optional.

* [Doxygen](http://www.doxygen.nl/download.html). Add doxygen to the PATH.
  Required for building documentation.

* [Clang-Format](http://releases.llvm.org/download.html). Please download clang
  v6.0.0 since that is what we are using to format our code.

If you are building from a command prompt, it must be a x64 visual studio
developer command prompt in order for CMake to find the installed compilers.
We build both 32-bit and 64-bit binaries, but 64-bit binaries are the only
binaries that are tested. (The command prompt should be called something like
x64 Native Tools Command Prompt for VS 2017). Note: call the command line tool
with the option -arch=amd64 for x64 builds i.e 'VsDevCmd.bat -arch=amd64'

You can run [scripts/verify-windows.ps1](scripts/verify-windows.ps1) to
verify that your Windows PC is setup properly.

### Linux Dependencies

Most of these dependencies should be avaiable through the distribution's
package manager.

* [cmake](https://cmake.org/download/). Add cmake to the PATH. Must be
  version 3.10 or higher. If your distro has a version of cmake that is too old
  you can download and build cmake from source.

* [ninja](https://github.com/ninja-build/ninja/releases). Add ninja to the
  PATH.

* gcc or clang (6.0.0)

* pkgconfig

* python3

* doxygen (optional)

* zip (needed for VS cross compiled builds)

* libssl

* libusb-1.0

* uuid-dev

* git-lfs

If you are using Ubuntu you can use our CI script
[scripts/bootstrap-ubuntu.sh](scripts/bootstrap-ubuntu.sh) to download and
install all needed tools and libraries.

## Building

### Building using a terminal (cross platform)

1. Create a folder named "build" in the root of the git repo and cd into that
    directory.

    ```shell
    mkdir build && cd build
    ```

2. Run cmake from that directory. The preferred generator is ninja. All other
    generators are untested.

    ```shell
    cmake .. -GNinja
    ```

3. Run the generator (ninja) to run the actual build

    ```shell
    ninja
    ```

### Building using Visual Studio IDE

Visual Studio 2017 supports opening CMake based projects directly.
Use File / Open / CMake ... to open the root CMakeLists.txt in the project.

To cross compile for Linux on Windows you can run a pre-configured
[docker container](../docker/DOCKER.md) with the tools needed for Visual
Studio.