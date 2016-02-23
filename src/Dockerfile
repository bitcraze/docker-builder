FROM buildpack-deps:jessie

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN apt-get update && apt-get install -y --no-install-recommends \
		gcc-arm-none-eabi \
		binutils-arm-none-eabi \
		libnewlib-arm-none-eabi \
		zip unzip \
		sdcc libftdi-dev \
		python python-pip python-wheel \
		python3 python3-pip python3-wheel \
		rake \
	&& rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://get.docker.com/ | sh

RUN pip install flake8
RUN pip3 install flake8

RUN pip install pyusb==1.0.0b2
RUN pip3 install pyusb==1.0.0b2

WORKDIR /module
