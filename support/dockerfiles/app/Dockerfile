FROM ubuntu:14.04
MAINTAINER Jeremy Derr <jeremy@derr.me>

RUN apt-get -qq update
RUN apt-get install -y python-dev build-essential sqlite3 libsqlite-dev python-virtualenv openssh-server
RUN virtualenv /opt/env

ADD requirements.txt /opt/app/requirements.txt

RUN /opt/env/bin/pip install -r /opt/app/requirements.txt

EXPOSE 80
EXPOSE 8000
EXPOSE 8080

