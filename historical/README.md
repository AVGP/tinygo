Contents of this directory include the following:

src/pkg/runtime/tiny/
====
This is the last instance of runtime/tiny/ in the Go history. I pulled it from revision 6185df48facb.

https://code.google.com/p/go/source/detail?r=820f713f76f62b07c4df984125e104c7d165ade4
https://codereview.appspot.com/3996047

Here is the comment of the removal commit:

> runtime: remove tiny

> It is unmaintained and untested, and I think it's broken too.
> It was a toy to show that Go can run on real hardware,
> and it served its purpose.

> The source code will of course remain in the repository
> history, so it could be brought back if needed later.

> R=r, r2, uriel
> CC=golang-dev
> http://codereview.appspot.com/3996047
>
> Affected files
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

changes in runtime organization
====
At this 479bba71fb07, the organization of the runtime code was changed to the platform/architecture file naming convention instead of folders. This is important if you want to integrate the old tiny runtime code into the more recent code tree.

https://code.google.com/p/go/source/detail?r=479bba71fb0739f76ce3e3d0f63f1a78acb9e79c

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
> 
> R=r, iant, r, lucio.dere
> CC=golang-dev
> http://codereview.appspot.com/5490053
> 
> Affected files
>	Delete	/src/pkg/runtime/386/arch.h	diff
>	Delete	/src/pkg/runtime/386/asm.s	diff
>	Delete	/src/pkg/runtime/386/atomic.c	diff
>	Delete	/src/pkg/runtime/386/closure.c	diff
>	Delete	/src/pkg/runtime/386/memmove.s	diff
>	Delete	/src/pkg/runtime/386/vlop.s	diff
>	Delete	/src/pkg/runtime/386/vlrt.c	diff
>	Modify	/src/pkg/runtime/Makefile	diff
>	Delete	/src/pkg/runtime/amd64/arch.h	diff
>	Delete	/src/pkg/runtime/amd64/asm.s	diff
>	Delete	/src/pkg/runtime/amd64/atomic.c	diff
>	Delete	/src/pkg/runtime/amd64/closure.c	diff
>	Delete	/src/pkg/runtime/amd64/memmove.s	diff
>	Delete	/src/pkg/runtime/amd64/traceback.c	diff
>	Add	/src/pkg/runtime/arch_386.h	diff
>	Add	/src/pkg/runtime/arch_amd64.h	diff
>	Add	/src/pkg/runtime/arch_arm.h	diff
>	Delete	/src/pkg/runtime/arm/arch.h	diff
>	Delete	/src/pkg/runtime/arm/asm.s	diff
>	Delete	/src/pkg/runtime/arm/atomic.c	diff
>	Delete	/src/pkg/runtime/arm/closure.c	diff
>	Delete	/src/pkg/runtime/arm/memmove.s	diff
>	Delete	/src/pkg/runtime/arm/memset.s	diff
>	Delete	/src/pkg/runtime/arm/softfloat.c	diff
>	Delete	/src/pkg/runtime/arm/traceback.c	diff
>	Delete	/src/pkg/runtime/arm/vlop.s	diff
>	Delete	/src/pkg/runtime/arm/vlrt.c	diff
>	Add	/src/pkg/runtime/asm_386.s	diff
>	Add	/src/pkg/runtime/asm_amd64.s	diff
>	Add	/src/pkg/runtime/asm_arm.s	diff
>	Add	/src/pkg/runtime/atomic_386.c	diff
>	Add	/src/pkg/runtime/atomic_amd64.c	diff
>	Add	/src/pkg/runtime/atomic_arm.c	diff
>	Add	/src/pkg/runtime/callback_windows_386.c	diff
>	Add	/src/pkg/runtime/callback_windows_amd64.c	diff
>	Modify	/src/pkg/runtime/cgocall.c	diff
>	Add	/src/pkg/runtime/closure_386.c	diff
>	Add	/src/pkg/runtime/closure_amd64.c	diff
>	Add	/src/pkg/runtime/closure_arm.c	diff
>	Modify	/src/pkg/runtime/cpuprof.c	diff
>	Delete	/src/pkg/runtime/darwin/386/defs.h	diff
>	Delete	/src/pkg/runtime/darwin/386/rt0.s	diff
>	Delete	/src/pkg/runtime/darwin/386/signal.c	diff
>	Delete	/src/pkg/runtime/darwin/386/sys.s	diff
>	Delete	/src/pkg/runtime/darwin/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/darwin/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/darwin/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/darwin/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/darwin/defs.go	diff
>	Delete	/src/pkg/runtime/darwin/mem.c	diff
>	Delete	/src/pkg/runtime/darwin/os.h	diff
>	Delete	/src/pkg/runtime/darwin/signals.h	diff
>	Delete	/src/pkg/runtime/darwin/thread.c	diff
>	Add	/src/pkg/runtime/defs1_linux.go	diff
>	Add	/src/pkg/runtime/defs2_linux.go	diff
>	Add	/src/pkg/runtime/defs_arm_linux.go	diff
>	Add	/src/pkg/runtime/defs_darwin.go	diff
>	Add	/src/pkg/runtime/defs_darwin_386.h	diff
>	Add	/src/pkg/runtime/defs_darwin_amd64.h	diff
>	Add	/src/pkg/runtime/defs_freebsd.go	diff
>	Add	/src/pkg/runtime/defs_freebsd_386.h	diff
>	Add	/src/pkg/runtime/defs_freebsd_amd64.h	diff
>	Add	/src/pkg/runtime/defs_linux.go	diff
>	Add	/src/pkg/runtime/defs_linux_386.h	diff
>	Add	/src/pkg/runtime/defs_linux_amd64.h	diff
>	Add	/src/pkg/runtime/defs_linux_arm.h	diff
>	Add	/src/pkg/runtime/defs_netbsd.go	diff
>	Add	/src/pkg/runtime/defs_netbsd_386.h	diff
>	Add	/src/pkg/runtime/defs_netbsd_amd64.h	diff
>	Add	/src/pkg/runtime/defs_openbsd.go	diff
>	Add	/src/pkg/runtime/defs_openbsd_386.h	diff
>	Add	/src/pkg/runtime/defs_openbsd_amd64.h	diff
>	Add	/src/pkg/runtime/defs_plan9_386.h	diff
>	Add	/src/pkg/runtime/defs_windows.go	diff
>	Add	/src/pkg/runtime/defs_windows_386.h	diff
>	Add	/src/pkg/runtime/defs_windows_amd64.h	diff
>	Delete	/src/pkg/runtime/freebsd/386/defs.h	diff
>	Delete	/src/pkg/runtime/freebsd/386/rt0.s	diff
>	Delete	/src/pkg/runtime/freebsd/386/signal.c	diff
>	Delete	/src/pkg/runtime/freebsd/386/sys.s	diff
>	Delete	/src/pkg/runtime/freebsd/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/freebsd/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/freebsd/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/freebsd/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/freebsd/defs.go	diff
>	Delete	/src/pkg/runtime/freebsd/mem.c	diff
>	Delete	/src/pkg/runtime/freebsd/os.h	diff
>	Delete	/src/pkg/runtime/freebsd/signals.h	diff
>	Delete	/src/pkg/runtime/freebsd/thread.c	diff
>	Modify	/src/pkg/runtime/iface.c	diff
>	Delete	/src/pkg/runtime/linux/386/defs.h	diff
>	Delete	/src/pkg/runtime/linux/386/rt0.s	diff
>	Delete	/src/pkg/runtime/linux/386/signal.c	diff
>	Delete	/src/pkg/runtime/linux/386/sys.s	diff
>	Delete	/src/pkg/runtime/linux/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/linux/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/linux/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/linux/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/linux/arm/defs.h	diff
>	Delete	/src/pkg/runtime/linux/arm/rt0.s	diff
>	Delete	/src/pkg/runtime/linux/arm/signal.c	diff
>	Delete	/src/pkg/runtime/linux/arm/sys.s	diff
>	Delete	/src/pkg/runtime/linux/defs.go	diff
>	Delete	/src/pkg/runtime/linux/defs1.go	diff
>	Delete	/src/pkg/runtime/linux/defs2.go	diff
>	Delete	/src/pkg/runtime/linux/defs_arm.go	diff
>	Delete	/src/pkg/runtime/linux/mem.c	diff
>	Delete	/src/pkg/runtime/linux/os.h	diff
>	Delete	/src/pkg/runtime/linux/signals.h	diff
>	Delete	/src/pkg/runtime/linux/thread.c	diff
>	Modify	/src/pkg/runtime/malloc.goc	diff
>	Modify	/src/pkg/runtime/mcache.c	diff
>	Modify	/src/pkg/runtime/mcentral.c	diff
>	Add	/src/pkg/runtime/mem_darwin.c	diff
>	Add	/src/pkg/runtime/mem_freebsd.c	diff
>	Add	/src/pkg/runtime/mem_linux.c	diff
>	Add	/src/pkg/runtime/mem_netbsd.c	diff
>	Add	/src/pkg/runtime/mem_openbsd.c	diff
>	Add	/src/pkg/runtime/mem_plan9.c	diff
>	Add	/src/pkg/runtime/mem_windows.c	diff
>	Add	/src/pkg/runtime/memmove_386.s	diff
>	Add	/src/pkg/runtime/memmove_amd64.s	diff
>	Add	/src/pkg/runtime/memmove_arm.s	diff
>	Add	/src/pkg/runtime/memset_arm.s	diff
>	Modify	/src/pkg/runtime/mfinal.c	diff
>	Modify	/src/pkg/runtime/mfixalloc.c	diff
>	Modify	/src/pkg/runtime/mgc0.c	diff
>	Modify	/src/pkg/runtime/mheap.c	diff
>	Modify	/src/pkg/runtime/mprof.goc	diff
>	Modify	/src/pkg/runtime/msize.c	diff
>	Delete	/src/pkg/runtime/netbsd/386/defs.h	diff
>	Delete	/src/pkg/runtime/netbsd/386/rt0.s	diff
>	Delete	/src/pkg/runtime/netbsd/386/signal.c	diff
>	Delete	/src/pkg/runtime/netbsd/386/sys.s	diff
>	Delete	/src/pkg/runtime/netbsd/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/netbsd/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/netbsd/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/netbsd/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/netbsd/defs.go	diff
>	Delete	/src/pkg/runtime/netbsd/mem.c	diff
>	Delete	/src/pkg/runtime/netbsd/os.h	diff
>	Delete	/src/pkg/runtime/netbsd/signals.h	diff
>	Delete	/src/pkg/runtime/netbsd/thread.c	diff
>	Delete	/src/pkg/runtime/openbsd/386/defs.h	diff
>	Delete	/src/pkg/runtime/openbsd/386/rt0.s	diff
>	Delete	/src/pkg/runtime/openbsd/386/signal.c	diff
>	Delete	/src/pkg/runtime/openbsd/386/sys.s	diff
>	Delete	/src/pkg/runtime/openbsd/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/openbsd/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/openbsd/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/openbsd/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/openbsd/defs.go	diff
>	Delete	/src/pkg/runtime/openbsd/mem.c	diff
>	Delete	/src/pkg/runtime/openbsd/os.h	diff
>	Delete	/src/pkg/runtime/openbsd/signals.h	diff
>	Delete	/src/pkg/runtime/openbsd/thread.c	diff
>	Add	/src/pkg/runtime/os_darwin.h	diff
>	Add	/src/pkg/runtime/os_freebsd.h	diff
>	Add	/src/pkg/runtime/os_linux.h	diff
>	Add	/src/pkg/runtime/os_netbsd.h	diff
>	Add	/src/pkg/runtime/os_openbsd.h	diff
>	Add	/src/pkg/runtime/os_plan9.h	diff
>	Add	/src/pkg/runtime/os_windows.h	diff
>	Delete	/src/pkg/runtime/plan9/386/defs.h	diff
>	Delete	/src/pkg/runtime/plan9/386/rt0.s	diff
>	Delete	/src/pkg/runtime/plan9/386/signal.c	diff
>	Delete	/src/pkg/runtime/plan9/386/sys.s	diff
>	Delete	/src/pkg/runtime/plan9/mem.c	diff
>	Delete	/src/pkg/runtime/plan9/os.h	diff
>	Delete	/src/pkg/runtime/plan9/signals.h	diff
>	Delete	/src/pkg/runtime/plan9/thread.c	diff
>	Modify	/src/pkg/runtime/proc.c	diff
>	Add	/src/pkg/runtime/rt0_darwin_386.s	diff
>	Add	/src/pkg/runtime/rt0_darwin_amd64.s	diff
>	Add	/src/pkg/runtime/rt0_freebsd_386.s	diff
>	Add	/src/pkg/runtime/rt0_freebsd_amd64.s	diff
>	Add	/src/pkg/runtime/rt0_linux_386.s	diff
>	Add	/src/pkg/runtime/rt0_linux_amd64.s	diff
>	Add	/src/pkg/runtime/rt0_linux_arm.s	diff
>	Add	/src/pkg/runtime/rt0_netbsd_386.s	diff
>	Add	/src/pkg/runtime/rt0_netbsd_amd64.s	diff
>	Add	/src/pkg/runtime/rt0_openbsd_386.s	diff
>	Add	/src/pkg/runtime/rt0_openbsd_amd64.s	diff
>	Add	/src/pkg/runtime/rt0_plan9_386.s	diff
>	Add	/src/pkg/runtime/rt0_windows_386.s	diff
>	Add	/src/pkg/runtime/rt0_windows_amd64.s	diff
>	Modify	/src/pkg/runtime/runtime.h	diff
>	Modify	/src/pkg/runtime/sema.goc	diff
>	Add	/src/pkg/runtime/signal_darwin_386.c	diff
>	Add	/src/pkg/runtime/signal_darwin_amd64.c	diff
>	Add	/src/pkg/runtime/signal_freebsd_386.c	diff
>	Add	/src/pkg/runtime/signal_freebsd_amd64.c	diff
>	Add	/src/pkg/runtime/signal_linux_386.c	diff
>	Add	/src/pkg/runtime/signal_linux_amd64.c	diff
>	Add	/src/pkg/runtime/signal_linux_arm.c	diff
>	Add	/src/pkg/runtime/signal_netbsd_386.c	diff
>	Add	/src/pkg/runtime/signal_netbsd_amd64.c	diff
>	Add	/src/pkg/runtime/signal_openbsd_386.c	diff
>	Add	/src/pkg/runtime/signal_openbsd_amd64.c	diff
>	Add	/src/pkg/runtime/signal_plan9_386.c	diff
>	Add	/src/pkg/runtime/signal_windows_386.c	diff
>	Add	/src/pkg/runtime/signal_windows_amd64.c	diff
>	Add	/src/pkg/runtime/signals_darwin.h	diff
>	Add	/src/pkg/runtime/signals_freebsd.h	diff
>	Add	/src/pkg/runtime/signals_linux.h	diff
>	Add	/src/pkg/runtime/signals_netbsd.h	diff
>	Add	/src/pkg/runtime/signals_openbsd.h	diff
>	Add	/src/pkg/runtime/signals_plan9.h	diff
>	Add	/src/pkg/runtime/signals_windows.h	diff
>	Modify	/src/pkg/runtime/sigqueue.goc	diff
>	Modify	/src/pkg/runtime/slice.c	diff
>	Add	/src/pkg/runtime/softfloat_arm.c	diff
>	Modify	/src/pkg/runtime/stack.h	diff
>	Modify	/src/pkg/runtime/string.goc	diff
>	Modify	/src/pkg/runtime/symtab.c	diff
>	Add	/src/pkg/runtime/sys_darwin_386.s	diff
>	Add	/src/pkg/runtime/sys_darwin_amd64.s	diff
>	Add	/src/pkg/runtime/sys_freebsd_386.s	diff
>	Add	/src/pkg/runtime/sys_freebsd_amd64.s	diff
>	Add	/src/pkg/runtime/sys_linux_386.s	diff
>	Add	/src/pkg/runtime/sys_linux_amd64.s	diff
>	Add	/src/pkg/runtime/sys_linux_arm.s	diff
>	Add	/src/pkg/runtime/sys_netbsd_386.s	diff
>	Add	/src/pkg/runtime/sys_netbsd_amd64.s	diff
>	Add	/src/pkg/runtime/sys_openbsd_386.s	diff
>	Add	/src/pkg/runtime/sys_openbsd_amd64.s	diff
>	Add	/src/pkg/runtime/sys_plan9_386.s	diff
>	Add	/src/pkg/runtime/sys_windows_386.s	diff
>	Add	/src/pkg/runtime/sys_windows_amd64.s	diff
>	Add	/src/pkg/runtime/syscall_windows.goc	diff
>	Add	/src/pkg/runtime/thread_darwin.c	diff
>	Add	/src/pkg/runtime/thread_freebsd.c	diff
>	Add	/src/pkg/runtime/thread_linux.c	diff
>	Add	/src/pkg/runtime/thread_netbsd.c	diff
>	Add	/src/pkg/runtime/thread_openbsd.c	diff
>	Add	/src/pkg/runtime/thread_plan9.c	diff
>	Add	/src/pkg/runtime/thread_windows.c	diff
>	Modify	/src/pkg/runtime/time.goc	diff
>	Add	/src/pkg/runtime/traceback_amd64.c	diff
>	Add	/src/pkg/runtime/traceback_arm.c	diff
>	Add	/src/pkg/runtime/vlop_386.s	diff
>	Add	/src/pkg/runtime/vlop_arm.s	diff
>	Add	/src/pkg/runtime/vlrt_386.c	diff
>	Add	/src/pkg/runtime/vlrt_arm.c	diff
>	Delete	/src/pkg/runtime/windows/386/callback.c	diff
>	Delete	/src/pkg/runtime/windows/386/defs.h	diff
>	Delete	/src/pkg/runtime/windows/386/rt0.s	diff
>	Delete	/src/pkg/runtime/windows/386/signal.c	diff
>	Delete	/src/pkg/runtime/windows/386/sys.s	diff
>	Delete	/src/pkg/runtime/windows/amd64/callback.c	diff
>	Delete	/src/pkg/runtime/windows/amd64/defs.h	diff
>	Delete	/src/pkg/runtime/windows/amd64/rt0.s	diff
>	Delete	/src/pkg/runtime/windows/amd64/signal.c	diff
>	Delete	/src/pkg/runtime/windows/amd64/sys.s	diff
>	Delete	/src/pkg/runtime/windows/defs.go	diff
>	Delete	/src/pkg/runtime/windows/mem.c	diff
>	Delete	/src/pkg/runtime/windows/os.h	diff
>	Delete	/src/pkg/runtime/windows/signals.h	diff
>	Delete	/src/pkg/runtime/windows/syscall.goc	diff
>	Delete	/src/pkg/runtime/windows/thread.c	diff
