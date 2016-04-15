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
	&& rm -rf /var/lib/apt/lists/*

# Install docker (see https://github.com/docker-library/docker)
ENV DOCKER_BUCKET get.docker.com
ENV DOCKER_VERSION 1.10.3
ENV DOCKER_SHA256 d0df512afa109006a450f41873634951e19ddabf8c7bd419caeb5a526032d86d
RUN curl -fSL "https://${DOCKER_BUCKET}/builds/Linux/x86_64/docker-$DOCKER_VERSION" -o /usr/local/bin/docker \
	&& echo "${DOCKER_SHA256}  /usr/local/bin/docker" | sha256sum -c - \
	&& chmod +x /usr/local/bin/docker

RUN pip install flake8
RUN pip3 install flake8

RUN pip install pyusb==1.0.0b2
RUN pip3 install pyusb==1.0.0b2

RUN pip3 install pre-commit
# Make pre-commit donwload and prepare all tools
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

WORKDIR /module
