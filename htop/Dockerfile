#
# Title:  htop/Dockerfile
#
# Description:
#   Docker file to run htop in a container. On Docker for Mac (now Docker for
#   Desktop), htop only monitors the xhyve VM in which the containers run.
#
# Dependencies:
#   None.
#
# Credits:
#   Based on the work of Jessie Frazelle:
#   https://blog.jessfraz.com/post/docker-containers-on-the-desktop/
#
# Usage:
#   To use the Docker container either:
#
#   (1) Build the image whilst in the htop directory:
#
#       docker build -t <reg>/htop .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/htop
#
#   Run the container in the root of the project requiring tags:
#
#       docker run -it --pid host --rm --name htop <reg>/htop <params>
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
LABEL status "test"
LABEL version "0.3"

RUN apk --no-cache add \
   htop

ENTRYPOINT [ "htop" ]
