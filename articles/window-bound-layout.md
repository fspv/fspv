# Window bound layout for Linux + i3

Most of you have probably never noticed this issue, but there is no built-in ability to bind keyboard layout to a window in Xorg. It is usually provided by the desktop environment + window manager you're using. It might be Gnome, Unity, KDE, etc.

However, there are some that don't have such a useful functionality built-in. Like the one, I'm using, [i3wm](https://i3wm.org/). I really love that window manager as it makes coding and navigation without a mouse so much easier. But anyway, due to its simplicity, some things are required to be implemented from scratch. The ability to bind keyboard layout to the window is one of them.

## Installation

Here is how I implemented it in my setup.

First of all, you'll need `i3ipc` python module. You can get it from pip. It is convenient to use virtualenv for that.

https://gist.github.com/prius/946b6bca2bc0cafb4514cde6cd5a4c20

Then you have to compile `xkblayout-state` utility from this repo https://github.com/nonpop/xkblayout-state

https://gist.github.com/prius/aa1c09a825a76feec7017841ab681247

You'll get `xkblayout-state` binary as a result. Put this binary wherever you like so that you can use it later (I put it under `~/.config/i3/scripts/`, for example).

Now your environment is set up to run the script that will store the layout values for every window and switch it when you jump from one window to another.

And here is the script:

https://gist.github.com/prius/28c858771b0b9ee6eea29e49dc702c61

There are just a couple of steps left.

First, you have to create a bash script that will activate virtualenv and then run the python script.

I put this scipt under `${HOME}/.config/i3/scripts/window-bound-layout.sh`.

https://gist.github.com/prius/85b3f52a83adda23729e4fdd631cb430

And finally, add to the i3 config.

https://gist.github.com/prius/8ef227c06f8bcc050d94ec7713022783

Here you go. Now you have to log out and log in again into your user, and you'll get a working window-bound layout.

The only thing is that if you want to use layouts different from English and Russian, you might need to modify the code slightly. But I'm pretty sure you'll manage that if you come this far.

## Debugging
For debugging just run the sh script in your terminal. It will start producing a lot of output.
