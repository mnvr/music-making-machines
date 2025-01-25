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
