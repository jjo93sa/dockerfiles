# Title:  tftpd/Dockerfile
#
# Description:
#   A container-based TFTP server running in Alpine Linux. We define some port
#   numbers in this build:
#
#   Purpose             Port       Build ARG          ENV var
#   -------------------------------------------------------------
#   Control         - 69/UDP    - ARG: TFTP_PORT   - ENV: <N/A>
#   Data range low  - 60005/UDP - ARG: SERVER_LOW  - ENV: PORT_LOW
#   Data range high - 60010/UDP - ARG: SERVER_TOP  - ENV: PORT_TOP
#
#   These ports can be changed at build time by supplying alternate values
#   using the docker build flag --build-arg. It is also possible to change
#   the ephemeral ports at run time by overriding the ENV parameters.
#
#   The TFTP daemon is configured to run as a user specified by a build ARG,
#   TFTP_USER, which can be overridden, like the ports, at build time.
#
#   TFTP data is stored in the TFTP_USER's configured home, /tftpd/data, and
#   this can be bind mounted into the Docker host to access the files.
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
#   (1) Build the image whilst in the ctags directory:
#
#       docker build -t <repo>/tftpd .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/tftpd          # for x86-64
#       docker pull r.j2o.it/arm32v6/tftpd  # for arm32, Raspberry Pi-compatible
#
#    (3) Run the container, --net host might not be needed, TBC:
#
#       docker run -it -p 69:69/udp -p 60005-60010:60005-60010/udp --net host \
#          -- mount type=bind,source=<some-dir>,target=/tftpd/data \
#          --rm --name tftpd <repo>/tftpd
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#

ARG ARCH=library
ARG SRCT=latest

FROM $ARCH/alpine:$SRCT

# Meta data for the image
LABEL maintainer="dr.j.osborne@gmail.com"
LABEL version "0.4"
LABEL status "Development"

# Install the tftp server
RUN apk add --no-cache tftp-hpa

# Set-up the entrypoint script
COPY assets/entrypoint /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint

# Build ARGS, these can be overridden at buildtime
ARG TFTP_PORT=69
ARG SERVER_LOW=60005
ARG SERVER_TOP=60010
ARG TFTP_USER=tftp-user

# Document the the ports we need to expose
EXPOSE $TFTP_PORT/udp
EXPOSE ${SERVER_LOW}-${SERVER_TOP}/udp

# Make a new user and group, and associated home dir
RUN mkdir -p /tftpd/data \
        && addgroup tftpd-grp \
        && adduser -S -s /bin/sh -h /tftpd/data ${TFTP_USER} -G tftpd-grp

# By default tftpd runs as user nobody so we change it to our user
RUN chown -R ${TFTP_USER}:tftpd-grp /tftpd && chmod -R a+w /tftpd

# Set the environment variables, which can be changed at runtime
ENV PORT_LOW=${SERVER_LOW}
ENV PORT_TOP=${SERVER_TOP}
ENV TFTP_USR=${TFTP_USER}

#ENTRYPOINT [ "in.tftpd" ]
#CMD [ "-L", "--secure", "/tftpd/data", "--create", "-v", "-v", "-v", "--port-range", "$SERVER_LOW:$SERVER_TOP" ]
ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
