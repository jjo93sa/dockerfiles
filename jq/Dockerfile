# Title:  jq/Dockerfile
#
# Description:
#   Definition for Docker container to run jq, which is useful for macOS,
#   which doesn't have jq.
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
#   (1) Build the image whilst in the nmap directory:
#
#       docker build -t <repo>/jq .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/jq          # for x86-64
#       docker pull r.j2o.it/arm32v6/jq  # for arm32, Raspberry Pi-compatible
#
#   Run the container:
#
#       docker run -it --rm --name jq <repo>/jq <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the jq help (jq -h).
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

LABEL maintainer "James Osborne <dr.j.osborne@gmail.com>"
LABEL status "work in progress"
LABEL version "0.1"

RUN apk --no-cache add jq

ENTRYPOINT [ "jq" ]
CMD [ "-h" ]

#
#
# Title:  jq/Dockerfile
#
# Description:  Definition for Docker container to run jq json parser
#
# Usage:  See .dockerfunc for more details, but in brief:
#
# docker run -i jjo93sa/jq "$*"
#
# Maintainer: dr.j.osborne@gmail.com
#
#FROM debian:jessie
#
#LABEL maintainer "James Osborne <dr.j.osborne@gmail.com>"
#LABEL status "work in progress"
#
#RUN apt-get update && apt-get install -y \
#   jq \
#   --no-install-recommends \
#   && rm -rf /var/lib/apt/lists/*
#
#ENTRYPOINT [ "jq" ]
