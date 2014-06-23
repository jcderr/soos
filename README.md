# SOOZ

Sooz is a framework for working with docker images in a development environment. It serves as an example for how to build
your own, rather than a drop-in system. The example is a [python-django](http://djangoproject.com) project. The idea is to
provide some project-workflow scripting that abstracts away the myriad flags, args, and concepts of docker itself. Convention
is preferred over configuration, but you can customize the scripts to your own needs. Internally at [Djed](http://djed.com/),
many of these scripts take a handful of optional arguments.

## Requirements

* VirtualBox
* Vagrant
* Docker

# How It Works

    bin/sooz up

`sooz-up` will launch a new vagrant box, set up docker, and set docker to listen on `tcp://0.0.0.0:4243` so our scripts
can talk to it.

    bin/sooz down

`sooz-down` will stop all containers and halt the vagrant box.

    bin/sooz migrate

`sooz-migrate` runs django's syncdb followed by database migrations.

    bin/sooz build

`sooz-build` will completely rebuild the docker images at the base of our app from scratch, using `Dockerfiles` located in
`support/dockerfiles/`

# Dockerfiles

We deploy with [AWS Beanstalk](http://aws.amazon.com/elasticbeanstalk/), so the root-level `Dockerfile` is intended mostly for this
deployment. As such, we tuck away our developer environment `Dockerfile` in `support/dockerfiles`. The only major difference is that
the root `Dockerfile` will use `ADD` to copy our code into the image, whereas `support/dockerfiles/app/Dockerfile` only copies in 
`requirements.txt` so as to seed the image with what it needs to install a python `virtualenv`. It's expected that the code will be
shared into the developer environment using docker's `-v [volume]:[volume]` argument. As convention, we prefer that code always 
reside in `/opt/app`.

# TODO

* `sooz-up` should check to see if the docker binary is installed (eg. [boot2docker](https://github.com/boot2docker/boot2docker)).
