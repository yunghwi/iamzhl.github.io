---
title: How to build Clover from source
date: 2017-09-20 20:30:51
categories: Study
description: How to build Clover from source
tags: 
- Clover
---

# Perface
> How to build Clover from source
> [原出处](https://clover-wiki.zetam.org/Development#Compiling-from-source)

# Compiling from source

To compile a project you need a compiler and libraries - it's a truism. What needs to be done in this particular case? The libraries are covered by the EDK2 environment from http://sourceforge.net/projects/edk2. Due to the fact that this project was created for a Hackintosh, focus will be set on the compilation process under Mac OS X. Nevertheless, this is not the only option. EDK2 was designed for a compilation on different systems like Windows, Linux, and others. Visual Studio 20xx is needed for Windows, Xcode Command Line Tools for Mac OS X, and gcc for Linux. The pre-bundled software are still not enough for a successful compilation under Mac. The recommendation is - same as for Linux - to build a new version of gcc using the script "buildgcc.sh" from Clover's directory. Why not clang? - It still does not produce any working code. Let's not give up hope, though.

Now to the real deal. A reader who came to this section cannot be a simple user by definition and thus it is not necessary to explain how to use a terminal.

## How to build

### Download the source code and prepare the environment

```
mkdir src
cd src
svn co  -r 18198 svn://svn.code.sf.net/p/edk2/code/trunk/edk2 edk2
cd edk2
make -C BaseTools/Source/C
svn co svn://svn.code.sf.net/p/cloverefiboot/code Clover 
cd Clover
```

### Building the compiler. gcc-4.9. This is the compiler that can do LTO optimization.

```
./buildgettext.sh
./buildgcc-4.9.sh 
./buildnasm.sh 
```

### Adapting the EDK2 environment to our needs

```
./edksetup.sh 
cp -R Clover/Patches_for_EDK2/* ./
```

## How to update

### In the case you already did all these steps and just want to update Clover then no need to repeat this steps. Just do:

```
cd Clover
svn up
```

### Now it is possible to build CloverEFI. For example like this:

```
./ebuild.sh
./ebuild.sh -mc
./ebuild.sh --ia32
```

> Other compilation scripts are available and usually they are documented. Look, choose and decide for yourself, which one you want to use.

### Small note: HFSPlus.efi is not available in the repository. There are two options: You can find it somewhere else or change the project definition files .fdf to use the open driver VboxHfs instead of the private one. It is a bit slower and has some downsides that will probably be corrected in future, but it is functional. Change:

```
# foreign file system support
#INF  Clover/VBoxFsDxe/VBoxHfs.inf
INF  RuleOverride=BINARY Clover/HFSPlus/HFSPlus.inf
```

to:

```
# foreign file system support
INF  Clover/VBoxFsDxe/VBoxHfs.inf
#INF  RuleOverride=BINARY Clover/HFSPlus/HFSPlus.inf
```

### This project does not stand still and that is why this instruction may become obsolete because of some small change. This project is for people who think and for those who can figure out what is wrong and what to do.

### Building the installer package

```
cd ~/src/edk2/Clover/CloverPackage/
./makepkg
./makeiso
```

> That's all! Some steps can be left out if you do this process repeatedly. For example you can issue an "svn up" instead of re-downloading the whole project one more time, exclude the 32-bit compilation process and skip compiling. New users can use automated scripts CloverGrower and its Pro version, however they should think about just using the pre-made installer package first.
And now, as everything is ready, you can do the installation.


