**Part Deux**

# M for Modules

[Audio](#audio) • [Poly](#poly) • [VCA Mix](#vca-mix)

## Audio

VCV Rack comes with stock modules of two types.

1. Core modules are modules that are included in VCV Rack itself because they
  deal with I/O (audio and MIDI).

2. Fundamental modules are modules provided by the Fundamental plugin that also
   comes with VCV Rack.

This document is a tour of the Fundamental modules, but from the first category
there is one, and only one, that we will need for our purposes.

Audio (or more specifically, Audio 2). All it does is send its input out through
your speakers. Might be useful.

![Audio 2 plays the audio signal sent to its L/MON input](i/audio-1.png)

> [!TIP]
>
> Actually, there is another core module that we've been using all along, to
> scribble our graffiti on. Notes!

## Poly

So far we've talked of voltages as real numbers carried by a cables, but cables
can also carry vectors! Such cables are called **Polyphonic cables**, and the
vectors they carry can be of length up to 16.

All the operations that the stock modules are doing can also be done on these
entire vectors in one go.

The modules **Merge**, **Split**, **Sum** and **Viz** are for converting normal
cables into polyphonic ones (or vice versa) and visualizing their contents.

> [!TIP]
>
> When starting out, for a while we can just pretend these don't exist, to
> reduce the number of modules in our palette.

## VCA Mix

---

[← Part 1](../)
