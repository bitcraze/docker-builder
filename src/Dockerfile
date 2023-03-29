FROM buildpack-deps:22.04

LABEL maintainer="Kristoffer Richardsson <kristoffer@bitcraze.io>"

RUN dpkg --add-architecture i386 && apt-get update \
    && apt-get install -y --no-install-recommends \
		telnet \
		libncurses5:i386 \
		libusb-1.0-0 \
		zip unzip \
		sdcc libftdi-dev \
		python3-pip python3-wheel python3-dev python3-setuptools python-is-python3 \
		rake \
		swig \
		flex bison gperf \
		python3-opencv \
		binutils-arm-none-eabi \
		gcc-arm-none-eabi \
		libnewlib-arm-none-eabi \
		picolibc-arm-none-eabi \
		libstdc++-arm-none-eabi-newlib \
		libstdc++-arm-none-eabi-dev \
		cmake \
		ninja-build \
	&& rm -rf /var/lib/apt/lists/*

# Required environment for Zephyr to find the right libc
ENV SYSROOT_DIR /usr/lib/arm-none-eabi/newlib/

ENV DOCKER_VERSION 19.03.1
RUN curl -fsSLO https://download.docker.com/linux/static/stable/x86_64/docker-${DOCKER_VERSION}.tgz && tar --strip-components=1 -xvzf docker-${DOCKER_VERSION}.tgz -C /usr/local/bin

# When running on linux with a non root user, there is no HOME directory why it
# defaults to /. To make the behaviour uniform for all users create a new HOME
# for all users. We also have to make it read and writeable for everyone (done later)
RUN mkdir /new_home
ENV HOME=/new_home

# Upgrade pip to avoid need for wheel compiliation
RUN pip3 install --upgrade pip

RUN pip3 install scipy==1.9.2 flake8==3.8.4 pyusb==1.0.0b2 libusb-package==1.0.24.2 mock pyserial pre-commit tox pyelftools pytest west

# Make pre-commit download and prepare all tools for python testing
ADD .pre-commit-config.yaml /dummy/.pre-commit-config.yaml
RUN cd /dummy && git init && pre-commit run && cd && rm -rf /dummy

# Install Zephyr SDK for arm
ENV ZEPHYR_SDK_VERSION=0.16.0
RUN curl -L https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v${ZEPHYR_SDK_VERSION}/zephyr-sdk-${ZEPHYR_SDK_VERSION}_linux-x86_64_minimal.tar.xz | tar xJ -C  /opt
RUN curl -L https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v${ZEPHYR_SDK_VERSION}/toolchain_linux-x86_64_arm-zephyr-eabi.tar.xz | tar xJ -C /opt/zephyr-sdk-${ZEPHYR_SDK_VERSION}/

# In order to build ESP-32 based projects we need the Espressif SDK and
# the build scripts need to know where to find it
RUN mkdir -p /new_home/esp

RUN cd /new_home/esp && git clone --depth 1 --branch v4.3.1 --recursive https://github.com/espressif/esp-idf.git
# install.sh does not exactly do what we want. Running parts of it...
RUN cd /new_home/esp/esp-idf && python3 ./tools/idf_tools.py install
ENV IDF_PATH=/new_home/esp/esp-idf
RUN cd /new_home/esp/esp-idf && pip3 install -r requirements.txt
# The export.sh script can not be sourced in docker, set it up manually
ENV PATH=/new_home/esp/esp-idf/components/esptool_py/esptool:/new_home/esp/esp-idf/components/espcoredump:/new_home/esp/esp-idf/components/partition_table:\
/new_home/esp/esp-idf/components/app_update:/new_home/.espressif/tools/xtensa-esp32-elf/esp-2021r1-8.4.0/xtensa-esp32-elf/bin:\
/new_home/.espressif/tools/xtensa-esp32s2-elf/esp-2021r1-8.4.0/xtensa-esp32s2-elf/bin:/new_home/.espressif/tools/xtensa-esp32s3-elf/esp-2021r1-8.4.0/xtensa-esp32s3-elf/bin:\
/new_home/.espressif/tools/riscv32-esp-elf/esp-2021r1-8.4.0/riscv32-esp-elf/bin:/new_home/.espressif/tools/esp32ulp-elf/2.28.51-esp-20191205/esp32ulp-elf-binutils/bin:\
/new_home/.espressif/tools/esp32s2ulp-elf/2.28.51-esp-20191205/esp32s2ulp-elf-binutils/bin:/new_home/.espressif/tools/openocd-esp32/v0.10.0-esp32-20210401/openocd-esp32/bin:\
/new_home/esp/esp-idf/tools:$PATH

# Set the locale to make ruby open text files as UTF-8 default
# (Encoding.default_external). Required to make html-proofer handle UTF-8.
ENV LANG C.UTF-8

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module && chmod -R 777 /new_home

# New version of git are more strict about the permissions of the project directory
# Disable this check
RUN git config --global --add safe.directory '*'

WORKDIR /module
