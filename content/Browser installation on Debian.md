+++
title = "Browser installation on Debian"
date = 2024-08-01

[taxonomies]
categories = ["Ubuntu"", "Linux"]
tags = ["Browser", "Chrome"]
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

Ad