#
# Title:  cron-certbot/Dockerfile
#
# Description:
#   Docker file specifying a container to run EFF/Let's Encrypt client in cron
#   for automated certificate generation and renewal. I based this on the Docker
#   file found here: https://github.com/pslobo/dockerized-certbot
#
#   Support for Intel & ARM builds is provided by the "ARCH" ARG parameter,
#   which may be set at build time using --build-arg.
#
# Dependencies:
#   (1) Uses the Docker image r.j2o.it/cron
#
# Credits:
#   Credit to pslobo for working out the incantations for getting certbot
#   to run under Alpine Linux. I found the official image in Alpine didn't work
#   and was defeated in my own attempts to build the software in Docker. Do
#   check out the repo at: https://github.com/pslobo/dockerized-certbot
#
# Usage:
#   Usage of this Docker image is very simple, pull and execute
#   r.j2o.it/cron_certbot.
#
#   Alternatively, if you want more control over the build-time parameters, you
#   can build the image as follows:
#
#     (1) Create an image of cron/Dockerfile (or pull it from r.j2o.it/cron),
#         which is required in the next step
#     (2) Build this file, specifying the correct repository and cron times:
#
#           docker build --build-arg CRON_SPEC="<insert>" \
#                        --build-arg REPO="XXXX" \
#                        -t <your-tag>/cron_certbot .
#
#     (3) Run a container from the image built in (2):
#
#           docker run -d -p80:80 -v /path/on/host:/etc/letsencrypt \
#                    -e "YOUR_EMAIL=you@mail.og" \
#                    -e "CERT_DOMAIN=you.org,another.org" \
#                    -e "STAGING=no" \
#                     --rm --name cron-certbot <your-tag>/cron-certbot
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   GPLv3 - this is a derivative work made on 2018-09-09 from this repository:
#   https://github.com/pslobo/dockerized-certbot Hence this file, and only this
#   file, is covered under the same license, see LICENSE-GPL.
#
#   In accordance with the aggregate clause in GPLv3, every other file of this repository
#   (https://github.com/jjo93sa/dockerfiles.git) is licensed under MIT. Only
#   this file is covered by GPLv3. See the LICENSE file in the repoistory root.
#
ARG CRON_SPEC="29 17 * * 3"
ARG ARCH=r.j2o.it
ARG SRCT=latest
FROM $ARCH/cron:$SRCT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.2"
LABEL status "development"

WORKDIR /opt/certbot
ENV PATH /opt/certbot/venv/bin:$PATH

RUN export BUILD_DEPS="git \
                build-base \
                libffi-dev \
                linux-headers \
                py-pip \
                python-dev" && \
                       \
    apk --no-cache add augeas-libs \
                       dialog \
                       openssl-dev \
                       python \
                       ${BUILD_DEPS} && \
    pip --no-cache-dir install virtualenv && \
    git clone https://github.com/letsencrypt/letsencrypt /opt/certbot/src && \
    virtualenv --no-site-packages -p python2 /opt/certbot/venv && \
    /opt/certbot/venv/bin/pip install \
        -e /opt/certbot/src/acme \
        -e /opt/certbot/src \
        -e /opt/certbot/src/certbot-apache \
        -e /opt/certbot/src/certbot-nginx && \
    apk del ${BUILD_DEPS}

EXPOSE 80 443
VOLUME /etc/letsencrypt

# NOTE: ENTRYPOINT is provided in the base image
