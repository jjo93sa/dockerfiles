# Title:  wget/Dockerfile
#
# Description:
#   Definition for Docker container to run wget, which is useful for macOS,
#   which doesn't have wget.
#
# Dependencies:
#   None
#
# Credits:
#   None
#
# Usage:
#   To use the Docker container:
#
#   (1) Build the image whilst in the wget directory:
#
#       docker build -t <repo>/wget .
#
#       Alternatively, download a prebuilt image from r.j2o.it. Check this site
#       for listing of images/architectures supported: https://r.j2o.it
#
#   (2) Run the container:
#
#       docker run -it -v `pwd`:/output --rm --name wget <repo>/wget <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the wget help (wget -h).
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
LABEL status "production"
LABEL version "1.0"

RUN apk --no-cache add wget

WORKDIR /output

RUN addgroup -S wget-grp && adduser -S wget-user -G wget-grp
USER wget-user

ENTRYPOINT [ "wget" ]
CMD [ "-h" ]
