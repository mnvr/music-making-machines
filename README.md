# Music Making Machines

In which I describe how to make Music Making Machines.

To keep this as accessible as possible, the illustrations use the freely
available and open source VCV Rack<small>†</small>. The same techniques can be
used in any medium that allows direct and basic manipulation of signals (modular
hardware, other visual patching softwares, music programming languages etc).

<small>
† using stock plugins as far as possible; if other ones are necessary only other
open source and freely available ones are used.
</small>

## Meta

### Play

Musical instruments take <i>decades</i> to learn how to operate. These
techniques are no different. At the core of it, there really isn't much to say -
signals modulate signals, it's signals all the way down. However there is a gulf
between words and putting them into action that can only be crossed by doing,
there is no other substitute.

Play it with when you feel like. Or setup a daily practice routine. Lasseiz
faire or with discipline, just play. And keep at it.

### Embrace limitations

Limitations are not your enemy, they are your friend. They delineate the
playground you're playing in, so that you can then start to focus on and get in
the flow of playing, instead of wondering which game you are playing.

### Others

Not all of these work for everyone, and not all the time.

-   **Keep a patch journal**. Writing about music is like dancing about food. It
    doesn't compute. Still, there is value in being immersed in what one is doing,
    and writing about it is one way. And there is nothing special about writing:
    any ritual that you wish can be substituted if it makes you reflect and engage
    more with what you're doing.

-   **Use a fixed patch**. If you're feeling overwhelmed, fix on a base template
    patch for a while, and restrict yourself to exploring the possibilities
    offered by this particular setup without introducing the extra dimension of
    which modules to use.

## Seq of seq

### Sequencer

The main tool in our arsenal is the sequencer. If we were doing sound design,
then our main tool would be, say, the oscillator or the filter, and those two
are still important, but the heart of machines that make music is a sequencer.

> [!NOTE]
>
> An oscillator can also serve as a sequencer, especially if quantized, but
> we're getting into advanced territory here.

### Sequencing the sequencer

The fact that the sequencer itself can be sequenced makes it easier for us to
make music making machines (in contrast to making music itself, where this fact
would not be of much importance).

This is not to say that music making machines were not possible before. A
windchime is a great example of a music making machine. It uses a quantizer to
convert the noise signal provided by wind to notes on a musical scale, with the
wind also serving as the background noise in which the notes sit.

## VCV Rack specifics

### All signals are voltages

Any output can be connected to any input; all cables carry (virtual) voltages.

However, purely semantically, there are distinctions:

-   **Audio** signals are audible when played through speakers. They contain
    _audio-rate_ frequencies typically between 20Hz to 20kHz.

-   **CV** (control voltage) signals modulate parameters of other modules. e.g. an
    LFO (low frequency oscillator) can oscillate the pitch of a VCO
    (voltage-controlled oscillator) or the volume level of a VCA
    (voltage-controlled amplifier).

-   **1V/oct** (1 volt per octave) signals are control voltages, that is, CV
    signals, that represent a pitch or a note. In this standard, an increase of 1V
    increases the pitch by 1 octave. Since there are 12 semitones in an octave, an
    increase in 1/12 V increases the pitch by 1 semitone.

-   **Gate** signals carry an on/off signal. 0V represents off, and a positive
    voltage (typically 10V) represents on. For example, a gate signal can turn on
    whe a key is pressed and off when the key is released.

-   **Trigger** signals are short gates, usually around 1 millisecond, that cause
    an event to occur, such as a percussion hit.

-   **Clock** signals are triggers played at a steady tempo.

Viewed as a tree:

-   Audio
-   CV
    -   1V/oct
    -   Gate
        -   Trigger
            -   Clock

Input and output voltages carried by patch cables are thus in the (virtual)
voltage unit, V. Both oscillators and CV generators typically produce 10 Vpp
(peak-to-peak) signals, but audio outputs are usually **±5V**, while CV sources
are either **0 to 10V** (unipolar CV) or **±5V** (bipolar CV).

## Pitches and frequencies

Frequency knobs in audio-rate oscillators typically use a baseline of C4 (middle
C, MIDI 60, `f0 = 261.6256 Hz`) at their default position. This can be offset by
an input voltage V which would use the 1 V/oct (volt-per-octave) standard for CV
control of frequency information. Thus the offset frequency in terms of the
input voltage V would be `f = f0 * 2 ^ V`, where `f0` is the aforementioned
baseline C4.

LFOs and clock generators should use 120 BPM (`f0 = 2 Hz`) as their default.

### Patches are text files

Almost. They're compressed folders containing the patch JSON, alongwith any wav
files that the patch uses.

```sh
tar xvzf my-patch.vcv
more patch.json
```

### Stop the engine

There isn't a way to stop the engine, but as a workaround <kbd>Cmd - A</kbd>
(Select all modules) and <kbd>Cmd - E</kbd> (Bypass).
