# Music Making Machines

Notes about making Music Making Machines.

The specific software used is freely available and open source VCV
Rack<small>†</small>. The same techniques can be used in any medium that allows
basic but direct manipulation of signals (modular hardware, other visual
patching softwares, music programming languages etc).

<small>
† using stock plugins as far as possible; if other ones are necessary only other
open source and freely available ones are used.
</small>

## Meta

- **Play**. Musical instruments take <i>decades</i> to learn how to
  operate. These techniques are no different.

- **Embrace limitations**. Limitations delineate the playground we're playing
  in, allowing us to focus on and get in the flow of playing.

- **Keep a patch journal**. Writing about music is like dancing about food.
  Still, there is value in being immersed in what one is doing, and writing is
  one common way. Not the only way though, _there can be other rituals too_.

## Steps

1. Spend time with the stock modules.
    - Their [documentation](https://vcvrack.com/Free) is great.
    - Here is a [quirkier walkthrough](modules/README.md).

2. Recreate some patches others have made.
    - TODO

## Thoughts

### Sequencer

The heart of music making machines is often a sequencer. This is in contrast to
if we were doing sound design, where the focus would be on the oscillator or the
filter, or if we were making music itself, where the musician would be the sequencer.

Everything can be modulated by a sequencer, including the sequencer itself.

> [!NOTE]
>
> An oscillator can also serve as a sequencer, especially if quantized.

However, one doesn't necessarily have to use clocks or a sequencer. A windchime
is a great example of a music making machine that has no clocks. It uses a
quantizer to convert the noise signal provided by wind to notes on a musical
scale, with the wind also serving as the background noise in which the notes
sit.

## VCV Rack specifics

Reference: [VCV Rack Manual](https://vcvrack.com/manual/).

### All signals are voltages

Any output can be connected to any input; all cables carry (virtual) voltages.

However, purely semantically, there are distinctions:

- **Audio** signals are audible when played through speakers. They contain
  _audio-rate_ frequencies typically between 20Hz to 20kHz.

- **CV** (control voltage) signals modulate parameters of other modules. e.g. an
  LFO (low frequency oscillator) can oscillate the pitch of a VCO
  (voltage-controlled oscillator), or the volume level of a VCA
  (voltage-controlled amplifier).

- **1V/oct** (1 volt per octave) signals are control voltages, that is, CV
  signals, that represent a pitch or a note. In this standard, an increase of 1V
  increases the pitch by 1 octave. Since there are 12 notes in an octave, an
  increase in 1/12 V increases the pitch by 1 note.

- **Gate** signals carry an on/off signal. 0V represents off, and a positive
  voltage (typically 10V) represents on. For example, a gate signal can turn on
  when a key is pressed and off when the key is released.

- **Trigger** signals are short gates, typically 1 millisecond, that cause an
  event to occur, such as a triggering a sound.

- **Clock** signals are triggers played at a steady tempo.

Viewed as a tree:

- Audio
- CV
  - 1V/oct
  - Gate
    - Trigger
      - Clock

> [!TIP]
>
> Remember that everything is a voltage. (Most) modules are not about "audio" or
> "CV". It only matters whether a signal is at audio rate when trying to move to
> a speaker. Before that, at any point, it is only voltages.

> [!WARNING]
>
> While we just emphasized that everything is a voltage, there is one
> implementation distinction between "audio" voltages and "CV" voltages. The VCV
> VCO uses anti-aliasing to avoid introducing harmonics beyond the Nyquist
> frequency, so the waveforms it generates exhibit the Gibbs effect (i.e. the
> square waves are not exactly square but wiggle at the edges).
>
> The VCV LFO doesn't use anti-aliasing. Thus, the LFO is more suited for
> generating CV for modulation, while the VCO is more suitable for generating
> audible waveforms.

Input and output voltages carried by patch cables are thus in the (virtual)
voltage unit, V. Both oscillators and CV generators typically produce 10 Vpp
(peak-to-peak) signals, but audio outputs are usually **±5V**, while CV sources
are either **0 to 10V** (unipolar CV. e.g. this is the default for the VCV LFO)
or **±5V** (bipolar CV; LFO emits this when OFST switch is off).

## Pitches and frequencies

Frequency knobs on VCO typically default to C4 (middle C, MIDI 60, `f0 =
261.6256 Hz`). This can be offset by an input voltage V which would use the 1
V/oct (volt-per-octave) standard for CV control of frequency information. Thus
the offset frequency in terms of the input voltage V would be `f = f0 * 2 ^ V`,
where `f0` is the aforementioned baseline C4.

LFOs and clock generators typically default to 120 BPM (`f0 = 2 Hz`).

### Patches are text files

Almost. They're compressed folders containing the patch JSON, alongwith any wav
files that the patch uses.

```sh
tar xvzf my-patch.vcv
more patch.json
```

> [!TIP]
>
> I know what you're thinking. Since they are JSON files, maybe I can write a
> program to generate them...

### Stop the engine

There isn't a way to stop the engine, but as a workaround <kbd>Cmd - A</kbd>
(Select all modules) and <kbd>Cmd - E</kbd> (Bypass).
