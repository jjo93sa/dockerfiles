# Title:  tftpd/Dockerfile
#
# Description:
#
# Dependencies:
#
# Credits:
#
# Usage:
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
LABEL version "0.2"
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

# Document the the ports we need to expose
EXPOSE $TFTP_PORT/udp
EXPOSE ${SERVER_LOW}-${SERVER_TOP}/udp

# Make a new user and group, and associated home dir
RUN mkdir -p /tftpd/data \
        && addgroup tftpd-grp \
        && adduser -S -s /bin/sh -h /tftpd/data tftpd-user -G tftpd-grp

# By default tftpd runs as user nobody so we change it to our user
RUN chown -R tftpd-user:tftpd-grp /tftpd && chmod -R a+w /tftpd

# Set the environment variables, which can be changed at runtime
ENV PORT_LOW=${SERVER_LOW}
ENV PORT_TOP=${SERVER_TOP}
ENV TFTP_USR=tftpd-user

#ENTRYPOINT [ "in.tftpd" ]
#CMD [ "-L", "--secure", "/tftpd/data", "--create", "-v", "-v", "-v", "--port-range", "$SERVER_LOW:$SERVER_TOP" ]
ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
