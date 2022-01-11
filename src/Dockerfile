FROM buildpack-deps:buster

LABEL maintainer="Kristoffer Richardsson <kristoffer@bitcraze.io>"

RUN dpkg --add-architecture i386 && apt-get update \
    && apt-get install -y --no-install-recommends \
		telnet \
		libncurses5:i386 \
		libusb-1.0-0 \
		zip unzip \
		sdcc libftdi-dev \
		python3 python3-pip python3-wheel python3-dev python3-setuptools\
		python-pip python-setuptools \
		rake \
		swig \
		flex bison gperf \
		python3-opencv \
	&& rm -rf /var/lib/apt/lists/*

RUN curl -fsSLO https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2020q2/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 \
    && tar --strip-components=1 -xvjf gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 -C /usr/local/

ENV DOCKER_VERSION 19.03.1
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz && tar --strip-components=1 -xvzf docker-${DOCKER_VERSION}.tgz -C /usr/local/bin

# When running on linux with a non root user, there is no HOME directory why it
# defaults to /. To make the behaviour uniform for all users create a new HOME
# for all users. We also have to make it read and writeable for everyone (done later)
RUN mkdir /new_home
ENV HOME=/new_home

# Upgrade pip to avoid need for wheel compiliation
RUN pip3 install --upgrade pip

RUN pip3 install numpy==1.20.2 scipy==1.7.1 flake8==3.8.4 pyusb==1.0.0b2 mock pyserial pre-commit tox opencv-python==4.5.1.48 pyelftools pytest

# Make pre-commit download and prepare all tools for python testing
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

# In order to build ESP-32 based projects we need the Espressif SDK and
# the build scripts need to know where to find it
RUN mkdir -p /new_home/esp

# First we setup the esp32 toolchain and add it to PATH
RUN curl -fsSLO https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-97-gc752ad5-5.2.0.tar.gz \
	&& tar -xzf xtensa-esp32-elf-linux64-1.22.0-97-gc752ad5-5.2.0.tar.gz -C /new_home/esp
ENV PATH=$PATH:/new_home/esp/xtensa-esp32-elf/bin

# ... as well as openocd for esp32
RUN curl -fsSLO https://github.com/espressif/openocd-esp32/releases/download/v0.10.0-esp32-20210902/openocd-esp32-linux64-0.10.0-esp32-20210902.tar.gz \
	&& tar -xzf openocd-esp32-linux64-0.10.0-esp32-20210902.tar.gz -C /new_home/esp

RUN cd /new_home/esp && git clone -b release/v3.3 --recursive https://github.com/espressif/esp-idf.git
RUN cd /new_home/esp/esp-idf && python2.7 -m pip install --user -r requirements.txt
RUN cd /new_home/esp/esp-idf && ./install.sh esp32
ENV IDF_PATH=/new_home/esp/esp-idf

# Set the locale to make ruby open text files as UTF-8 default
# (Encoding.default_external). Required to make html-proofer handle UTF-8.
ENV LANG C.UTF-8

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module && chmod -R 777 /new_home

WORKDIR /module
