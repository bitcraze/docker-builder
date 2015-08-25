# Bitcraze builder

This image is used to build Bitcraze projects. It contains all the tools needed
and is used by our build tools/servers.

# Usage

To build a project using the image

    docker run --rm -v "$PWD":/module bitcraze/builder ./tools/build/build.sh

This will add your current directory, as a volume to the container, and run the
build script.
