# Title:  unix2mac/Dockerfile
#
# Description:
#   Definition for Docker image to run unix2mac, which is useful for macOS,
#   which doesn't have unix2mac.
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
#   (1) Build the image whilst in the unix2mac directory:
#
#         docker build -t <reg>/unix2mac .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#         docker pull r.j2o.it/unix2mac
#
#   Then run the container:
#
#         docker run --rm --name unix2mac \
#            --mount type=bind,source=`pwd`,target=/unix2mac/data \
#            <reg>/unix2mac <params>
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

ENTRYPOINT [ "unix2mac" ]
CMD [ "-h" ]
