---
title: Moving to Github
---
I'm sure you're probably still catching up on all the updates and call
to arms on the list of patches in patchwork, but I have one more
announcement to make.

I was able to get our code hosted on Github:
https://github.com/crosstool-ng/crosstool-ng

Over the next month, as we clear out patchwork, I'd like contributors
to start "forking" crosstool-ng and opening "pull requests" to get
code into mainline for crosstool-ng.

# Why Github
For one, it's widely used in other open-source projects and keeps me
away from being an "BDFL" (Benevolent dictator for life). Top
contributors can now be added as "Project contributors" that can
commit code themselves.

Secondly, there is an issue tracker!!!
https://github.com/crosstool-ng/crosstool-ng/issues
I'm no good at tracking issues on the mailing list. If I forget about
the issue, it will hide on the mailing list until someone (the issue
reporter) gets angry at me. Tracking the issue from the time it is
reported until it is resolved will help crosstool-ng become a better
tool!

There are many other reasons this better then what we currently have,
but for now this is a good start.

# Why not X (bitbucket, gitorious, etc..)
I've used them all. I prefer github, and so do most other people.

# How is this transition going to work?
I personally have two remotes in my local git clone of crosstool-ng.
My origin is currently the new github repository, and I have another
remote for the crosstool-ng.org repository.

As we clear out patchworks, I will be applying those changes to
crosstool-ng.org and then pushing those to github. As new pull
requests come in from github, I will help work the pull requests into
the github repository and push those changes to the crosstool-ng.org
repository.

When patchwork is empty and there are no more patches to be worked
there, I will turn off patchwork and the crosstool-ng.org git
repository.

There might still be need for the server crosstool-ng.org is currently
on, so that might stick around for a while.

Let me know if you have any questions or comments.

Cheers,  
-Bryan
