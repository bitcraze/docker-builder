FROM buildpack-deps:jessie

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN dpkg --add-architecture i386 && apt-get update \
    && apt-get install -y --no-install-recommends \
		telnet \
		libncurses5:i386 \
		zip unzip \
		sdcc libftdi-dev \
		python python-pip python-wheel python-dev \
		python3 python3-pip python3-wheel python3-dev \
		rake \
		python-numpy python-scipy python3-numpy python3-scipy \
	&& rm -rf /var/lib/apt/lists/*

RUN curl -fsSLO https://launchpad.net/gcc-arm-embedded/5.0/5-2016-q2-update/+download/gcc-arm-none-eabi-5_4-2016q2-20160622-linux.tar.bz2 \
    && tar --strip-components=1 -xvjf gcc-arm-none-eabi-5_4-2016q2-20160622-linux.tar.bz2 -C /usr/local/

ENV DOCKER_VERSION 1.12.0
RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-${DOCKER_VERSION}.tgz && tar --strip-components=1 -xvzf docker-${DOCKER_VERSION}.tgz -C /usr/local/bin

RUN pip2 install flake8 pyusb==1.0.0b2 mock pyserial
RUN pip3 install flake8 pyusb==1.0.0b2 mock pyserial pre-commit tox

# Make pre-commit download and prepare all tools
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module

WORKDIR /module

# When running on linux with a non root user, there is no HOME directory why it
# defaults to /. To make the behaviour uniform for all users set HOME
# to /root for all users. We also have to make it read and writeable for everyone
RUN chmod -R 777 /root
ENV HOME=/root
