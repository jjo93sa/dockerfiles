#
# Title: shellcheck/Dockerfile
#
# Description:
#   Definition for Docker image to run shellcheck in a container, see: 
#   https://github.com/koalaman/shellcheck
#
# Dependencies:
#   None
#
# Credits:
#   None
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the shellcheck directory:
#
#       docker build -t <reg>/shellcheck .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/shellcheck
#
#   Then run the container, bind mounting a volume so the files are accessible
#   to the shellcheck process:
#
#       docker run --rm --name shellcheck \
#          -v <some-dir>:/shellcheck/data \
#          <reg>/shellcheck <params>
#
#   If no parameters are provided, the container defaults to running the -h
#   switch to show the help text.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.

ARG TAG=bullseye-slim
FROM debian:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.2"

RUN apt-get update && apt-get install -y \
            file \
            shellcheck \
            --no-install-recommends \
            && rm -rf /var/lib/apt/lists/*

WORKDIR /shellcheck/data

ENTRYPOINT [ "shellcheck" ]
CMD [ "-h" ]
