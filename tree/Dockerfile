# Title:  tree/Dockerfile
#
# Description:
#   Definition for Docker image to run tree, which is useful for macOS, which
#   doesn't have tree.
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
#   (1) Build the image whilst in the tree directory:
#
#       docker build -t <repo>/tree .
#
#       Alternatively, download a prebuilt image from r.j2o.it. Check this site
#       for supported images/architectures: https://r.j2o.it
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/tree          # for x86-64
#       docker pull r.j2o.it/arm32v6/tree  # for arm32, Raspberry Pi-compatible
#
#   Then run the container:
#
#       docker run -it -v `pwd`:/tree/data --rm --name tree <repo>/tree <params>
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG ARCH=library
ARG SCRT=latest
FROM $ARCH/alpine:$SCRT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "development"
LABEL version "0.1"

RUN apk --no-cache add tree

WORKDIR /tree/data

RUN addgroup -S tree-grp && adduser -S tree-user -G tree-grp
USER tree-user

ENTRYPOINT [ "tree" ]
