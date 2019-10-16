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
ARG  TAG=3.1
FROM graylog/graylog:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.2"

ARG GRAYLOG_UID=1100
ARG GRAYLOG_GID=1100

USER root

ADD  assets/GeoLite2-City_20190820.tar.gz GeoLite2-City
RUN chown --recursive "${GRAYLOG_UID}":"${GRAYLOG_GID}" GeoLite2-City
RUN mv GeoLite2-City/GeoLite2-City_20190820/GeoLite2-City.mmdb GeoLite2-City/

USER graylog

# Entrypoint it inherited from the graylog image
