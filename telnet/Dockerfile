# Title:  telnet/Dockerfile
#
# Description:
#   Definition for Docker container to run telnet because macOS doesn't have a
#   telnet client by default and I don't want to install Homebrew/similar.
#
#   Use Alpine linux to keep the container size small, but the telnet client is
#   barebones.
#
# Dependencies: None
#
# Credits: None
#
# Usage:
#   To use the Docker container:
#
#   (1) Build the image, executing the following in the directory containing
#       the Dockerfile:
#
#         docker build -it <repo>/telnet .
#
#   (2) Run the Docker container:
#
#         docker run --rm --name telnet -it <repo>/telnet <params>
#
#       Executing the container with no params shows the help text.
#
#   (3) Test the container:
#
#         docker run --rm --name telnet -it <repo>/telnet \
#           towel.blinkenlights.nl 23
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see LICENSE file in repoistory root.
#
ARG ARCH=library
ARG SCRT=latest
FROM $ARCH/alpine:$SCRT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"
LABEL version "1.0"

# Alpine's telnet client is container in the busybox-extras package
RUN apk --no-cache add busybox-extras

ENTRYPOINT [ "telnet" ]
