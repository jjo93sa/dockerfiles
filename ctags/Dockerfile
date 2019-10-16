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
#       docker build -t <reg>/ctags .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/ctags
#
#   Run the container in the root of the project requiring tags:
#
#       docker run -it --mount type=bind,source="$(pwd)",target=/ctags/data \
#          --rm --name ctags <reg>/ctags <params> .
#
#   If the container is run without any parameters, it defaults to showing the
#   languages supported by this version of ctags: −−list−languages
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repository root.
#
ARG TAG=latest
FROM alpine:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "test"
LABEL version "0.3"

RUN apk --no-cache add ctags

WORKDIR /ctags/data

RUN addgroup -S ctags-grp && adduser -S ctags-user -G ctags-grp
USER ctags-user

ENTRYPOINT [ "ctags" ]
CMD [ "--list-languages" ]
