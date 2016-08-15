FROM buildpack-deps:jessie

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN apt-get update && apt-get install -y --no-install-recommends \
		gcc-arm-none-eabi \
		gdb-arm-none-eabi \
		binutils-arm-none-eabi \
		libnewlib-arm-none-eabi \
		telnet \
		zip unzip \
		sdcc libftdi-dev \
		python python-pip python-wheel python-dev \
		python3 python3-pip python3-wheel python3-dev \
		rake \
		python-numpy python-scipy python3-numpy python3-scipy \
	&& rm -rf /var/lib/apt/lists/*

ENV DOCKER_VERSION 1.12.0
RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-${DOCKER_VERSION}.tgz && tar --strip-components=1 -xvzf docker-1.12.0.tgz -C /usr/local/bin

RUN pip install flake8 pyusb==1.0.0b2
RUN pip3 install flake8 pyusb==1.0.0b2

RUN pip3 install pre-commit
# Make pre-commit donwload and prepare all tools
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

WORKDIR /module
