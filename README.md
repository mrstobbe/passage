Passage
=======

This is just an experiment. Well, several experiments mixed into one:
* Non-threaded async I/O (socket oriented mostly) using the best polling method
  available while keeping events consitent from edge-triggered down to classic
  `select()`.
* Provision of arbitrary events within the I/O pooling system that
  appropriately blocks for the right amount of time while still relying on
  kernel I/O events to flow through. Basically, synthetic events (timeout/
  interval) should piggyback on the system's I/O polling method without
  sacraficing trigger resolution.
* Cascading fd capabilities (sockets have different capabilities than pipes or
  files, and even sockets of different types have major differences).
* Graceful handling of what should be graceful, and full exception bubble on
  what is a serious error.
* Last, but no where near least, further exploration into C++11. You need a
  modern (probably gcc) compiler to even play with this. For the last couple of
  years I've been obsessed with learning the ins-and-outs of C++11. This is no
  different.
* Elegance of all of these things tied together.


Project Goals
-------------
* Modernity: An essentially complient C++11 compiler is required (best get
  yourself gcc or mingw).
* Backwards compatibility: `select()` is still the worst case fallback.
* Stability: No unexpected but handlable FD error should cause a panic.
* Security: Things like starvation should not be possible (even at the expense
  of efficiency... you other projects claming rediculous throughput because you
  ignore things like starvation know who you are).
* Common tasks highly abstracted: "tying" two descriptors together creates a
  tunnel in one call, listening on an arbitrary address with arbitrary
  protocol details because a call with a string parameter that can come
  directly from a config file without validation, sub-process pipe management
  becomes no different than TCP socket management, etc.
* Process management: Services (posix=forking, windows=service), handle
  passing, thread management, etc, all abstracted and distilled. These are
  common needs of most modern I/O oriented services. This is not an
  experimental aspect of Passage, but one I know pretty well by now. It's just
  something I'd like to see built in.


Not Project Goals
-----------------
* An end product: I'm not building something actually useful. It may become
  that, but it's not an explict goal.
* Efficiency: CPU cycles/branching/kernel ticks/resident set size are not my
  concerns. Even if this weren't an experiment, they should be post-thoughts
  with optimization coming later. Everything's optimizable later. For the most
  part I've been approaching this little project sacraficing memory for speed.


Why Are You Bothering?
----------------------
Why not? It's an interesting project with a very unique mix of goals. Most
people don't approach async I/O like this. They want compatibility and/or
usability and/or speed. This is just an experiment ignoring many of the normal
goals.


Is This Usable For Anything?
----------------------------
Maybe. Acually, probably. First, I've split it's generic components (event
pooling, I/O handles, etc) into a library which, by default, is a static build.
The includes are specific to the library and treated as such. This should be
usable in any project and even system installable (although no install
definitions exist yet).

Second, there's a simple tunneling example (src/bin/passaged.cc) that does a
one-to-one tunnel of tcp (or unix) requests. It's just a test program and very
bare bones, but it's logic and error handling should actually be pretty stable.
It should give you a comprehensive understanding of what's going on so far in
Passage (after all even a simple async connection oriented bound pair is more
complex than many servers by themselves). If you understand socket programming,
POSIX systems, and C++11, you should have no problem understanding where this
project is headed and how it might be useful to you (even in the unstable
stage).

Third, keep in mind that this one of my (many; usually non-public) playgrounds.
This means that it could be stable one day and not the next. I won't even
bother branching or tagging until this is no longer my playground because
there's a demand. If you want to use this, you should find the best commit for
you needs, patch as needed, and call it good (contribute back while you're at
it?).


Passage: The Name
-----------------
History... "What are synonyms for 'tunnel'?"


Building
--------
Requires cmake 2.8 (maybe earlier?), and linux (maybe?). Probably that's it.
Within the project root directory (NOT src!)...

	$ mkdir build
	$ cd build
	$ cmake ..
	$ make

You're done (there aren't any install directives).


