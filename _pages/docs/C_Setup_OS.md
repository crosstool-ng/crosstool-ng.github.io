---
layout: docs
title: Setting up host OS
permalink: /docs/os-setup/
---

This section describes the setup needed by various operating systems
in order to run crosstool-NG, as well as some OS-specific caveats and limitations.
The package lists given in the following subsections cover all the features tested
by the sample configurations. You particular configuration may not need all those
packages. For example, `git` is needed if your configuration is for an uClinux-based
target which requires `elf2flt` utilities (which does "rolling releases" and must
be checked out from a Git repository).

Linux
-----

Sample configurations for supported Linux distributions are available as [Docker
configuration files](https://github.com/crosstool-ng/crosstool-ng/tree/master/testing/docker) in the 
`testing` directory. You can use the contents of these files as a list of the packages 
that need to be installed on a particular distribution.

If on the other hand you encounter a dependency not listed there, please let us
know over the mailing list or via a pull request!

Windows: Cygwin
---------------

*Originally contributed by: Ray Donnelly*

Prerequisites and instructions for using crosstool-NG for building a cross
toolchain on Windows (Cygwin) as build and, optionally Windows (hereafter)
MinGW-w64 as host.

1. Use Cygwin64 if you can. DLL base-address problems are lessened that
   way and if you bought a 64-bit CPU, you may as well use it.

2. You must enable Case Sensitivity in the Windows Kernel (this is only really
   necessary for Linux targets, but at present, crosstool-ng refuses to operate
   on case insensitive filesystems). The registry key for this is:
   `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel\obcaseinsensitive`
   Read more [here](https://cygwin.com/cygwin-ug-net/using-specialnames.html).

   Since release 1.7, Cygwin no longer supports the 'managed' mount option.
   You must use case sensitive FS.

3. Using `setup.exe` or `setup-x86_64.exe`, install the default packages and also
   the following ones:
<!-- TBD in a clean win install, check which ones are selected by default -->
   - autoconf
   - automake
   - bison
   - diffutils
   - flex
   - gawk
   - gcc-g++
   - git
   - gperf
   - help2man
   - libncurses-devel
   - make
   - patch
   - python-devel
   - texinfo
   - wget
   - xz

   Leave "Select required packages (RECOMMENDED)" ticked.

   Notes:
   - The packages marked with * are only needed if your host is MinGW-w64.

4. Although nativestrict symlinks seem like the best idea, extracting glibc fails
   when they are enabled, so just don't set anything here. If your host is MinGW-w64
   then these 'Cygwin-special' symlinks won't work, but you can dereference them by
   using tar options --dereference and --hard-dereference when making a final tarball.
   I plan to investigate and fix or at least work around the extraction problem.
   Read more [here](https://cygwin.com/cygwin-ug-net/using-cygwinenv.html).

5. If both BFD and GOLD linkers are enabled in binutils, `collect2.exe` will attempt
   to run `ld` which is a shell script so you need to make sure that a working shell
   is in your path.  Eventually this will be replaced with a native program for
   MinGW-w64 host.

**Notes**

1. Cygwin is slow. No, really, really slow. Expect about approximately 5x to 10x slowdown
   compared to a Linux system.

FreeBSD (and other BSD)
-----------------------

FreeBSD support is currently experimental in crosstool-NG.

FreeBSD does not provide a `gcc` command by default. Crosstool-NG and many of the packages
used expect this by default. A comprehensive fix for various ways of setting up the OS
is planned after the 1.23 release. Until then, setting up the following packages is
recommended as a prerequisite for crosstool-NG:

- `archivers/zip`
- `devel/automake`
- `devel/bison`
- `devel/gettext-tools`
- `devel/git`
- `devel/gmake`
- `devel/gperf`
- `devel/libatomic_ops`
- `devel/libtool`
- `devel/patch`
- `lang/gcc6`
- `lang/gawk`
- `misc/help2man`
- `print/texinfo`
- `textproc/asciidoc`
- `textproc/gsed`
- `textproc/xmlto`

Use any supported method of installation, e.g.:
````
cd /usr/ports/lang/gcc6
make install clean
````

Even with these packages installed, some of the samples are failing to build. YMMV.

> **Previous version of the installation guidelines**
>
> *Contributed by: Titus von Boxberg*
>
> Prerequisites and instructions for using ct-ng for building a cross toolchain on FreeBSD as host.
>
> 0. Tested on FreeBSD 8.0
>
> 1. Install (at least) the following ports
>    archivers/lzma
>    textproc/gsed
>    devel/gmake
>    devel/patch
>    shells/bash
>    devel/bison
>    lang/gawk
>    devel/automake110
>    ftp/wget
>
>    Of course, you should have /usr/local/bin in your PATH.
>
> 2. run ct-ng's configure with the following tool configuration:
> ````
> ./configure --with-sed=/usr/local/bin/gsed \
>       --with-make=/usr/local/bin/gmake \
>       --with-patch=/usr/local/bin/gpatch
>    [...other configure parameters as you like...]
> ````
>
> 3. proceed as described in general documentation
>   but use gmake instead of make


macOS (a.k.a Mac OS X, OS X)
----------------------------

*Originally contributed by: Titus von Boxberg*
*Updates by: Bryan Hundven*

These instructions have been tested on macOS Sonoma (14.3.1). I have not tested
on any other version with the following commands. *YMMV*
Currently we only support [homebrew](brew.sh).
Type any one of these commands: `git`, `clang`, or `gcc` in a terminal, and it
will prompt you to install the Xcode Command Line Tools.
Follow the instructions on the [homebrew](brew.sh) website to get homebrew
installed.
Run `brew doctor` to make sure everything is setup properly. If you have any
problems, please refer to the [homebrew](brew.sh) website.

1. Install the following packages:
```
autoconf automake bash binutils bison gawk git gnu-sed gnu-tar gettext help2man make ncurses readline wget xz zstd
```
   Then append the following to your shell's profile script (`~/.profile` or
   `~/.zprofile`):
```
BREW_PREFIX="$(brew --prefix)"

PATH=${PATH}:${BREW_PREFIX}/opt/binutils/bin"
PATH=${BREW_PREFIX}/opt/bison/bin:${PATH}"
PATH=/Volumes/crosstool-ng/bin:${PATH}"
export PATH

LDFLAGS="-L${BREW_PREFIX}/opt/binutils/lib"
LDFLAGS+=" -L${BREW_PREFIX}/opt/bison/lib"
LDFLAGS+=" -L${BREW_PREFIX}/opt/ncurses/lib"
export LDFLAGS

CPPFLAGS="-I${BREW_PREFIX}/opt/binutils/include"
CPPFLAGS+=" -I${BREW_PREFIX}/opt/ncurses/include"
export CPPFLAGS

export PKG_CONFIG_PATH="${BREW_PREFIX}/share/pkgconfig:${PKG_CONFIG_PATH}"
```

2. You have to use a case sensitive file system for crosstool-NG's build and target
   directories. Use a disk or disk image with a case sensitive FS that you
   mount somewhere.
```
cd $HOME
hdiutil create -size 40g -fs "Case-sensitive APFS" -type SPARSE -volname crosstool-ng crosstool-ng
```
   This will create `crosstool-ng.sparseimage`. You can mount it by typing:
```
cd $HOME
hdiutil mount crosstool-ng.sparseimage
```
   And unmount it by typing:
```
hdiutil unmount /Volumes/crosstool-ng
```

3. You can now build crosstool-ng on that volume by cloning the git repo or
   untaring a release in `/Volumes/crosstool-ng` and bootstrapping (only if from
   git) and configuring and building crosstool-ng.
```
./bootstrap # again, only needed if you got the source from git
./configure --prefix=/Volumes/crosstool-ng
make
make install
```
   Close your terminal app and open it again to get a new shell and verify your
   enviornment variables are set with `env`.

You should now be able to make a directory under `/Volumes/crosstool-ng` to build a toolchain in.
