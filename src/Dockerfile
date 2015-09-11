FROM buildpack-deps:jessie

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN apt-get update && apt-get install -y --no-install-recommends \
		gcc-arm-none-eabi \
		binutils-arm-none-eabi \
		libnewlib-arm-none-eabi \
		zip unzip \
		sdcc libftdi-dev \
		python python-pip python3 python3-pip \
	&& rm -rf /var/lib/apt/lists/*

RUN pip install flake8
RUN pip3 install flake8

WORKDIR /module
