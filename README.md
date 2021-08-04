# Tensorflow 2.5.0 on ARM: (ARMv8 ARMv7)
A guide to getting Tensorflow up and running on your ARM workstation

# Introduction
So you just got an ARM device? Now you want to get Tensorflow working? Well oof! Tough luck, PIP doesn't have a build for ARM so we have to do this ourselves. In this guide I will be using Debian Buster on a Lenovo Duet through the Crostini VM. This guide will work on pretty much any Debian/Ubuntu based distro including Raspberry Pi OS on the Rapsberry Pi.

# Step One: Getting dependencies and stuff!
Start off with a basic upgrade:
`sudo apt-get update` and `sudo apt-get upgrade`

Now we will install dependencies. Be sure you have any previous Tensorflow installations deleted with `pip uninstall tensorflow`

Here is the apt-get command for dependencies:
`sudo apt-get install gfortran libhdf5-dev libc-ares-dev libeigen3-dev libatlas-base-dev libopenblas-dev libblas-dev openmpi-bin libopenmpi-dev liblapack-dev cython build-essential libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev wget tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev`

Before we get started with PIP packages, we need Python 3.8 specifically or the Tensorflow wheel will not install.

Enter your `/opt` directory:
`cd /opt`

Download the tarball:
`sudo wget https://www.python.org/ftp/python/3.8.7/Python-3.8.7.tgz`

Extract:
`sudo tar xzf Python-3.8.7.tgz`

Enter your Python directory:
`cd Python-3.8.7`

Configure build:
`sudo ./configure --enable-optimizations`

Build!:
`sudo make -j$(nproc) install`

Now we need to update alternative Python installations:
`update-alternatives --config python`

Select Python 3.8

Here are a few PIP packages you need:

`pip install keras_applications`, `pip install keras_preprocessing`, `pip install -U --user six wheel mock`
