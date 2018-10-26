# Title:  ctags/Dockerfile
#
# Description:
#   Definition for Docker container to run ctags, which is useful for macOS,
#   which has an old/feature incomplete version of ctags.
#
# Dependencies:
#   None
#
# Credits:
#   None
#
# Usage:
#   To use the Docker container either:
#
#   (1) Build the image whilst in the ctags directory:
#
#       docker build -t <repo>/ctags .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/ctags          # for x86-64
#       docker pull r.j2o.it/arm32v6/ctags  # for arm32, Raspberry Pi-compatible
#
#   Run the container in the root of the project requiring tags:
#
#       docker run -it --mount type=bind,source="$(pwd)",target=/data \
#          --rm --name ctgas <repo>/ctags <params> .
#
#       If the container is run without any parameters, it defaults to showing
#       the languages supported by this version of ctags: −−list−languages
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG ARCH=library
ARG SRCT=latest
FROM $ARCH/alpine:$SRCT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "test"
LABEL version "0.1"

RUN apk --no-cache add ctags

WORKDIR /data

ENTRYPOINT [ "ctags" ]
CMD [ "--list-languages" ]
