# Title:  nmap/Dockerfile
#
# Description:
#   Definition for Docker container to run nmap, which is useful for macOS,
#   which doesn't have nmap.
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
#       docker build -t <reg>/nmap .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/nmap
#
#   Run the container:
#
#       docker run -it --net host --rm --name nmap <reg>/nmap <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the nmap help (nmap -h).
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
LABEL status "production"
LABEL version "1.1"

RUN apk --no-cache add nmap

ENTRYPOINT [ "nmap" ]
CMD [ "-h" ]
