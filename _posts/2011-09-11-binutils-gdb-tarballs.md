---
title: Binutils and GDB tarballs renamed
---
[Binutils](http://sourceware.org/binutils/) and [GDB](http://sourceware.org/gdb/) have
recently renamed their tarballs due to a GPL compliance issue, and the old tarballs
are now missing. The binutils guys were kind enough to provide *legacy* symlinks
so that scripts using the old versions continue to work; the issue has been reported
[here](http://sourceware.org/ml/gdb/2011-09/msg00002.html) and
[here](http://sourceware.org/ml/gdb/2011-09/msg00030.html) to gdb as well. This means
that crosstool-NG releases so far choke when trying to download gdb. A fix
to crosstool-NG to use the new versions is coming soon, but in the meantime, you'll
have to manually [download](ftp://ftp.gnu.org/pub/gnu/gdb/) the tarballs, and rename
them to the old names.
