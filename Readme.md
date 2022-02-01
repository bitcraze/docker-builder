# Bitcraze Builder Docker Image

This repository contains source code for the official docker builder image
that is used to build Bitcraze projects.

See src/README.md for more information.

## Building and releasing the image

To build the image and push it to docker hub:
1. Create a tag
2. Go to the [`Release and push to docker hub`](https://github.com/bitcraze/docker-builder/actions/workflows/release.yml) workflow in actions for the repository on github
3. Chose the tag and start the workflow

## Manual build

If the image can not be built automatically follow this procedure:
1. Build a local image for test: `docker build -t my_builder ./src`
1. Test the image
1. Commit, tag and push to github
1. Make sure your work space is clean and build the production image: `docker build -t bitcraze/builder ./src`. If you have not changed anything this should be very fast.
1. Build a tagged (the tag in this case is 4711) version: `docker build -t bitcraze/builder:4711 ./src` (should also be very fast)
1. Push the tagged version to docker hub: `docker push bitcraze/builder:4711`
1. Push the 'latest' version to docker hub: `docker push bitcraze/builder`


## Docker

On information on how to install and use docker, please go to
[![logo](https://www.docker.com/sites/all/themes/docker/assets/images/logo.png)](https://www.docker.com/)
