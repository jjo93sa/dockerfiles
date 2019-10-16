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
#         docker build  -t <reg>/unix2dos .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#         docker pull r.j2o.it/unix2dos
#
#   Then run the container:
#
#         docker run --rm --name unix2dos \
#          --mount type=bind,source=`pwd`,target=/unix2dos/data \
#          <reg>/unix2dos <params>
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#

ARG REG=r.j2o.it
ARG TAG=latest
FROM $REG/dos2unix:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.2"

WORKDIR /dos2unix/data

ENTRYPOINT [ "unix2dos" ]
CMD [ "-h" ]
