Boblight for Arch Linux ARM / Raspberry Pi
================

Overview
--------

This is my personal edit of the original boblight software which can be found at: https://code.google.com/p/boblight/

I updated the v4l client code to work with the newer ffmpeg (>0.8) libraries provided in Arch Linux.

Added some additional optimizations from [Speedy1985's Boblight repository](https://github.com/Speedy1985/boblightd-for-raspberry).  This code means that using the standard boblight.conf will no longer work and you must use 3 character naming for lights as per the example boblight.conf inlcuded here.

I have also fixed a few other V4L related bugs as reported on the [boblight issues list](https://code.google.com/p/boblight/issues/detail?id=49).

Installing
----------

Assuming you are starting with a base image of Arch Linux for Raspberry Pi (ArchArm) you will need to install the following dependencies using Pacman to compile this code:

git, gcc, make, libx11, libxrender, portaudio, libxext, mesa, glu, ffmpeg

This can be done with the following command:

`pacman -Sy git gcc make libx11 libxrender portaudio libxext mesa glu ffmpeg`

To install: `./configure && make && make install`

If you want v4l support use `./configure --with-v4l`

After installing it create a boblight.conf in /etc as per the official documentation: [Boblight Config](https://code.google.com/p/boblight/wiki/boblightconf)

Create a new ld config file in /etc/ld.so.conf.d/ as usr-local.conf and add `/usr/local/lib` to it.

Run `ldconfig`

Create a systemd service for the boblight daemon if you are running the damon on this Pi, for example:

`vi /usr/lib/systemd/system/boblight.service`

```[Unit]
Description=Boblight Ambient Lighting Daemon
DefaultDependencies=no
After=network.target

[Service]
ExecStart=/usr/local/bin/boblightd
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

`systemctl enable boblight`

`systemctl start boblight`

At this point the boblight daemon and all clients should be working.
