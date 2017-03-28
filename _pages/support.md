---
layout: page 
title: Support
permalink: /support/
topnav: true
---

## Reporting issues and wishlist items

Please report all the issues with building or using the toolchain at
[the project site on GitHub](https://github.com/crosstool-ng/crosstool-ng/issues).

Make sure that the report includes the following:
- the version of crosstool-NG you are using as reported by `ct-ng version`;
- the host OS, including the version;
- for MacOS, also indicate whether you are using Homebrew or Macports (and list
  the packages installed)
- the `.config` file you are using (unless it is an unmodified sample that ships
  with crosstool-NG - in which case you can just state the name of the sample
  configuration);
- the `build.log` file that is produced, if applicable (i.e. unless crosstool-NG
  fails before the `ct-ng build` step).

If crosstool-NG breaks during the `ct-ng build` phase, please retry using a
`ct-ng build.1` command. If it also fails, please attach the `build.log` from 
`ct-ng build.1` run (this command forces a non-parallel build, so the build takes
longer but the log file is much easier to read). If `ct-ng build` fails but
`ct-ng build.1` succeeds, please attach *both* `build.log` files.

Please *attach* any files, do not *paste* them into the issue description.

## Mailing list

The mailing list is at [crossgcc@sourceware.org](mailto:crossgcc@sourceware.org).
Archive and subscription info can be found here:
[https://sourceware.org/ml/crossgcc/](https://sourceware.org/ml/crossgcc/)

## International Relay Chat (IRC)

Use channel #crosstool-ng on FreeNode (irc.freenode.net)

## Deprecation policy

Starting with the 1.23 release, the following package version deprecation policy is in effect:

1. The packages are not removed until they have been marked *OBSOLETE* for one release.
2. The packages that are marked *OBSOLETE* will likely be removed in the next release. A special
   policy applies to the Linux and GNU libc versions: the versions used by still supported major
   releases from distributions (CentOS, Ubuntu) are kept until the End-Of-Life date
   of the respective release.
3. If you feel that a certain version of a package needs to be retained, please file an issue
   at [the project site on GitHub](https://github.com/crosstool-ng/crosstool-ng/issues) and describe
   the reason. Due to limited manpower the project has, it is not guaranteed though.

In general, this means if you need to use particular versions of packages, you're likely stuck
with a particular version of crosstool-NG as well. They might stop working at some time,
unfortunately (the host compiler version can change after an upgrade, resulting in build
errors; or the download URLs may go invalid; etc). Again, due to limited manpower, we
don't generally do dot-releases (e.g. 1.22.1). If you really need a certain version of
crosstool-NG to work, please consider stepping up to maintaining that branch.
