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

Kitty follows the conventional `XDG` directory systems