#
# Title:  cron_checkip/Dockerfile
#
# Description:
#   Definition for a container to poll public IP address (using ipinfo.io) and
#   notify changes (using pushover.net). Also updates a DuckDNS-hosted domain.
#
#   We use an environment file to define the TOKENs needed for the services:
#   ipinfo.io, DuckDNS and pushover.net. See checkip.env in the repository, and
#   complete with your token information.
#
#   To keep the final image small, we use multi-stage build. First we create an
#   intermediate image that we use to clone several github repos. In the second
#   stage we then copy the necessary scripts from the intermediate into the
#   final image.
#
#   The base image for the second build stage, r.j2o.it/cron is a "template"
#   image that uses a lot of ONBUILD instructions. This means we must define
#   some parameters for the second build stage to complete successfully. Key is
#   that we define a CRON_SPEC either before the _FIRST_ FROM or as an argument
#   in the build command:
#
#        --build-arg CRON_SPEC="* * * * *"
#
#   For ease of automated building, we define CRON_SPEC in this file. If
#   building from the Dockerfile, you may still over-ride this setting during
#   build by specifying the --build-arg as above.
#
#   This image uses a default stage 2 base of r.j2o.it/cron, which is built for
#   X86-64 processors. The default architecture can be changed at build-time
#   with the following --build-arg, e.g. for Raspberry Pi images:
#
#        --build-arg ARCH="r.j2o.it/arm32v6/cron"
#
# Dependencies:
#   (1) This Dockerfile uses a multi-stage build, which requires Docker >17.05.
#   (2) The build must have access to the following GitHub repositories:
#
#          jjo93sa/notification-utils
#          jjo93sa/script-utils
#          jjo93sa/ip-utils
#
#   (3) There must be an image of r.j2o.it/cron available in your Docker system
#   (4) You must have accounts at pushover.net, ipinfo.io and DuckDNS
#   (5) The checkip.env file must be populated with your tokens from those
#       services
#
# Credits:  None
#
# Usage:
#   Usage of this file is very simple, just download the pre-built image for
#   your architecture from r.j2o.it, which provides lists for all available
#   images. For example, docker pull r.j2o.it/arm32v6/cron_checkip and jump to
#   (3) below.
#
#   If you prefer to build your own, for example to set the CRON_SPEC or other
#   parameters, the steps are:
#
#   (1) Create an image of cron/Dockerfile from this repository, or download
#   the pre-built image for your architecture from: r.j2o.it
#   (2) Build this file, specifying the correct ARCH and CRON_SPEC:
#
#           docker build --build-arg CRON_SPEC="<insert>" \
#                        --build-arg ARCH="XXXX" \
#                        -t <your-tag>/cron_checkip .
#
#       (which requires a previously built cron/Dockerfile image)
#
#   (3) Edit checkip.env with your token information
#   (4) Run a container from the image built in (2):
#
#           docker run -d --env-file checkip.env \
#                       --rm --name checkip <your-tag>/cron_checkip
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#

# Build stage 1:
# Define CRON_SPEC before the first FROM to make it a global option
ARG CRON_SPEC="*/5 * * * *"
ARG REG=r.j2o.it
ARG TAG=latest
FROM alpine:$TAG AS intermediate

# Install git in intermediate
RUN apk --no-cache add git

# Clone the repos we need
RUN git clone https://github.com/jjo93sa/script-utils.git
RUN git clone https://github.com/jjo93sa/notification-utils.git
RUN git clone https://github.com/jjo93sa/ip-utils.git

# Build stage 2:
FROM $REG/cron:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "1.1"
LABEL status "production"

# Add jq and curl, which are needed in the scripts below
RUN apk --no-cache add curl jq

# Copy the scripts we need from the intermediate
# NB: ./task is copied via an ONBUILD from parent image
COPY --from=intermediate /script-utils/scripts/script_logging /usr/local/bin/
COPY --from=intermediate /script-utils/scripts/script_ctrl /usr/local/bin/
COPY --from=intermediate /ip-utils/scripts/get_ip_info /usr/local/bin/
COPY --from=intermediate /ip-utils/scripts/update_dns /usr/local/bin/
COPY --from=intermediate /notification-utils/scripts/pushover /usr/local/bin/

# NB: ENTRYPOINT/CMD inherited through ONBUILD from parent image
