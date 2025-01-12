# Music Making Machines

In which I describe how to make Music Making Machines.

To keep this as accessible as possible, the illustrations use the freely
available and open source VCV Rack<small>†</small>. The same techniques can be
used in any medium that allows direct and basic manipulation of signals (modular
hardware, other visual patching softwares, music programming languages etc).

<small>
† using stock
plugins as far as possible; if other ones are necessary only other open source
and freely available ones are used.
</small>

## Meta

### Setup a daily practice

Musical instruments take <i>decades</i> to learn how to operate. These
techniques are no different. At the core of it, there really isn't much to say -
signals modulate signals, it's signals all the way down. However, to get these
signals to make music takes doing, there is no other substitute.

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

The thing that makes it easier for us to make music making machines (as compared
to making music) is the fact that the sequencer itself can be sequenced.

This is not to say that music making machines were not possible before. A
windchime is a great example of a music making machine. It uses a quantizer to
convert the noise signal provided by wind to notes on a musical scale, with the
wind also serving as the background noise in which the notes sit.

## VCV Rack specifics

### Patches are text files

Almost. They're compressed folders containing the patch JSON, alongwith any wav
files that the patch uses.

```sh
tar xvzf my-patch.vcv
more patch.json
```
