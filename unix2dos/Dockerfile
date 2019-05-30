# Title:  unix2dos/Dockerfile
#
# Description:
#   Definition for Docker image to run unix2dos, which is useful for macOS,
#   which doesn't have unix2dos.
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
#   (1) Build the image whilst in the unix2dos directory:
#
#       docker build -t <repo>/unix2dos .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/unix2dos          # for x86-64
#       docker pull r.j2o.it/arm32v6/unix2dos  # for arm32, Rasp Pi-compatible
#
#   Then run the container:
#
#       docker run --rm --name unix2dos \
#          --mount type=bind,source=`pwd`,target=/unix2dos/data \
#          <repo>/unix2dos <params>
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

ENTRYPOINT [ "unix2dos" ]
CMD [ "-h" ]
