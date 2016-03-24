FROM buildpack-deps:jessie

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN apt-get update && apt-get install -y --no-install-recommends \
		gcc-arm-none-eabi \
		binutils-arm-none-eabi \
		libnewlib-arm-none-eabi \
		zip unzip \
		sdcc libftdi-dev \
		python python-pip python-wheel python-dev \
		python3 python3-pip python3-wheel python3-dev \
		rake \
	&& rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://get.docker.com/ | sh

RUN pip install flake8
RUN pip3 install flake8

RUN pip install pyusb==1.0.0b2
RUN pip3 install pyusb==1.0.0b2

RUN pip3 install pre-commit
# Make pre-commit donwload and prepare all tools
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

WORKDIR /module
