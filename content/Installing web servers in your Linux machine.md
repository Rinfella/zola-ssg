+++
title = "Installing web servers in your Linux machine"
date = 2024-08-01

[taxonomies]
categories = ["Web Server", "Linux"]
tags = ["Apache", "Nginx", "OpenLitespeed"]
+++

Basic instructions on how to install and configure a couple of web servers in Debian and Debian based Linux distros. Nginx, Apache, OpenLitespeed..

<!-- more -->

We will first look into Apache web server, how to install and configure it:

# Apache

On Debian and its family, Apache web server is by default present in the `apt` repository by the name `apache2`. You can install it by running:

```bash
sudo apt install apache2 -y
```

By default, on Debian based systems, the configuration files for Apache locates under `/etc/apache2` every modifications you need to do must be done here.

All of your virtual host configs must be inside `/etc/apache2/sites-available` and if you want to enable them, run `aa2ensite your_config.conf`
