# Title:  metaflac/Dockerfile
#
# Description:
#   Definition for Docker container to run metaflac, which is useful for macOS,
#   which doesn't have metaflac. This application can be used to show and edit
#   the metadata blocks in a FLAC file. metaflac is one component in the Alpine
#   main/flac package.
#
#   https://xiph.org/flac/documentation_tools_metaflac.html
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
#   (1) Build the image whilst in the metaflac directory:
#
#       docker build -t <reg>/metaflac .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/metaflac
#
#   Run the container:
#
#       docker run -it --rm --name metaflac \
#          --mount type=bind,source=`pwd`,target=/metaflac/data \
#          <reg>/metaflac <params>
#
#       If the container is run without any parameters, it defaults to showing
#       the metaflac version information (metaflac --version).
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
LABEL status "development"
LABEL version "0.2"

RUN apk --no-cache add flac

WORKDIR /metaflac/data

ENTRYPOINT [ "metaflac" ]
CMD [ "--version" ]
