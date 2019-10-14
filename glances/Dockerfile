#
# Title:  glances/Dockerfile
#
# Description:
#   Glances (https://nicolargo.github.io/glances/) running in a Docker
#   container for easy deployment. We use Alpine Linux and install Glances from
#   source to keep the image size small.
#
#   This image has been created primarily to export information to a remote
#   InfluxDB instance. However, the export (since it is defined in a CMD) can
#   be overridden and the glances terminal output used instead. (For example,
#   use a different command to override the CMD value, such as --time TIME
#   where TIME is the refresh time.)
#
# Dependencies:
#   Some runtime options are required to ensure the glances process has access
#   to the requisite host information, see usage.
#
#   Also, the glances.env file should be populated with your InfluxDB
#   information.
#
# Credits:  None
#
# Usage:
#   You have two options to use the image defined by this Dockerfile:
#
#     (1) Build this file yourself;
#     (2) Use the pre-built image available at r.j2o.it/glances.
#
#   (1) Building the image is achieved like this:
#
#     docker build --build-arg "ARCH=arm32v6" -t <repo>/glances .
#
#   Without the --build-arg parameter the image built defaults for X86-64
#   processors. Specifying the ARCH in this way allows images for other
#   architectures to be built (arm32v6 for Raspberry Pis for example).
#
#   (2) Pulling the image can be achieved like this:
#
#     docker pull r.j2o.it/glances
#     docker pull r.j2o.ir/arm32v6/glances
#
#   Running the image goes something like this:
#
#     docker run --detach --restart=always \
#       -v /var/run/docker.sock:/var/run/docker.sock:ro \
#       --env-file glances.env \
#       --net host \
#       --pid host -it r.j2o.it/glances
#
#   Where we give the docker container read only access to the Docker socket,
#   the host's PID namespace and the host's network to enable monitoring of the
#   host resources. The environment file, glances.env, should be populated with
#   the information for your remote InfluxDB.
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
LABEL version "0.1"
LABEL status "development"

# Install python, plus some development tools to build later packages
RUN apk --no-cache add alpine-sdk \
                       linux-headers \
                       python \
                       python-dev \
                       py-pip && \
# Use pip to install glances and modules we need
    pip install glances[ip,folders,docker,cpuinfo,action] && \
    pip install bernhard \
                couchdb \
                elasticsearch \
                influxdb \
                kafka-python \
                pika \
                potsdb \
                prometheus-client \
                statsd \
                protobuf \
                futures \
                python-dateutil \
                pytz && \
    # Remove the development tools, which we don't need any more
    apk del alpine-sdk python-dev py-pip linux-headers

# Comment out these two lines because they cause exceptions on the RPi
RUN sed -i -e "s/self.stats\['cpu_hz_current'\] = /#self.stats['cpu_hz_current'] = /g" \
           -e "s/self.stats\['cpu_hz'\] = /#self.stats['cpu_hz'] = /g" \
            /usr/lib/python2.7/site-packages/glances/plugins/glances_quicklook.py

# Copy over the config and entry script
RUN mkdir -p /etc/glances
COPY glances.conf /etc/glances/
COPY start-glances /usr/local/bin

# Give the entry script executable perms
RUN chmod 755 /usr/local/bin/start-glances

RUN addgroup -g 1729 glances && adduser -S -u 1729 -G glances glances

RUN chown -R glances:glances /etc/glances

USER glances

# Define default command.
ENTRYPOINT [ "/usr/local/bin/start-glances", "-C", "/etc/glances/glances.conf" ]

# Define default parameters, making this glances output to InfluxDB
# Can override these commands, for example with --time <s> to see curses output
CMD [ "--export", "influxdb", "--quiet" ]
