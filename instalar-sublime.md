---
title: "INSTALAR SUBLIME"
tags: ""
---

The apt repository contains packages for both x86-64 and arm64.

Install the GPG key:

```shell
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

Ensure apt is set up to work with https sources:

```shell
sudo apt-get install apt-transport-https
```
Select the channel to use:

Stable

```shell
    echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
```


Update apt sources and install Sublime Text

```shell
sudo apt-get update
sudo apt-get install sublime-text
```
