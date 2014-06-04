original tiny
----
This is the last instance of src/pkg/runtime/tiny/ in the Go history.

https://code.google.com/p/go/source/detail?r=820f713f76f62b07c4df984125e104c7d165ade4
https://codereview.appspot.com/3996047

Here is the comment of the removal commit:

> runtime: remove tiny

> It is unmaintained and untested, and I think it's broken too.
> It was a toy to show that Go can run on real hardware,
> and it served its purpose.

> The source code will of course remain in the repository
> history, so it could be brought back if needed later.

changes in runtime organization
----
At this 479bba71fb07, the organization of the runtime code was changed to the platform/architecture file naming convention instead of folders. This is important if you want to integrate the old tiny runtime code into the more recent code tree.

https://code.google.com/p/go/source/detail?r=479bba71fb0739f76ce3e3d0f63f1a78acb9e79c
https://codereview.appspot.com/5490053

Here is the comment of the removal commit:

> runtime: make more build-friendly
> 
> Collapse the arch,os-specific directories into the main directory
> by renaming xxx/foo.c to foo_xxx.c, and so on.
> 
> There are no substantial edits here, except to the Makefile.
> The assumption is that the Go tool will #define GOOS_darwin
> and GOARCH_amd64 and will make any file named something
> like signals_darwin.h available as signals_GOOS.h during the
> build.  This replaces what used to be done with -I$(GOOS).
> 
> There is still work to be done to make runtime build with
> standard tools, but this is a big step.  After this we will have
> to write a script to generate all the generated files so they
> can be checked in (instead of generated during the build).

Regarding 'bootblock'
----
The file 'bootblock', according to the README, is from a project call xv6. It does have source, but it doesn't appear to be included with golang. Please consider the following site and repository:

http://pdos.csail.mit.edu/6.828/2012/xv6.html
git://pdos.csail.mit.edu/xv6/xv6.git

2014/06/04: Moving forward, I will be integrating the source for bootblock into the repository. I've been frustrated with it not being included from the beginning. Even if I never modify it, anyone following in my footsteps is going to want to see the source, license, and documentation associated with it as I have.

