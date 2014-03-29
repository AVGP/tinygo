Contents of this directory include the following:

tiny
====
This is the last instance of tiny in the Go history. I pulled it from commit 6185df48facb. The next commit, 820f713f76f6, is when the code was removed entirely. Here is the comment of the removal commit:

> runtime: remove tiny

> It is unmaintained and untested, and I think it's broken too.
> It was a toy to show that Go can run on real hardware,
> and it served its purpose.

> The source code will of course remain in the repository
> history, so it could be brought back if needed later.

> R=r, r2, uriel
> CC=golang-dev
> http://codereview.appspot.com/3996047
> Affected files     expand all   collapse all
>	Modify	/src/pkg/runtime/Makefile	diff
>	Delete	/src/pkg/runtime/tiny/386/defs.h	diff
>	Delete	/src/pkg/runtime/tiny/386/rt0.s	diff
>	Delete	/src/pkg/runtime/tiny/386/signal.c	diff
>	Delete	/src/pkg/runtime/tiny/386/sys.s	diff
>	Delete	/src/pkg/runtime/tiny/README	diff
>	Delete	/src/pkg/runtime/tiny/arm/defs.h	diff
>	Delete	/src/pkg/runtime/tiny/arm/rt0.s	diff
>	Delete	/src/pkg/runtime/tiny/arm/signal.c	diff
>	Delete	/src/pkg/runtime/tiny/arm/sys.s	diff
>	Delete	/src/pkg/runtime/tiny/bootblock	diff
>	Delete	/src/pkg/runtime/tiny/dot-bochsrc	diff
>	Delete	/src/pkg/runtime/tiny/io.go	diff
>	Delete	/src/pkg/runtime/tiny/mem.c	diff
>	Delete	/src/pkg/runtime/tiny/os.h	diff
>	Delete	/src/pkg/runtime/tiny/runtime_defs.go	diff
>	Delete	/src/pkg/runtime/tiny/signals.h	diff
>	Delete	/src/pkg/runtime/tiny/thread.c	diff

