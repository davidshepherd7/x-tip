# Xtip

[![travis](https://travis-ci.org/davidshepherd7/xtip.svg?branch=master)](https://travis-ci.org/davidshepherd7/xtip)

A semi-clone of [tanin47's tip](https://github.com/tanin47/tip) but for X11 (i.e. Linux).

Quickly run auto-completed commands on selected text. For example:

* Google translate some text
* Convert a unix timestamp to ISO format
* Open a path in your text editor
* Google search some text

In comparison to a browser extension this works from *any* program.

Currently alpha: anything may change at any time.

Customised in python (but it's very easy to run shell commands from python if
you prefer another language).


## Installation and setup

* Install the core dependencies: `zenity`, `dmenu`, `xclip`
  * e.g. on Debian-based OS: `sudo apt install zenity xclip suckless-tools`
* Install xtip from pypi, e.g. `pip3 install xtip`
* Save x-tip.py into a directory on your `$PATH` and `chmod +x x-tip.py`
* Bind a hotkey to it using your preferred method. 
  * I bound it to `super + right click` using [sxhkd](https://github.com/baskerville/sxhkd)
  * I think most desktop environments have a hotkey binding system. If you get it working using something else it would be great if you could add documentation.
* Optionally install dependencies for any individual commands that you want to use:
  * GoogleTranslate requires the python googtrans library
  * Emacsclient requires emacs (obviously)


## Writing your own commands


For most customisation of commands you should probably just write your own
(because it only takes a few lines of python). You can write new commands in
`~/.config/xtip/custom_commands.py`.

To do so: write a new class derived from `Command` and decorate it with
`@command`, for example:


```
from typing import Optional
from subprocess import run

from xtip.commands import Command, command


@command
class Emacsclient(Command):
    unique_name = "Open in emacsclient"

    def run(self, text: str) -> Optional[str]:
        # TODO(david): Figure out a way to get the absolute path... maybe by
        # guessing from a few possible prefixes?
        run(["emacsclient", "-c", "-n", text])

        # Return text from here to output it to the screen and clipboard
        return None

    def accepts(self, text: str) -> bool:
        # TODO: only accept things that look like valid paths?
        Return True
```

TODO: Do we need both inheritence and a decorator? Probably not!


## TODO

* Write some tests
* Try to display useful outputs in dmenu completion (e.g. converted datetimes)
* Something better than dmenu? Better mouse support, popup at cursor.
* Figure out how to construct an absolute path from a relative one
