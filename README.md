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

Finally, delete the .tar.gz with: `cd /opt && sudo rm -f Python-3.8.7.tgz`. You can also do the same with the extracted folder.

Here are a few PIP packages you need:

`pip install keras_applications`, `pip install keras_preprocessing`, `pip install -U --user six wheel mock` `pip install pybind11`, `pip install h5py`

We will now upgrade `setuptools` with: `pip install --upgrade setuptools`

# Step 2: Install the Tensorflow Wheel File (This will take a while, roughly 1hr on my 8 core Mediatek MT8183)

We will use the wheel files graciously built by QEngineering. They must've gone through so much pain building as Bazel has to be downright one of the worst build systems that exist. If you want, there are tutorials online to building your own wheel file but they take hours (>5hrs) due to how slow most ARM devices are. Also, you will have to do pretty desperate things like creating a swap file on a USB drive because you need >8GB of RAM to build.

Here is the link for a 64bit OS: https://github.com/Qengineering/TensorFlow-Raspberry-Pi_64-bit
Here is the link for a 32bit OS: https://github.com/Qengineering/TensorFlow-Raspberry-Pi

Find the .whl file for the Tensorflow version you want (as well as OS) and download it with `wget <link_here>`. Then install it with: `pip install <name_of_package.whl>` of course, without "<" and ">". This will take a long time, but eventually it will install.

# BUT WAIT!

When Tensorflow is done installing, when you try to run it, you will get a complaint about `glibc`. This can be fixed by installing a verison of `glibc` >2.29. To do this, you must add the Debian `testing` repo to `/etc/apt/sources.list`. 

First, run `sudo nano /etc/apt/preferences` and paste this into the file:

`Package: *
Pin: release a=stable
Pin-Priority: 900

Package: *
Pin: release a=testing
Pin-Priority: 650`

Then press ctrl+x to exit and press `y` to save.

After that, run `sudo nano /etc/apt/sources.list` and add `deb https://deb.debian.org/debian testing main contrib` near the other sources.

To finish things, run `sudo apt-get update` and finally `sudo apt install -t testing libc6`

You should now have a newer version of `glibc`

# Test and enjoy!

To test Tensorflow, run `python` in your shell to open the Python shell. Then type `import tensorflow as tf` and press enter. After that, running `print(tf.__version__)` in the shell should return your Tensorflow version.

# Enjoy!
