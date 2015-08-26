FROM python:2.7.10

MAINTAINER Kristoffer Richardsson <kristoffer@bitcraze.io>

RUN apt-get update && apt-get install -y --no-install-recommends \
		gcc-arm-none-eabi \
		binutils-arm-none-eabi \
		libnewlib-arm-none-eabi \
		zip unzip\
	&& rm -rf /var/lib/apt/lists/*

RUN pip install flake8

WORKDIR /module
