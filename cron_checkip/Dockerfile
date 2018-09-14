#
# Title:  checkip/Dockerfile
#
# Description:
#   Definition for Docker container to poll public IP addresses (using
#   ipinfo.io) and notify changes (using pushover.net). Also updates a DuckDNS
#   -hosted domain.
#
#   We use an environment file to define the TOKENs needed for the services:
#   ipinfo.io, DuckDNS and pushover.net. See checkip.env, and complete with
#   your token information.
#
#   The base image, jjo93sa/intel/cron-template, runs the worker task every
#   minute by default. See the cron-template/Dockerfile for instructions on
#   how to change the frequency that checkip will runi, but simply put use this
#   build-argument using the normal cron syntax:
#
#        --build-arg CRON_SPEC="* * * * *"
#
#   This image uses a base of jjo93sa/intel/cron-template. The default
#   repository jjo93sa/intel can be changed at build time with:
#
#        --build-arg REPO="XXX"
#
# Dependencies:
#   (1) This Dockerfile uses a multi-stage build, which requires Docker >17.05.
#   (2) The build must have access to the following GitHub repositories:
#	       jjo93sa/notification-utils
#	       jjo93sa/script-utils
#	       jjo93sa/ip-utils
#	(3) There must be an image of jjo93sa/intel/cron-template
#	(4) You must have accounts at pushover.net, ipinfo.io and DuckDNS
#	(5) The checkip.env file must be populated with your tokens from those
#	    services
#
# Credits:  None
#
# Usage:
#   Usage of this file is very simple:
#
#   (1) Create an image of cron-template/Dockerfile
#   (2) Build this file, specifying the correct repository and cron times:
#
#           docker build --build-arg CRON_SPEC="<insert>" \
#                        --build-arg REPO="XXXX" \
#                        -t <your-tag>/checkip .
#
#       (which requires a previously built XXX/cron-template image)
#
#   (3) Edit checkip.env with your toekn information
#   (4) Run a container from the image built in (2):
#
#           docker run -d -env-file checkip.env \
#                       --rm --name checkip <your-tag>/checkip
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see LICENSE file in repoistory root.
#
ARG ARCH=r.j2o.it
ARG SRCT=latest
FROM alpine AS intermediate

# Install git in intermediate
RUN apk --no-cache add git

# Clone the repos we need
RUN git clone https://github.com/jjo93sa/script-utils.git
RUN git clone https://github.com/jjo93sa/notification-utils.git
RUN git clone https://github.com/jjo93sa/ip-utils.git

FROM $ARCH/cron:$SRCT

ARG CRON_SPEC="*/5 * * * *"

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "1.0"
LABEL status "production"

# Add jq and curl, which are needed in the scripts below
RUN apk --no-cache add jq curl

# Copy the scripts we need
# NB: ./task is copied via an ONBUILD from parent image
COPY --from=intermediate /script-utils/scripts/script_logging /usr/local/bin/
COPY --from=intermediate /script-utils/scripts/script_ctrl /usr/local/bin/
COPY --from=intermediate /ip-utils/scripts/get_ip_info /usr/local/bin/
COPY --from=intermediate /ip-utils/scripts/update_dns /usr/local/bin/
COPY --from=intermediate /notification-utils/scripts/pushover /usr/local/bin/

# NB: ENTRYPOINT/CMD inherited through ONBUILD from parent image
