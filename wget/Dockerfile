# Title:  wget/Dockerfile
#
# Description:
#   Definition for Docker container to run wget, which is useful for macOS,
#   which doesn't have wget.
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
#       docker build -t <repo>/wget .
#
#   (2) Run the container:
#
#       docker run -it -v `pwd`:/output --rm --name wget <repo>/wget <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the wget help (wget -h).
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see LICENSE file in repoistory root.
#
FROM alpine:latest

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"
LABEL version "1.0"

RUN apk --no-cache add wget

WORKDIR /output

RUN addgroup -S wget-grp && adduser -S wget-user -G wget-grp
USER wget-user

ENTRYPOINT [ "wget" ]
CMD [ "-h" ]
