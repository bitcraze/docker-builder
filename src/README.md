# Bitcraze builder

This image is used to build Bitcraze projects. It contains all the tools needed
and is used by our build tools/servers. The intention is to reduce problems
with installation of compilers and tools, and to simplify building. If a build
with the image passes on one machine it should also pass on another.

The image currently supports python and compilers for Crazyflie and Crazyradio 
firmware.

# Usage

## Building

You can call make

    docker run --rm -v ${PWD}:/module bitcraze/builder make

This will add your current directory, as a volume in the container at /module,
and run make.

Or make with any arguments, clean for instance

    docker run --rm -v ${PWD}:/module bitcraze/builder make clean

Scripts can be called

    docker run --rm -v ${PWD}:/module bitcraze/builder tools/build/build.sh

## Image structure 

The default directory for the image is /module

    docker run --rm bitcraze/builder pwd

will print

    /module

## Interactive use

To use a container interactively with your current directory as a volume at 
/module

    docker run --rm -it -v ${PWD}:/module bitcraze/builder bash

# Limtations

1. On OS X and Windows, where the docker host is a virtual machine, the source
code you want to build must be located under /User (OS X) and c:\Users
(Windows). For more information see
https://docs.docker.com/userguide/dockervolumes/

1. Using USB from within the container probably works on linux (with some
configuration) but might be harder on OS X and Windows. We have not tried
yet so we don't really know. Flashing will not work without USB.

1. We have not tested this on Windows, there might be limitations. Please
share.