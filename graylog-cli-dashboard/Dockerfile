
#
# Description:
#   Definition for Docker image to run graylog-cli-dashboard in a container,
#   repository is found here: https://github.com/jjo93sa/cli-dashboard, which
#   is forked from: https://github.com/graylog-labs/cli-dashboard.
#
#   The fork fixes some problems, which were reported to upstream in this
#   comment thread: https://github.com/graylog-labs/cli-dashboard/issues/36.
#
#   This image has been tested against Graylog version 3.1.2.
#
# Dependencies:
#   None
#
# Credits:
#   The heavy lifting is done by: https://github.com/graylog-labs/cli-dashboard
#   thanks to them.
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the graylog-cli-dashboard directory:
#
#       docker build -t <repo>/graylog-cli-dashboard .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/graylog-cli-dashboard          # for x86-64
#       docker pull r.j2o.it/arm32v6/graylog-cli-dashboard  # for arm32, RPi
#
#   Then run the container:
#
#       docker run --rm --name graylog-cli-dashboard \
#          <repo>/graylog-cli-dashboard <params>
#
#   If no parameters are provided, the container defaults to running the --help
#   switch.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG TAG=lts-alpine
FROM node:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.1"

RUN apk add --no-cache git \
                       tzdata

# Set-up some environment variables. LANG and TERM make graphs display properly
# TZ ensures we run in UTC, same as our Graylog server
ENV LANG=en_US.utf8
ENV TERM=xterm-256color
ENV TZ Etc/UTC

# More timezone stuff
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN npm config set unsafe-perm true
# Install the module directly from GitHub, -g is global & installs deps
#RUN npm install graylog-cli-dashboard -g
RUN npm install github:jjo93sa/cli-dashboard -g --loglevel verbose -dd

#COPY screen.js /usr/local/lib/node_modules/graylog-cli-dashboard/lib/
ENTRYPOINT [ "graylog-dashboard" ]
CMD [ "--help" ]
