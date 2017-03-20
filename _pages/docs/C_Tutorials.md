---
layout: docs
title: Miscellaneous tutorials
permalink: /docs/tutorials/
---

<!-- TBD needs update/rework; will be changed as I set up test machines for 1.23-rc testing -->

Using crosstool-NG on FreeBSD (and other BSD)
---------------------------------------------

*Contributed by: Titus von Boxberg*

Prerequisites and instructions for using ct-ng for building a cross toolchain on FreeBSD as host.

0. Tested on FreeBSD 8.0

1. Install (at least) the following ports
   archivers/lzma
   textproc/gsed
   devel/gmake
   devel/patch
   shells/bash
   devel/bison
   lang/gawk
   devel/automake110
   ftp/wget

   Of course, you should have /usr/local/bin in your PATH.

2. run ct-ng's configure with the following tool configuration:
````
   ./configure --with-sed=/usr/local/bin/gsed --with-make=/usr/local/bin/gmake \
   --with-patch=/usr/local/bin/gpatch
   [...other configure parameters as you like...]
````

3. proceed as described in general documentation
   but use gmake instead of make


Using crosstool-NG on MacOS-X
-----------------------------

*Contributed by: Titus von Boxberg*

Prerequisites and instructions for using crosstool-NG for building a cross
toolchain on MacOS as host.

0. Mac OS Snow Leopard, with Developer Tools 3.2 installed, or
   Mac OS Leopard, with Developer Tools & newer gcc (>= 4.3) installed
   via macports

1. You have to use a case sensitive file system for ct-ng's build and target
   directories. Use a disk or disk image with a case sensitive fs that you
   mount somewhere.

2. Install macports (or similar easy means of installing 3rd party software),
   make sure that macport's bin dir is in the front (!) of your PATH.
   Further on assuming it is /opt/local/bin.

3. Install (at least) the following macports
   lzmautils
   libtool
   binutils
   gsed
   gawk
   gcc43 (only necessary for Leopard OSX 10.5)
   gcc_select (only necessary for OSX 10.5, or Xcode > 4)

4. Prerequisites
   On Leopard, make sure that the macport's gcc is called with the default
   commands (gcc, g++,...), via macport's gcc_select

   On OSX 10.7 Lion / when using Xcode >= 4 make sure that the default commands
   (gcc, g++, etc.) point to gcc-4.2, NOT llvm-gcc-4.2
   by using macport's gcc_select feature. With MacPorts >= 1.9.2
   the command is: "sudo port select --set gcc gcc42"
   This also requires (like written above) that macport's bin dir
   comes before standard directories in your PATH environment variable
   because the gcc symlink is installed in /opt/local/bin and the default /usr/bin/gcc
   is not removed by the gcc select command!
   Explanation: llvm-gcc-4.2 (with Xcode 4.1 it is on my machine
   "gcc version 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2335.15.00)")
   cannot boostrap gcc. See [this bug](http://llvm.org/bugs/show_bug.cgi?id=9571)

5. run ct-ng's configure with the following tool configuration
   (assuming you have installed the tools via macports in /opt/local):
````
   ./configure --with-sed=/opt/local/bin/gsed                 \
               --with-libtool=/opt/local/bin/glibtool         \
               --with-libtoolize=/opt/local/bin/glibtoolize   \
               --with-objcopy=/opt/local/bin/gobjcopy         \
               --with-objdump=/opt/local/bin/gobjdump         \
               --with-readelf=/opt/local/bin/greadelf         \
               --with-grep=/opt/local/bin/ggrep               \
               [...other configure parameters as you like...]
````

6) proceed as described in standard documentation

-----

HINTS:
- Apparently, GNU make's builtin variable `.LIBPATTERNS` is misconfigured
  under MacOS: It does not include `lib%.dylib`.
  This affects build of (at least) gdb-7.1
  Put `lib%.a lib%.so lib%.dylib` as `.LIBPATTERNS` into your environment
  before executing `ct-ng build`.
  See [here](http://www.gnu.org/software/make/manual/html_node/Libraries_002fSearch.html)
  for details.
- `ct-ng menuconfig` will not work on Snow Leopard 10.6.3 since libncurses
  is broken with this release. MacOS <= 10.6.2 and >= 10.6.4 are ok.

Using crosstool-NG on Windows
-----------------------------

*Contributed by: Ray Donnelly*

Prerequisites and instructions for using crosstool-NG for building a cross
toolchain on Windows (Cygwin) as build and, optionally Windows (hereafter)
MinGW-w64 as host.

0. Use Cygwin64 if you can. DLL base-address problems are lessened that
   way and if you bought a 64-bit CPU, you may as well use it.

1. You must enable Case Sensitivity in the Windows Kernel (this is only really
   necessary for Linux targets, but at present, crosstool-ng refuses to operate
   on case insensitive filesystems). The registry key for this is:
   `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\kernel\obcaseinsensitive`
   Read more [here](https://cygwin.com/cygwin-ug-net/using-specialnames.html).

2. Using `setup{,-x86_64}.exe`, install the default packages and also the
   following ones: (tested versions in brackets, please test newer versions
   and report successes via pull requests changing this list and failures to:
   https://github.com/crosstool-ng/crosstool-ng/issues
   autoconf (13-1), make (4.1-1), gcc-g++ (4.9.3-1), gperf (3.0.4-2),
   bison (3.0.4-1), flex (2.5.39-1), texinfo (6.0-1), wget (1.16.3-1),
   patch (2.7.4-1), libtool (2.4.6-2), automake (9-1), diffutils (3.3-3),
   libncurses-devel (6.0-1.20151017), help2man (1.44.1-1)
   mingw64-i686-gcc-g++* (4.9.2-2), mingw64-x86_64-gcc-g++* (4.9.2-2)
   Leave "Select required packages (RECOMMENDED)" ticked.

   Notes:
   - The packages marked with * are only needed if your host is MinGW-w64.
   - Unfortunately, wget pulls in an awful lot of dependencies, including
       Python 2.7, Ruby, glib and Tcl.

3. Although nativestrict symlinks seem like the best idea, extracting glibc fails
   when they are enabled, so just don't set anything here. If your host is MinGW-w64
   then these 'Cygwin-special' symlinks won't work, but you can dereference them by
   using tar options --dereference and --hard-dereference when making a final tarball.
   I plan to investigate and fix or at least work around the extraction problem.
   Read more [here](https://cygwin.com/cygwin-ug-net/using-cygwinenv.html).

4. `collect2.exe` will attempt to run `ld` which is a shell script that runs either
   ld.exe or gold.exe so you need to make sure that a working shell is in your path.
   Eventually I will replace this with a native program for MinGW-w64 host.

Using crosstool-NG to build Xtensa toolchains
---------------------------------------------

*Contributed by: Max Filippov*

Xtensa cores are highly configurable: endianness, instruction set, register set
of a core is chosen at processor configuration time. New registers and
instructions may be added by designers, making each core configuration unique.
Toolchain components cannot know about features of each individual core and
need to be configured in order to be compatible with particular architecture
variant. This configuration includes:
- definitions of instruction formats, names and properties for assembler,
  disassembler and debugger;
- definitions of register names and properties for assembler, disassembler and
  debugger;
- selection of predefined features, such as endianness, presence of certain
  processor options or instructions for compiler, debugger C library and OS
  kernels;
- macros with instruction sequences for saving and restoring special, user or
  coprocessor registers for OS kernels.

This configuration is provided in form of source files, that must replace
corresponding files in binutils, gcc, gdb or newlib source trees or be added
to OS kernel source tree. This set of files is usually distributed as archive
known as Xtensa configuration overlay.

Tensilica provides such an overlay as part of the processor download, however,
it needs to be reformatted to match the specific format required by the
crosstool-NG. For a script to convert the overlay file, and additional
information, please see [this link](http://wiki.linux-xtensa.org/index.php/Toolchain_Overlay_File)

The current version of crosstool-NG requires that the overlay file name has the
format xtensa_<CORE_NAME>.tar, where CORE_NAME can be any user selected name.
To make crosstool-NG use overlay file located at `<PATH>/xtensa_<CORE_NAME>.tar`
select XTENSA_CUSTOM, set config parameter `CT_ARCH_XTENSA_CUSTOM_NAME` to
`CORE_NAME` and `CT_ARCH_XTENSA_CUSTOM_OVERLAY_LOCATION` to `PATH`.

The fsf target architecture variant is the configuration provided by toolchain
components by default. It is present only for build-testing toolchain
components and is in no way special or universal.
