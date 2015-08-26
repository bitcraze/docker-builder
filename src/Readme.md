# Bitcraze builder

This image is used to build Bitcraze projects. It contains all the tools needed
and is used by our build tools/servers.

The image currently support python and firmware modules.

# Usage

To build a module/project using the image

    docker run --rm -v "$PWD":/module bitcraze/builder ./tools/build/build.sh

This will add your current directory, as a volume to the container, and run the
build script.
