# R for Rack

[Keyboard shortcuts](#keyboard-shortcuts) • [Tips](#tips)

## Keyboard shortcuts

Right click (or <kbd>ENTER</kbd>) opens the module browser. The focus is already
in the search bar, so typing the first few characters of the module name and
pressing <kbd>ENTER</kbd> adds it it to our patch.

Dragging creates a cable.

<kbd>Shift + click</kbd> deletes the topmost cable.

<kbd>Cmd + drag</kbd> creates a new cable _from the  jack_.

![Cmd and drag creates a new cable from the jack](i/key-1.png)

<kbd>Cmd + shift + drag</kbd> creates a new cable _from the jack where the
topmost cable is from_.

![Cmd and shift and drag creates a new cable from where the topmost cable came from](i/key-2.png)

Note how in this case Rack will retain the cable color (In case of <kbd>Cmd +
drag</kbd> it auto rotates the color, unless you've turned it off in the View menu).

## Tips

### Stop the engine

There isn't a way to stop the engine, but as a workaround <kbd>Cmd - A</kbd>
(Select all modules) and <kbd>Cmd - E</kbd> (Bypass).
