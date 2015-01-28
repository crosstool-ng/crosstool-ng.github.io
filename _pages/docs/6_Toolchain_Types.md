---
layout: docs
title: Toolchain Types
permalink: /docs/toolchain-types/
---

There are four kinds of toolchains you could encounter.

First off, you must understand the following: when it comes to compilers
there are up to four machines involved:

1.  the machine configuring the toolchain components: the **config**
    machine
2.  the machine building the toolchain components: the **build** machine
3.  the machine running the toolchain: the **host** machine
4.  the machine the toolchain is generating code for: the **target**
    machine

We can most of the time assume that the config machine and the build
machine are the same. Most of the time, this will be true. The only time
it isn’t is if you’re using distributed compilation (such as distcc).
Let’s forget this for the sake of simplicity.

So we’re left with three machines:

-   build
-   host
-   target

Any toolchain will involve those three machines. You can be as pretty
sure of this as “2 and 2 are 4”. Here is how they come into play:

1.  build == host == target (**“native”**)

    This is a plain native toolchain, targeting the exact same machine
    as the one it is built on, and running again on this exact same
    machine. You have to build such a toolchain when you want to use an
    updated component, such as a newer gcc for example.

2.  build == host != target (**“cross”**)

    This is a classic cross-toolchain, which is expected to be run on
    the same machine it is compiled on, and generate code to run on a
    second machine, the target.

3.  build != host == target (**“cross-native”**)

    Such a toolchain is also a native toolchain, as it targets the same
    machine as it runs on. But it is build on another machine. You want
    such a toolchain when porting to a new architecture, or if the build
    machine is much faster than the host machine.

4.  build != host != target (**“canadian”**)

    This one is called a “Canadian Cross”¹ toolchain, and is tricky.
    The three machines in play are different. You might want such a
    toolchain if you have a fast build machine, but the users will use
    it on another machine, and will produce code to run on a third
    machine.

**crosstool-NG** can build all these kinds of toolchains. (Or is aiming at it,
anyway!)

------------------
¹   The term Canadian Cross came aboot because at the time that these
    issues were all being hashed out, Canada had three national
    political parties
    [(per Wikipedia)](http://en.wikipedia.org/wiki/Cross_compiler#Canadian_Cross).
