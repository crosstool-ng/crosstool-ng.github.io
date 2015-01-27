---
layout: docs
title: Installing crosstool-NG
permalink: /docs/install/
---

There are two ways you can use crosstool-NG:

-   build and install it, then get rid of the sources like you’d do for
    most programs,

-   or only build it and run from the source directory.

The former should be used if you got crosstool-NG from a packaged tarball,
see [Install method](#install-method), while the latter is most useful for
developers that use a clone of the repository, and want to submit patches,
see [“The Hacker’s way”](#hackers-way).

<a name="install-method"></a>
Install method
--------------

If you go for the install, then you just follow the classical, but yet easy
`./configure` way:

    ./configure --prefix=/some/place
    make
    make install
    export PATH="${PATH}:/some/place/bin"

You can then get rid of crosstool-NG source. Next create a directory to serve
as a working place, `cd` in there and run:

    mkdir work-dir
    cd work-dir
    ct-ng help

See below for complete usage.

<a name="hackers-way"></a>
The Hacker’s way
----------------

If you go the hacker’s way, then the usage is a bit different, although very
simple. First, you need to generate the `./configure` script from its
autoconf template:

    ./bootstrap

Then, you run `./configure` for local execution of crosstool-NG:

    ./configure --enable-local
    make

Now, **do not** remove crosstool-NG sources. They are needed to run
crosstool-NG! Stay in the directory holding the sources, and run:

    ./ct-ng help

See below for complete usage.

Now, provided you used a clone of the repository, you can send me your
changes. See the section [Contributing](#contrib) for how to submit changes.

<a name="package-prep">
Preparing for packaging
-----------------------

If you plan on packaging crosstool-NG, you surely don’t want to install it
in your root file system. The install procedure of crosstool-NG honors the
`DESTDIR` variable:

    ./configure --prefix=/usr
    make
    make DESTDIR=/packaging/place install

<a name="shell-completion"></a>
Shell completion
----------------

crosstool-NG comes with a shell script fragment that defines bash-compatible
completion. That shell fragment is currently not installed automatically,
but this is planned.

To install the shell script fragment, you have two options:

-   install system-wide, most probably by copying `ct-ng.comp` into
    `/etc/bash_completion.d/`, or

-   install for a single user, by copying `ct-ng.comp` into `${HOME}/`
    and sourcing this file from your `${HOME}/.bashrc`.

<a name="contributed-code"></a>
Contributed code
----------------

Some people contributed code that couldn’t get merged for various reasons.
This code is available as lzma-compressed patches, in the `contrib/`
sub-directory. These patches are to be applied to the source of crosstool-NG,
prior to installing, using something like the following:

    lzcat contrib/foobar.patch.lzma | patch -p1

There is no guarantee that a particular contribution applies to the current
version of crosstool-ng, or that it will work at all. Use contributions at
your own risk.
