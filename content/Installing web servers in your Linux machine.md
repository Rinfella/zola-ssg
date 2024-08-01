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

All of your virtual host configs must be inside `/etc/apache2/sites-available` and if you want to enable them, run :
```bash
```a2ensite your_config.conf`.

This `a2ensite` command will create a symlink of your config file into the `/etc/apache2/sites-enabled` directory of your Apache config. You must enable only the config you want to use. You can disable unwanted config files by running `a2dissite your_config.conf`.

You can also enable other Apache modules by running the command: `a2enmod module_name`.
Eg: `a2enmod rewrite, a2enmod proxy`

You need to restart your `apache2` service every time you modify a configuration.
You can do this by 
