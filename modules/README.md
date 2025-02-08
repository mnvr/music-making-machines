# M for Modules

[8vert](#8vert) • [Oct](#oct) • [Mix](#mix) • [CV Mix](#cv-mix) • [VCA](#vca) •
[Part 2](2/)

## 8vert

8vert outputs 10V,

![8vert outputs 10V if nothing is patched to its input](i/8vert10v-1.png)

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

![Octave outputs the number of volts it is set to octave shift](i/oct1v-2.png)

And it can also output 8. The octave shift itself can be modulated!

![Octaves can be chained together, and their octave shift can also be modulated](i/oct1v-3.png)

The octave shift (the yellow dot, plus the first input jack) will be literally
added to the "input" (second jack). That's all that Octave does.

![Octave's is a literal adder](i/oct1v-4.png)

Don't believe me? [Read the source](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/Octave.cpp#L41)!

```cpp
int octave = octaveParam              /* yellow dot in the panel */
           + round(inputs[OCTAVE].voltage) /* voltage of first jack */
float pitch = inputs[PITCH].voltage   /* voltage of second jack */
pitch += octave                       /* The "magic" */
output[PITCH].voltage = pitch;        /* voltage of third (output) jack */
```

## Mix

Mix adds voltages.

![Mix adds voltages](i/mix-1.png)

8vert and Oct both allow us to create constant voltages, in particular 1. Oct
allows us to add a constant voltage to a (variable) voltage. We have all the
Peano axioms we need recreate arithmetic!

But it did take Russell & Whitehead 300 pages to define "+" if they started with
just 1 and the successor function. Who has time for 300 pages these days; we
want our addition, and we want it now.

Mix allows us to add two variable voltages, completing the triad without
resorting to recreating arithmetic.

| Rack  | Math  |
|-------|-------|
| 8vert | 1     |
| Oct<sup>†</sup> | 1 + a |
| Mix   | a + a |

> <sup>†</sup> Oct also allows us to add two variable ones, if we're okay with
> rounding one of them. Although one man's rounding is another man's quantizer..

But if we just want to add the same thing to itself, we don't necessarily need
to duplicate it, we can just drag _two_ cables from the output jack to the
inputs.

![Stackable outputs](i/mix-2.png)

This ability, called **Stackable outputs**, allows us to stack multiple cables
on an output jack (or "ports", as Rack calls them). Which leads one to wonder:
can we stack multiple cables on an input? Can we have **Stackable inputs**?

We can. And it does what you'd expect it to - it'll add all the voltages up.

![Stackable inputs](i/mix-3.png)

> For those wondering, no, we can't drag two cables from the same output to the
> same input.

So both of them doing the exact same thing? Yes. This is how [stackable
inputs](https://github.com/VCVRack/Rack/blob/4a7ad1e1e781f2e858e2c8b04867e9665fecc1f1/src/engine/Engine.cpp#L383-L401)
do their thing:

```py
for (each cable of cables)
    inputVoltage += cable.outputVoltage
```

while this is how
[Mix](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/Mixer.cpp#L58-L61)
does it:

```cpp
for (int i = 0; i < 6; i++)
    outputVoltage += inputs[i].voltage
```

So both are literally adding all the inputs voltages up. This is what is
sometimes referred to as a "unity mix" - the inputs are added up without any
gain or attenuation (i.e. with _unit_ gain) applied to them or to the result.
This is useful behaviour for various semantics that might be attached to the
voltages under consideration:

* For audio signals we get an equally weighted mix of all the input sounds.

* For CVs this linear combination allows each input to equally offset the
  modulated parameter (note that voltages can be negative too).

* For 1V/oct pitches, the notes are transposed.

* For gates, it emulates a logical "OR" - the output gate is "on" whenever any
  of the input gates are.

Mix is still useful though.

For starters, stacking cables on an input port to add their voltages up, while
convenient, can make patches "harder to read". Using a Mix is more explicit,
though detractors will say it makes patches more "verbose". Programming language
syntax debates are not limited to text.

Secondly, Mix allows us to set the output level. That is, after adding all the
inputs up, it can attenuate the result before setting the voltage on the output
jack.

![Mix allows setting the output LEVEL](i/mix-4.png)

It allows us to multiply a voltage by -1 (Right click, "invert output").

![Mix allows setting the output LEVEL](i/mix-4.png)

Not
sure how this new operator (`f(x) = x * -1`) adds to our Peano arithmetic, but when
applied to a control voltage, this will reverse its effect, while for an audio
signal, it will invert its phase.

In the last sentence, notice the different semantic effect multiplying by -1 has
on the voltage. The code is literally [multiplying by
-1](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/Mixer.cpp#L48):

```cpp
if (invert)
    gain *= -1;
```

but it does different things depending on what the cable is carrying. In most
cases, the conventions around what the voltages in the cable mean, and the
operations we can do to them using modules, have evolved so that the effect will
be something useful, but there is nothing guaranteeing it. For example, if cable
is carrying a gate, then inverting it is usually not going to be what we want.

Anyways, so both these things - changing the output level, including inverting
it - can already be done by an 8vert. Maybe a Mix is more convenient, but it
isn't doing something we already cannot, right?

> [!TIP]
>
> Our attempt at factoring out functionality is only to gain familiarity with
> these modules, but in practice there are many ways to cut the cake. Factors
> like ergonomics, rack space, and the context in which a operation is needed:
> all these give rise to modules that are not "one way to do things" building
> blocks but chimeras with overlap in functionality.

Turns out, Mix does have a new trick up its sleeve - it allows averaging the
inputs.

![Mix has an option to average voltages at the outputs](i/mix-5.png)

This is handy for CV, since then the output modulation is in the same range as
the input modulations.

## CV Mix

CV Mix also outputs 10V.

![An unpatched CV mix outputs 10V when the corresponding level is
100%](i/cv-mix-1.png)

But unlike 8vert, the inputs don't ladder through, but each input gets its own
independent 10V.

![Unlike 8vert, each CV input gets an independent 10V](i/cv-mix-2.png)

In the above example, the first input to CV mix is getting 1V from the Oct. The
second and third inputs are 10V, but since the second level knob is at 0, none
of that goes through. The third knob is at 50%, so 5V (50% of 10V) goes through.
The output of CV Mix is then the sum of all three scaled inputs, 1V + 0V + 5V,
which we can see in the scope.

OTOH, for the 8vert, the first input is 1V, overriding the default 10V, for
itself and all the inputs below, so the (second) output is also 1V.

> [!TIP]
>
> The interface for the modules indicates these distinctions: notice how the the
> CV Mix has a single line indicating independent 10V supplies in the unpatched
> state for all inputs, while the 8vert arrows indicate the cascade between
> successive inputs, with only the first one getting a 10V.
>
> ![Module interface has subtle cues on the lines and arrows to indicate the
> different 8vert and CV Mix laddering](i/cv-mix-3.png)

So CV Mix is like Mix in that it sums its inputs, but also like 8vert in that it
allows us to "attenuvert" (attenuate and/or invert) the inputs before they get
summed, and like 8vert also provides a default value 10V for unpatched inputs.

![8vert, CV Mix and Mix next to each other for easy comparison](i/cv-mix-4.png)

Or to put it the other way around, CV Mix provides input level control unlike
Mix which provides output level control; and CV Mix provides a single summed
output unlike 8vert which provides 8 independent outputs.

#### Why "CV"??

CV Mix also works with audio, despite its name. Or does it?

If we patch an audio mix through it, and through a combination of 8vert and Mix, but usually all we can say when someone
we seem to get the same result (the _OFST1_ on the scope in the example is just
to show that there really are two signals; without the offset they align
perfectly).

![The same signals, mixed via a combination of 8vert and Mix, and also via CV
Mix](i/cv-mix-5.png)

Let us see if we can spot a difference in the code. This is
[Mix](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/CVMix.cpp#L47-L59):

```c++
float out = 0.f;
for (int i = 0; i < 6; i++) {
    out += inputs[IN_INPUTS + i].getVoltageSimd(c);
}
out *= params[LEVEL_PARAM].getValue();
outputs[OUT_OUTPUT].setVoltage(out, 0);
```

> I've tweaked some variables etc in both cases to better point the
> similarities.

and this is [CV
Mix](https://github.com/VCVRack/Fundamental/blob/d1c9f6f1fe7e2f2f1fa85cf2da3ac798b86ed2de/src/CVMix.cpp#L47-L59):

```c++
float_4 out = 0.f;
for (int i = 0; i < 3; i++) {
  float_4 cv = inputs[IN_INPUTS + i].getNormalVoltageSimd<float_4>(10.f, c);
  cv *= params[LEVEL_PARAMS + i].getValue();
  out += cv;
}
outputs[OUT_OUTPUT].setVoltageSimd(out, 0);
```

There are some surface level differences – Mix has 6 inputs while CV Mix has 3 –
and some differences due to the module's interfaces – Mix has one gain control
and so the multiplication with the gain happens outside the loop, while CV Mix
has one gain control for each input so the multiplication happens inside.

There is also the difference in the function used to read the value - Mix uses
`getVoltageSimd`, while CV Mix uses `getNormalVoltageSimd`, which is defined as:

```c++
T getNormalVoltageSimd(T normalVoltage, uint8_t ch) {
    return isConnected() ? getVoltageSimd(ch) : normalVoltage;
}
```

However, because of what the default positions of the gain knobs are, this
doesn't make a difference in the initial state of the module – For Mix the level
knob defaults to 100% and any inputs which are not connected are taken to be 0;
For CV Mix any input which is not connected is taken to be 10 V, but since the
level knobs default to 0% it all comes out to be the unless we start twisting
the knobs.

Note that the question here is not if CV Mix is the same as Mix, or whether it
is redundant etc. We already mentioned that these modules are not meant to be
abstract, uniquely factored out building blocks but rather are shaped by the
evolutionary landscape of usefulness to making music. So obviously there are
differences in the two. CV Mix is tailored more for mixing, well, CV, while for
mixing audio we usually need control over the envelopes, which is handled by a
module we'll see soon, called VCA Mix.

The hypothetical question is – why is it called "CV" mix specifically? Can we
not use it to process audio were we to wish to do so?

Does the answer lies in a small detail in the above code: `float_4`?

`float_4` is a [vector of 4 floats](https://github.com/VCVRack/Rack/blob/4a7ad1e1e781f2e858e2c8b04867e9665fecc1f1/include/simd/Vector.hpp#L340):

```c++
using float_4 = Vector<float, 4>
```

which allows it to be processed upon by the faster [SIMD ops](https://github.com/VCVRack/Rack/blob/4a7ad1e1e781f2e858e2c8b04867e9665fecc1f1/include/simd/Vector.hpp#L38-L43):

```c++
union {
    __m128 v;
    /** Accessing this array of scalars is slow and defeats
        the purpose of vectorizing */
    float s[4];
}
```

This explains why CV Mix has 3 and not 6 inputs (it firstly doesn't need that
many, but even if it did, using more than 4 inputs would've required the 256 bit
SIMD datatype instead of the `__m128` that currently suffices), but efficiency
considerations aside, conceptually it is still an array of 4 floats.

So there is no difference in the data type either.

We handwaved a bit when we were visualizing with the scope, let us try that but
be precise this time. We can subtract the outputs of both paths from each other,
and if they are the exact same, we should see a 0V flatline.

That's what we try to do here. We use the combination of 8vert and Mix to first
invert one of the paths, and then add them. This'd could've been done with just
a single CV Mix too, but that would conflate the system under test with the test
harness so we use the longer approach.

![Comparing the delta in the output of 8vert + Mix vs CV Mix](i/cv-mix-6.png)

The line is not flat!

We don't need to rely on visuals either. If you look at the top of the scope, it
is telling us that the signal has a pp (peak to peak amplitude) of 0.19V.

Where is this difference coming from? Has our subversive blasphemy of using CV
Mix to mix audio voltages finally caught up with us?

To know the answer, one needs to know a bit of Rack lore. Each cable in the
signal path introduces a 1 sample delay!

> Each cable in Rack induces a 1-sample delay of its carried signal from the
> output port to the input port. This means that it is not guaranteed that two
> signals generated simultaneously will arrive at their destinations at the same
> time if the number of cables in each signal's chain is different.
>
> [VCV Rack Manual / Voltage Standards /
>   Timing]((https://vcvrack.com/manual/VoltageStandards#Timing))

In our case, the 8vert + mix combination has 1 extra cable in the signal path as
compared to going via CV Mix, so there is an extra sample delay, which manifests
itself as the phase shift we're seeing at the output.

There are two ways to fix this - we can add an extra cable in the CV Mix signal
path by adding a dummy module, say a Mix; or more simply, we can get rid of the
Mix from the 8vert + Mix combination, and instead just stack the inputs since
we're doing a unity mix anyway.

![Comparing the delta in the output of 8vert + stackable inputs vs CV Mix after ensuring the same number of cables in both signal paths](i/cv-mix-7.png)

Indeed, with this change, we see the 0V flatline. The signals are exactly the
same.

CV Mix can be used to mix audio too if such a need arises. Though for audio we
usually need something else anyway, building up to which is the module we're
going to look at next, the VCA.

## VCA

The VCA is like a volume knob.

![VCA can attentuate the level of an input](i/vca-1.png)

Except instead of turning it by hand, you can get a (control) voltage to do it
for you.

![VCA level can be controlled by a control voltage](i/vca-2.png)

With a control voltage of 0V being 0% level, and 10V being 100% level.

![VCA output vs the level control voltage](i/vca-3.png)

> [!NOTE]
>
> Notice how we're passing a fixed "control voltage" of 1 to the _IN_ of the VCA
> in these examples. That's what the detour in the last section was about -
> these particular modules don't care if what they get a control voltage or an
> audio signal, it is just voltages for them to mathematically manipulate, and
> they'll happily work with either.
>
> It is only in the context of a musical patch that there might be ergonomic and
> functional reasons for using CV Mix for control voltages and VCA (or VCA Mix)
> for audio voltages.

Since we can use a voltage to control the amplification level of the output,
this module is called a **voltage controlled amplifier** (or as friends call it,
a VCO).

As a convenience, we still get the manual control if we wish in addition to the
voltage controlled level. The input signal (the voltage at the input jack _IN_)
is multiplied by the voltage controlled level (the control voltage at unnamed
jack below the level on the display), and then multiplied by the manually
controlled level (the setting on the display), and the result is what gets set
as the voltage of the output jack _OUT_.

![VCA output level can be set both manually and via the level control voltage at the same time](i/vca-4.png)

Note the that setup in our example is uncommon. We're sending a fixed voltage as
the input to be modulated, while in practice this is going to be a varying audio
voltage (e.g. the output of an oscillator).

![VCA behaviour for a audio-ish input](i/vca-5.png)

In fact, just for illustration purposes, it might be useful to flip the order in
which the modules are arranged in our rack, to put the modulation signal (the
LFO) first. This has no functional impact, but it does give us a glimpse of how
for a VCA the "input" signal is always on, it is the modulation (the amount of
it that goes through) that is being controlled.

![VCA inputs arranged in a sometimes more natural to understand order](i/vca-6.png)

> [!TIP]
>
> For some folks this is a stumbling block because this is not how things behave
> in the real world. The guitar is not always playing, we need to pluck it for
> it to make a sound. On the other hand, in modular setups, the oscillators are
> always making a sound, the thing we're controlling is how much of it gets
> heard (if any).

Another thing you need to pay attention to if you're just reading this on auto
pilot is that the (yellow) sine wave we see in the Scope above is _not_ the
audio signal (the audio is a triangle wave coming out from the _TRI_ of the
oscillator via the red cable). That yellow sine wave is actually the level of
the audio, fluctuating between silence (0) and its maximum level (2.5V).

> How did we get 2.5V? Audio voltages by convention in Rack are between -5V and
> 5V, and that is also what the _TRI_ output voltage will be between. The fixed
> level set on the VCA display is 50%, which means that after the modulation
> we'll get a signal between -5V and 5V, which then will be further attenuated
> 50%, giving us a signal between -2.5V and 2.5V.

To see the actual audio, we need to zoom the scope to much smaller timescales.
Here I've swapped out the modulating control voltage (the blue in the examples
above) with the original input (red) since Scope can only show two signals at
once. The yellow is the audio output.

![Zooming into the VCA output instead of looking at the envelope](i/vca-7.png)

In both cases, the yellow curve is the actual audio output, but when we zoom
into it, we see the audio voltage itself, while previously when we set the scope
to longer timescales via the _TIME_ knob of the Scope, the trace of this very
same audio voltage is "squished" together in the limited pixels available on the
Scope, effectively giving us the an outline of how its level was varying over
time.

![Zooming out on the VCA output to look at the input and output envelope](i/vca-8.png)

These outlines are also called "envelopes", and we'll hear a lot of them (pun
intended) in the future.

> [!NOTE]
>
> In our VCA examples so far the modulation voltage is a varying voltage (in our
> example, also the output of an oscillator), and sometimes in practice indeed
> the modulation voltage is coming from a slow moving LFO, but often times is a
> one shot thing.
>
> Taking the guitar example we were talking about a while ago - a guitar note
> does not keep playing indefinitely, nor does it loop on and off. Instead what
> happens is that we pluck it, and the note plays for a while, and then stops.
> If we want similar type of behaviour in our music making machines (and we do),
> then a LFO as a modulation voltage won't work.
>
> What we need for such cases is an "envelope" generator, which is what would
> control the VCA instead of the LFO in our examples so far. But we're getting
> ahead of ourselves here...

The VCA has one more trick up its ~~sleeve~~ right click menu. Exponential
responses.

#### Exponential

The word exponential is one of those things that everyone knows, but no one
really knows what it exactly means in a particular context. Often, exponential
is used as a stand in for quadratic (x<sup>2</sup>) or cubic (x<sup>2</sup>),
and it is indeed infrequent for it to be used in a context where it matches its
mathematical definition (e<sup>x</sup>).

The most general statement we can deduce from such assertions about exponential
behaviour is that it is _not linear_.

The same is happening here. The exponential response is VCV Rack is not
e<sup>x</sup>, but is x<sup>4</sup>. For musical purposes, what we need to know
is that this is more, to quote the manual, "aggressive".

The scope shows what we mean (compare it to the normal output above).

![VCA with exponential response](i/vca-9.png)

And it is even easier to here if one patches the _OUT 2_ of the scope to an
Audio. Why, we can do both simultaneously, since we have stereo outputs on our
laptop and stereo inputs via our ears.

![Contrasting two VCAs with and without exponential response](i/vca-10.png)

Notice how the yellow bar in the VCA display is lower for the second VCA (an
asymmetry that carries forward to the VU meter LEDs of the Audio). This is a
screenshot of an arbitrary point in time, but no matter when you take the
screenshot, the second VCA's instantaneous level (which is what the yellow bar
on the display indicates) will always be less than or equal to the first one's.

This is just a property of the mathematical function that was chosen. Before
getting to the specific choice, let us reflect on the motivations.

Here is a linear function.

![Linear function](i/graph-1.png)

Many real life phenomena exhibit non-linear behavior. You don't get richer when
you get richer, you also start getting richer faster. So the graph of wealth vs
money added is not a straight line, it tends to grow faster the more money you
already have. Same happens with knowledge, popularity or even downfall.

Pertinent to us is that the envelopes of many natural sounds we hear also behave
similarly. _They don't behave linearly_.

They don't behave exactly exponentially either (whatever exponentially means).
Modelling the envelopes of sound is not a one trick pony; sounds have different
phases, each of which might be best modelled by a different type of a function,
including linear ones.

But that caveat aside, making the level of a sound rise exponentially is an easy
way to make them sound more natural or musically appropriate in certain contexts
(e.g. kick sounds). Linear just sounds too _slow_ for those cases.

> [!TIP]
>
> This might have to do with how we ourselves respond exponentially (using our
> wavy hand wavy definition) to volume - if we were to correlate the volume to
> say, the max level of the voltage, then the voltage needs to increase 3.1
> times or us to feel it as twice as loud.
>
> That's why the unit of audio levels is dB (decibels), but I feel I'm going on
> a tangent to a tangent here, so will stop at that.

What do we know is - (a) linear is not always enough to model increases in the
level of a sound, and (b)a more "spiky" increase sometimes sounds more natural.
So the question then is - which "spikier" function should we use.

Let us first try with a literal definition of an exponential function,
e<sup>x</sup>.

![Linear and exponential function](i/graph-2.png)

This doesn't work, at least not directly. Anything to the power of 0 is 1, so is
e<sup>0</sup>, and for a zero input we're already getting output 1, and after
that it'll start getting out of range.

> We've not defined our input and output ranges explicitly, so let's mention
> that. Our inputs will we values between 0 and 1. Our outputs will be values
> between 0 and 1. The task of our VCA then is to map the input (0V to 1V) to
> the output (0V to 1V).

This problem will be with any function of the form a<sup>x</sup>, so let us try
the other way - x<sup>a</sup>. Let's take the quadratic function, x<sup>2</sup>.

![Quadratic function](i/graph-3.png)

It does things like bounce off in the negative direction, but within our ranges
of interest, 0V to 1V, this does seem more promising, but it is still not
"spiky" enough maybe? That's easy to fix, let's try cubic (x<sup>3</sup>)
instead.

![Cubic function](i/graph-4.png)

Spikier harder you say? Let's try x<sup>100</sup>

![x raised to 100](i/graph-5.png)

We may have gone too far!

> [!NOTE]
>
> Anyways, the exact choice of the power (the _exponent_, if we're being more
> precise) is not set in stone, because there isn't an exact physical phenomena
> we're trying to model. By providing this "Exponential response" option, VCV
> Rack itself tries to model hardware modular gear which themselves try to model
> musically relevant sounds, so there is not a right answer here, just what is
> musically most useful in a generic enough manner.
>
> The VCA is often driven by ADSR module, which itself does not have exponential
> responses for the individual phases of the envelope, and instead relies on the
> VCA, so whatever choice of the exponent we make should not be too _aggressive_
> lest it is unusable in the non-attack phases of the envelope.
>
> But we haven't seen the ADSR here, so this may be gobbledegook, and you can
> safely ignore this aside.

The function VCV itself uses is x<sup>4</sup>.

![Quartic function](i/graph-6.png)

Trimming off the also rans, let us compare the normal VCA behaviour (linear,
blue) and the VCA behaviour when the "Exponential response" option in the right
click menu is turned on (x<sup>4</sup>, purple).

![VCV VCA - Linear vs exponential response](i/graph-7.png)

We can see that it rises slower (which it needs to do if we want to retain the
full dynamic range instead of peaking at 1 too early), with almost nothing
happening until the input is ~0.5V, and then suddenly starts to ramp up,
reaching 1 at the same time as its default linear counterpart.

The output for a particular input voltage will always be smaller (as compared to
the default linear response) when the exponential response is enabled, but
because of the sharper rise in the second half, the overall sound's rise will
feel more, um, just to use the same adjective as the manual, _aggressive_.

---

<div align="right">

[Part 2 →](2/)

</div>
