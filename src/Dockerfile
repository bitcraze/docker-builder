FROM buildpack-deps:buster

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN dpkg --add-architecture i386 && apt-get update \
    && apt-get install -y --no-install-recommends \
		telnet \
		libncurses5:i386 \
		zip unzip \
		sdcc libftdi-dev \
		python3 python3-pip python3-wheel python3-dev python3-setuptools\
		rake \
		python-numpy python-scipy python3-numpy python3-scipy \
	&& rm -rf /var/lib/apt/lists/*

RUN curl -fsSLO https://developer.arm.com/-/media/Files/downloads/gnu-rm/8-2019q3/RC1.1/gcc-arm-none-eabi-8-2019-q3-update-linux.tar.bz2 \
    && tar --strip-components=1 -xvjf gcc-arm-none-eabi-8-2019-q3-update-linux.tar.bz2 -C /usr/local/

ENV DOCKER_VERSION 19.03.1
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz && tar --strip-components=1 -xvzf docker-${DOCKER_VERSION}.tgz -C /usr/local/bin

# When running on linux with a non root user, there is no HOME directory why it
# defaults to /. To make the behaviour uniform for all users create a new HOME
# for all users. We also have to make it read and writeable for everyone
RUN mkdir /new_home
ENV HOME=/new_home

RUN pip3 install flake8 pyusb==1.0.0b2 mock pyserial pre-commit tox

# Make pre-commit download and prepare all tools
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

# Set the locale to make ruby open text files as UTF-8 default
# (Encoding.default_external). Required to make html-proofer handle UTF-8.
ENV LANG C.UTF-8

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module && chmod -R 777 /new_home

WORKDIR /module
