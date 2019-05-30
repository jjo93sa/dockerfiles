# Title:  mac2unix/Dockerfile
#
# Description:
#   Definition for Docker image to run mac2unix, which is useful for macOS,
#   which doesn't have mac2unix.
#
# Dependencies:
#   None
#
# Credits:
#   None
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the mac2unix directory:
#
#       docker build -t <repo>/mac2unix .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/mac2unix          # for x86-64
#       docker pull r.j2o.it/arm32v6/mac2unix  # for arm32, Rasp Pi-compatible
#
#   Then run the container:
#
#       docker run --rm --name mac2unix \
#          --mount type=bind,source=`pwd`,target=/mac2unix/data \
#          <repo>/mac2unix <params>
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#

ARG ARCH=r.j2o.it
ARG SRCT=latest
FROM $ARCH/dos2unix:$SRCT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.1"

WORKDIR /dos2unix/data

ENTRYPOINT [ "mac2unix" ]
CMD [ "-h" ]
