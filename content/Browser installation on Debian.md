+++
title = "Browser installation on Debian and its derivatives"
date = 2024-08-01

[taxonomies]
categories = ["Ubuntu", "Linux", "Debian", "Browser"]
tags = ["Google Chrome", "Chrome", "Brave"]
+++

This guide is for installing Chrome and Brave browsers on Debian and Debian based Linux distros.

<!-- more -->

# Chrome

To install Google Chrome via your `apt` package manager, you need to add it to your repository list first.

Copy the following commands to download the signing key for Chrome:

```bash
sudo wget https://dl-ssl.google.com/linux/linux_signing_key.pub -O /tmp/google.pub
```

Create a new keyring for Chrome:

```bash
gpg --no-default-keyring --keyring /etc/apt/keyrings/google-chrome.gpg --import /tmp/google.pub
```

Add Chrome to your `apt` repository list:

```bash
echo 'deb [arch=amd64 signed-by=/etc/apt/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main' | sudo tee /etc/apt/sources.list.d/google-chrome.list
```

Update your apt repository and install Google Chrome:

```bash
sudo apt update && sudo apt install google-chrome-stable -y
```

You can also go to Chrome download page and download the `.deb` file and install it using this command:

```bash
sudo dpkg -i <path_to_your_downloaded_chrome_deb_file>
```

# Brave

You need to install `curl` if you don't already have it:

```bash
sudo apt isntall curl -y
```

Download the Brave signing key using curl:

```bash
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
```

Copy the signing key into your keyring:

```bash
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list
```

Update your apt repositories and then install the Brave application:

```bash
sudo apt update && sudo apt install brave-browser -y
```

You can install most of the browsers from Flatpak or Snap stores depending upon your choices. I prefer this method of installation because it will update whenever I run `sudo apt upgrade` command, and this is purely personal preference. It does not mean that the other methods are bad or wrong.

# Conclusion

Adding Chrome or any other type of packages like this into your apt repository is convenient because every time you update your system applications using `sudo apt upgrade`, it includes all these third party applications that you registered. So you don't have to manually download a `.deb` file and reinstall every time there is a new version.
