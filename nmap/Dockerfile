# Title:  nmap/Dockerfile
#
# Description:
#   Definition for Docker container to run nmap, which is useful for macOS,
#   which doesn't have nmap.
#
# Dependencies: None
#
# Credits: None
#
# Usage:
#   To use the Docker container:
#
#   (1) Build the image whilst in the nmap directory:
#
#       docker build -t <repo>/nmap .
#
#   (2) Run the container:
#
#       docker run -it --net host --rm --name nmap <repo>/nmap <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the nmap help (nmap -h).
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see LICENSE file in repoistory root.
#
FROM alpine:latest

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"
LABEL version "1.0"

RUN apk --no-cache add nmap

ENTRYPOINT [ "nmap" ]
CMD [ "-h" ]
