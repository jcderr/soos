# SOOS

Soos is a framework for working with docker images in a development environment. It serves as an example for how to build
your own, rather than a drop-in system. The example is a [python-django](http://djangoproject.com) project. The idea is to
provide some project-workflow scripting that abstracts away the myriad flags, args, and concepts of docker itself. Convention
is preferred over configuration, but you can customize the scripts to your own needs. Internally at [Djed](http://djed.com/),
many of these scripts take a handful of optional arguments.

## Requirements

* VirtualBox
* Vagrant
* Docker

# How It Works

    bin/soos up

Launch a new vagrant box, set up docker, and set docker to listen on `tcp://0.0.0.0:2375` so our scripts
can talk to it.

    bin/soos build

Completely rebuild the docker images at the base of our app from scratch, using `Dockerfile`.

    bin/soos shell
    
Start a shell using the base container.
