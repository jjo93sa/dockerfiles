#
# Title:  cron-certbot/Dockerfile
#
# Description:
#   Docker file to run EFF/Let's Encrypt client in cron for automated
#   certificate generation and renewal. I based this on the Docker file found
#   here: https://github.com/pslobo/dockerized-certbot
#
#   Support for Intel & ARM builds is provided by the "REPO" build-time
#   parameter.
#
# Dependencies:
#   (1) Uses the Docker image cron-template.
#
# Credits:
#   Credit to pslobo for working out the incantations for getting certbot
#   to run under Alpine Linux. I found the official image in Alpine didn't work
#   and was defeated in my own attempts to build the software in Docker. Do
#   check out the repo at: https://github.com/pslobo/dockerized-certbot
#
# Usage:
#   Usage of this file is very simple:
#
#   (1) Create an image of cron-template/Dockerfile
#   (2) Build this file, specifying the correct repository and cron times:
#
#           docker build --build-arg CRON_SPEC="<insert>" \
#                        --build-arg REPO="XXXX" \
#                        -t <your-tag>/cron-certbot .
#
#       (which requires a previously built XXXX/cron-template image)
#   (3) Run a container from the image built in (2):
#
#           docker run -d -p80:80 -v /path/on/host:/etc/letsencrypt \
#                    -e "YOUR_EMAIL=you@mail.og" \
#                    -e "CERT_DOMAIN=you.org,another.org" \
#                    -e "STAGING=no" \
#                     --rm --name cron-certbot <your-tag>/cron-certbot
#
# Maintainer:
#   dr.j.osborne@gmail.com
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
ARG ARCH=r.j2o.it
ARG SRCT=latest
FROM $ARCH/cron:$SRCT

ARG CRON_SPEC="29 17 * * 3"

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.1"
LABEL status "development"

WORKDIR /opt/certbot
ENV PATH /opt/certbot/venv/bin:$PATH

RUN export BUILD_DEPS="git \
                build-base \
                libffi-dev \
                linux-headers \
                py-pip \
                python-dev" && \
    apk upgrade -U && \
    apk --no-cache add dialog \
                       python \
                       openssl-dev \
		               augeas-libs \
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
