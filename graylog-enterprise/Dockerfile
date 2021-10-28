# Title:  graylog/Dockerfile
#
# Description:
#   Definition for Docker image to run Graylog in a container. We base this
#   image on the DockerHub Graylog version, but add the GeoLite2 City
#   geolocation database from https://dev.maxmind.com/geoip/geoip2/geolite2/
#   into: /usr/share/graylog/GeoLite2-City/GeoLite2-City.mmdb
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
#   (1) Build the image whilst in the graylog directory:
#
#         docker build -t <reg>/graylog .
#
#   Or
#
#   (2) Download a prebuilt, multi-platform image from r.j2o.it:
#
#         docker pull r.j2o.it/graylog
#
#   Then run the container, which is easiest with this docker-compose file:
#   https://github.com/jjo93sa/graylog-server.git
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG  TAG=4.0
FROM graylog/graylog-enterprise:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.5"

ARG GRAYLOG_UID=1100
ARG GRAYLOG_GID=1100
ARG GEOLITE2=GeoLite2-City_20210713
ENV GEOLITE2=${GEOLITE2}

USER root

RUN mkdir -p  /etc/graylog

ADD  assets/${GEOLITE2}.tar.gz /etc/graylog/GeoLite2-City
ADD  assets/exponents.csv /etc/graylog/exponents.csv
RUN mv /etc/graylog/GeoLite2-City/${GEOLITE2}/GeoLite2-City.mmdb /etc/graylog/GeoLite2-City.mmdb

COPY --chown="${GRAYLOG_UID}":"${GRAYLOG_GID}" assets/hue assets/pushover /usr/share/graylog/data/scripts/
RUN chmod 0755 /usr/share/graylog/data/scripts/*
RUN chown --recursive "${GRAYLOG_UID}":"${GRAYLOG_GID}" /etc/graylog

USER graylog

# Entrypoint it inherited from the graylog image
