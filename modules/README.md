## 8vert

8vert outputs 10V,

![8vert outputs 10V](i/8vert10v-1.png)

As many times as you want.

![8vert has multiple outputs](i/8vert10v-2.png)

Not just 10V, but anything you want.

![8vert copies its first input](i/8vert10v-3.png)

As much of it as you want,

![8vert allows attenuating its inputs](i/8vert10v-4.png)

Phase shifted if you want.

![8vert allows phase shifting its inputs](i/8vert10v-5.png)

Of many kinds if you want.

![8vert restarts the ladder on each input](i/8vert10v-6.png)

8vert can output any volts, not 10 if you want.

![8vert allows arbitrary constant voltage outputs](i/8vert10v-7.png)

8 attenuverters make 8vert, making this the 8th fact about it, and let's move
on.

## Oct

Oct outputs 1V.

![Octave outputs 1V when octave shifted by 1](i/oct1v-1.png)

But it can also output 4.

![Octave outputs then number of volts it is set to octave shift](i/oct1v-2.png)

And it can also output 8.

![Octave's octave shift can be modulated](i/oct1v-3.png)

The octave shift (the yellow dot, plus the first input jack) will be literally
added to the "input" (second jack). That's all that Octave does.

![Octave's is a literal adder](i/oct1v-4.png)

Don't believe me? [Read the source](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/Octave.cpp#L41)!

```cpp
int octave = octaveParam /* yellow dot in the panel */
           + round(inputs[OCTAVE].voltage); /* voltage of first jack */
float pitch = inputs[PITCH].voltage; /* voltage of second jack */
pitch += octave; /* The "magic" */
output[PITCH].voltage = pitch; /* voltage of third (output) jack */
```

## Mix

Mix adds voltages.

![Mix adds voltages](i/mix-1.png)

**8vert** and **Oct** both allow us to create constant voltages, including 1.
Oct allows us to add a constant voltage to a (variable) voltage. We have all the
Peano axioms we need recreate arithmetic, but it did take Russell & Whitehead
300 pages to define plus. Who has time for 300 pages these days, we want our
addition, we want it now.

Mix allows us to add two variable voltages, completing the triad.

| Rack  | Math  |
|-------|-------|
| 8vert | 1     |
| Oct†  | 1 + a |
| Mix   | a + a |

<small>† Oct also allows us to add two variable ones, if we're okay with
rounding one of them. Although one man's rounding is another man's
quantizer...</small>

But if we just want to add the same thing to itself, we don't necessarily need
to duplicate it, we can just drag _two_ cables from the output jack to the
inputs.

![Stackable outputs](i/mix-2.png)

This ability, called **Stackable outputs**, allows us to stack multiple cables
on an output jack (or "ports", as Rack calls them). Which leads one to wonder:
can we stack multiple cables on an input?

We can. And it does what you'd expect it to - it'll add all the voltages up.

![Stackable inputs](i/mix-3.png)

> [!NOTE]
>
> The mischievious amongst you are already thinking of it, but no, we cannot do
> both simultaneously for the same port pair - we can't drag two cables from the
> same output to the same input.

