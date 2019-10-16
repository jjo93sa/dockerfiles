# Title:  dos2unix/Dockerfile
#
# Description:
#   Definition for Docker image to run dos2unix, which is useful for macOS,
#   which doesn't have dos2unix or its variants.
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
#   (1) Build the image whilst in the dos2unix directory:
#
#       docker build -t <reg>/dos2unix .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/dos2unix
#
#   Then run the container:
#
#       docker run --rm --name dos2unix \
#          --mount type=bind,source=`pwd`,target=/dos2unix/data \
#          <reg>/dos2unix <params>
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG TAG=latest
FROM alpine:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.2"

RUN apk --no-cache add \
   dos2unix \
   --repository http://dl-3.alpinelinux.org/alpine/edge/community/ --allow-untrusted

WORKDIR /dos2unix/data

ENTRYPOINT [ "dos2unix" ]
CMD [ "-h" ]
