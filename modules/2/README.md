**Part Deux**

# M for Modules

[Audio](#audio) • [Poly](#poly) • [VCA Mix](#vca-mix)

## Audio

VCV Rack comes with stock modules of two types.

1. Core modules are modules that are included in VCV Rack itself because they
  deal with I/O (audio and MIDI).

2. Fundamental modules are modules provided by the Fundamental plugin that also
   comes with VCV Rack.

This walkthrough is a tour of the Fundamental modules, but from the first
category there is one, and only one, that we will need for our purposes: Audio
(or more specifically, Audio 2).

All it does is send its input out through
your speakers. Might be useful.

![Audio 2 plays the audio signal sent to its L/MON input](i/audio-1.png)

> [!TIP]
>
> Actually, there is another core module that we've been using all along, to
> scribble our graffiti on. Notes!

The (virtual) voltages travelling in the cables between modules in Rack can be
anything a float can hold (~±10<sup>38</sup> in magnitude though not in
precision), but individual modules can (and sometimes do) choose to clip their
inputs or outputs.

In particular, Audio clamps the values it gets to lie between ±10V, and then
rescales them them to ±1 before sending them out to device.

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

After that brief break to talk about Audio and Poly, let us get back to where we
were: VCA.

What happens when you put four VCAs together? Well, you get the VCA Mix.

That's one way of looking at it: 4 x VCA = VCA Mix. But we can arrive at it from
a different direction too:

* Mix was for unity mixing 8 signals, with only a manual control for the output level.

* CV Mix was for mixing 3 signals, with only a manual control for their respective
  input levels.

* VCA Mix is the granddaddy. It can unity mix 4 signals, with manual _and_ CV
  controllable control for their respective input levels, outputs for the
  respective 4 signals after this gain stage, a manual and CV controllable
  control for the output level, and the output after the mix.

![Mix, CV Mix and VCA Mix](i/vca-mix-1.png)

---

[← Part 1](../)
