tinygo
======

Private clone of the tinygo repository (hg) at https://code.google.com/p/tinygo/

Here is the description at the time of my clone:

> This project is dead. Here's what it was:

> This is a patch that made Go's tiny runtime more capable. With this patch applied on top of your Go source tree, you could catch interrupts, and compile many more of the standard libraries.

> In the Go distribution, the Tiny runtime is just a toy to demo how to run Go on raw hardware. Adding more complications to it would obscure the minimum things it is trying to show. So instead, we'll maintain this patch separately.

I have become interested in OS/low-level software and my primarily language is Go, so I am creating my own branch of tinygo. The original project appears to be abandoned, so I'm not too worried about creating confusion or conflicts in the community.

As my primary source for guidance, I refer to osdev.org. Understandably so, osdev.org promotes C as the language of choice for operating system design. I nevertheless make mention of it so you understand why I do some of the things I do.
