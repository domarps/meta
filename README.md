# MeTA: ModErn Text Analysis

Please visit our [web page][meta-website] for information and tutorials
about MeTA!

### Build Status (by branch)
- master: [![Build
  Status](https://travis-ci.org/meta-toolkit/meta.svg?branch=master)](https://travis-ci.org/meta-toolkit/meta)
  [![Windows Build
  Status](https://ci.appveyor.com//api/projects/status/github/meta-toolkit/meta?svg=true&branch=master)](https://ci.appveyor.com/project/skystrife/meta)
- develop: [![Build
  Status](https://travis-ci.org/meta-toolkit/meta.svg?branch=develop)](https://travis-ci.org/meta-toolkit/meta)
  [![Windows Build
  Status](https://ci.appveyor.com//api/projects/status/github/meta-toolkit/meta?svg=true&branch=develop)](https://ci.appveyor.com/project/skystrife/meta)

# Outline
- [Intro](#intro)
    - [Documentation](#documentation)
    - [Tutorials](#tutorials)
- [Project Setup](#project-setup)
    - [Mac OS X](#mac-os-x-build-guide)
    - [Ubuntu](#ubuntu-build-guide)
    - [Arch Linux](#arch-linux-build-guide)
    - [Fedora](#fedora-build-guide)
    - [EWS/EngrIT](#ewsengrit-build-guide) (this is UIUC-specific)
    - [Windows](#windows-build-guide)
    - [Generic Setup Notes](#generic-setup-notes)

# Intro

MeTA is a modern C++ data sciences toolkit featuring

 - text tokenization, including deep semantic features like parse trees
 - inverted and forward indexes with compression and various caching strategies
 - a collection of ranking functions for searching the indexes
 - topic models
 - classification algorithms
 - graph algorithms
 - language models
 - CRF implementation (POS-tagging, shallow parsing)
 - wrappers for liblinear and libsvm (including libsvm dataset parsers)
 - UTF8 support for analysis on various languages
 - multithreaded algorithms

## Documentation

Doxygen documentation can be found [here][doxygen].

## Tutorials

We have walkthroughs for a few different parts of MeTA on the
[MeTA homepage][meta-website].

# Project setup

## Mac OS X Build Guide
Mac OS X 10.6 or higher is required. You may have success with 10.5, but
this is not tested.

You will need to have [homebrew][homebrew] installed, as well as the
Command Line Tools for Xcode (homebrew requires these as well, and it will
prompt for them during install, or you can install them with `xcode-select
--install` on recent versions of OS X).

Once you have homebrew installed, run the following commands to get the
dependencies for MeTA:

```bash
brew update
brew install cmake jemalloc lzlib icu4c
```

To get started, run the following commands:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
CXX=clang++ cmake ../ -DCMAKE_BUILD_TYPE=Release -DICU_ROOT=/usr/local/opt/icu4c
make
```

You can now test the system by running the following command:

```bash
ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

## Ubuntu Build Guide

The directions here depend greatly on your installed version of Ubuntu. To
check what version you are on, run the following command:

```bash
cat /etc/issue
```

If it reads "Ubuntu 12.04 LTS" or something of that nature, see the
[Ubuntu 12.04 LTS Build Guide](#ubuntu-1204-lts-build-guide). If it reads
"Ubuntu 14.04 LTS" (or 14.10), see the
[Ubuntu 14.04 LTS Build Guide](#ubuntu-1404-lts-build-guide). If your
version is less than 12.04 LTS, your operating system is not supported
(even by your vendor) and you should upgrade to at least 12.04 LTS (or
14.04 LTS, if possible).

### Ubuntu 12.04 LTS Build Guide
Building on Ubuntu 12.04 LTS requires more work than its more up-to-date
14.04 sister, but it can be done relatively easily. You will, however, need
to install a newer C++ compiler from a ppa, and switch to it in order to
build meta. We will also need to install a newer CMake version than is
natively available.

Start by running the following commands to get the dependencies that we
will need for building MeTA.

```bash
# this might take a while
sudo apt-get update
sudo apt-get install python-software-properties

# add the ppa that contains an updated g++
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update

# this will probably take a while
sudo apt-get install g++ g++-4.8 git make wget libjemalloc-dev

wget http://www.cmake.org/files/v3.2/cmake-3.2.0-Linux-x86_64.sh
sudo sh cmake-3.2.0-Linux-x86_64.sh --prefix=/usr/local
```

During CMake installation, you should agree to the license and then say "n"
to including the subdirectory. You should be able to run the following
commands and see the following output:

```bash
g++-4.8 --version
```

should print

    g++-4.8 (Ubuntu 4.8.1-2ubuntu1~12.04) 4.8.1
    Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

and

```bash
/usr/local/bin/cmake --version
```

should print

    cmake version 3.2.0

    CMake suite maintained and supported by Kitware (kitware.com/cmake).

Once the dependencies are all installed, you should be ready to build. Run
the following commands to get started:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
CXX=g++-4.8 /usr/local/bin/cmake ../ -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system by running the following command:

```bash
/usr/local/bin/ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

### Ubuntu 14.04 LTS Build Guide
Ubuntu 14.04 has a recent enough GCC for building MeTA, but we'll need to
add a ppa for a more recent version of CMake.

Start by running the following commands to install the dependencies for
MeTA.

```bash
# this might take a while
sudo apt-get update
sudo apt-get install software-properties-common

# add the ppa for cmake
sudo add-apt-repository ppa:george-edison55/cmake-3.x
sudo apt-get update

# install dependencies
sudo apt-get install cmake libicu-dev git libjemalloc-dev
```

Once the dependencies are all installed, you should double check your
versions by running the following commands.

```bash
g++ --version
```

should output

    g++ (Ubuntu 4.8.2-19ubuntu1) 4.8.2
    Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

and

```bash
cmake --version
```

should output

    cmake version 3.2.2

    CMake suite maintained and supported by Kitware (kitware.com/cmake).

Once the dependencies are all installed, you should be ready to build. Run
the following commands to get started:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
cmake ../ -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system by running the following command:

```bash
ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

## Arch Linux Build Guide
Arch Linux consistently has the most up to date packages due to its rolling
release setup, so it's often the easiest platform to get set up on.

To install the dependencies, run the following commands.

```bash
sudo pacman -Sy
sudo pacman -S clang cmake git icu libc++ make jemalloc
```

Once the dependencies are all installed, you should be ready to build. Run
the following commands to get started:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
CXX=clang++ cmake ../ -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system by running the following command:

```bash
ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

## Fedora Build Guide

This has been tested with Fedora 22+ (the oldest currently supported Fedora
as of the time of writing). You may have success with earlier versions, but
this is not tested. (If you're on an older version of Fedora, use `yum`
instead of `dnf` for the commands given below.)

To get started, install some dependencies:

```bash
# These may be already installed
sudo dnf install make git wget gcc-c++ jemalloc-devel cmake zlib-devel
```

You should be able to run the following commands and see the following
output:

```bash
g++ --version
```

should print

    g++ (GCC) 5.3.1 20151207 (Red Hat 5.3.1-2)
    Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

and

```bash
cmake --version
```

should print

    cmake version 3.3.2

    CMake suite maintained and supported by Kitware (kitware.com/cmake).


Once the dependencies are all installed, you should be ready to build. Run
the following commands to get started:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
cmake ../ -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system with the following command:

```bash
ctest --output-on-failure
```

## EWS/EngrIT Build Guide
If you are on a machine managed by Engineering IT at UIUC, you should
follow this guide. These systems have software that is much too old for
building MeTA, but EngrIT has been kind enough to package updated versions
of research software as modules. The modules provided for GCC and CMake are
recent enough to build MeTA, so it is actually mostly straightforward.

To set up your dependencies (**you will need to do this every time you log
back in to the system**), run the following commands:

```bash
module load gcc
module load cmake/3.4.0
```

Once you have done this, double check your versions by running the
following commands.

```bash
g++ --version
```

should output

    g++ (GCC) 4.8.2
    Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

and

```bash
cmake --version
```

should output

    cmake version 3.4.0

    CMake suite maintained and supported by Kitware (kitware.com/cmake).

If your versions are correct, you should be ready to build. To get started,
run the following commands:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta/

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
CXX="/software/gcc-4.8.2/bin/g++" cmake ../ -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system by running the following command:

```bash
ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

## Windows Build Guide

MeTA can be built on Windows using the MinGW-w64 toolchain with gcc. We
strongly recommend using [MSYS2][msys2] as this makes fetching the compiler
and related libraries significantly easier than it would be otherwise, and
it tends to have very up-to-date packages relative to other similar MinGW
distributions.

To start, [download the installer][msys2] for MSYS2 from the linked
website and follow the instructions on that page. Once you've got it
installed, you should use the MinGW shell to start a new terminal, in which
you should run the following commands to download dependencies and related
software needed for building:

```bash
pacman -Syu git mingw-w64-x86_64-{gcc,cmake,make,icu,jemalloc}
```

Then, exit the shell and launch the "MinGW-w64 Win64" shell. You can obtain
the toolkit and get started with:

```bash
# clone the project
git clone https://github.com/meta-toolkit/meta.git
cd meta

# set up submodules
git submodule update --init --recursive

# set up a build directory
mkdir build
cd build
cp ../config.toml .

# configure and build the project
cmake .. -G "MSYS Makefiles" -DCMAKE_BUILD_TYPE=Release
make
```

You can now test the system by running the following command:

```bash
ctest --output-on-failure
```

If everything passes, congratulations! MeTA seems to be working on your
system.

[msys2]: https://msys2.github.io/

## Generic Setup Notes

 - There are rules for clean, tidy, and doc. **After you run the `cmake`
   command once, you will be able to just run `make` as usual** when you're
   developing---it'll detect when the CMakeLists.txt file has changed and
   rebuild Makefiles if it needs to.

 - To compile in debug mode, just replace `Release` with `Debug` in the
   appropriate `cmake` command for your OS above and rebuild using `make`
   after.

 - Don't hesitate to reach out on [the forum][forum] if you encounter
   problems getting set up. We routinely build with a wide variety of
   compilers and operating systems through our continuous integration
   setups ([travis-ci][travis-ci] for Linux and OS X and
   [Appveyor][appveyor] for Windows), so we can be fairly certain that
   things should build on nearly all major platforms.

[homebrew]: http://brew.sh
[forum]: https://forum.meta-toolkit.org
[travis-ci]: https://travis-ci.org/meta-toolkit/meta
[appveyor]: https://ci.appveyor.com/project/skystrife/meta
[meta-website]: https://meta-toolkit.org
[doxygen]: https://meta-toolkit.org/doxygen/namespaces.html
