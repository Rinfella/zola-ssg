+++
title = "Installing kitty terminal"
date = 2024-08-01

[taxonomies]
categories = ["Linux", "Ubuntu", "Debian", "Kitty"]
tags = ["Kitty", "Terminal Emulator"]
+++

Kitty terminal emulator installation instructions foe Debian and Debian-based distors.

<!-- more -->

# Install kitty using apt

The default `apt` package manager has `kitty` by default and you can simply install it my running the following command:

```bash
sudo apt install kitty -y
```

You can confirm the kitty installation by running `kitty` from your current terminal.
Kitty terminal should launch as a new window if your installation is correct.

You can make `kitty` the default terminal by running the following command:

```bash
sudo update-alternatives --config x-terminal-emulator
```

It will prompt you to select a number, like the image shown below:

![[Pasted image 20240801183435.png]]

Choose the corresponding number you want (in my case i want number 2).

# Configuration

Kitty follows the conventional `XDG` directory systems, so you can store your configurations in the `$XDG_COMFIG_HOME`, i.e. `~/,config` .

Make a new directory called `kitty` under `~/.config`:

```bash
mkdir -p ~/.config/kitty
```

Create a config file called `kitty.conf` under the `kitty` directory:

```bash
nano ~/.config/kitty/kitty.conf
```

Now you can input any customizations or configurations here.
Here is a sample configuration file:

```
# Font Famiily:

font_family Fira Code
italic_font auto
bold_font auto
bold_italic_font auto

# Font Size:
font_size 14.0

# COLORS:
foreground #44cfad
background #181a1b
background_opacity 0.9

```

This is just a sample configuration for starter. You can deep dive into the [documentations](https://sw.kovidgoyal.net/kitty/conf/) for more detailed guide on how to configure your `kitty` terminal.

Kitty is one of the most popular GPU-based terminal emulator among Linux users and has one of the best features like image rendering support, many kitten modules, etc.

# Conclusion

This is just the tip of the iceberg into customizing your Linux user-interface.
Since the terminal is one of the most important applications in your Linux system, it is important that you have a good experience with it.
You can also check out another alternatives like [Alacritty](https://alacritty.org/), [Warp](https://www.warp.dev/linux-terminal), etc.
