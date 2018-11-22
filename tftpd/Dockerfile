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
LABEL version "0.1"
LABEL status "Development"

# Install the tftp server
RUN apk add --no-cache tftp-hpa

# Build ARGS
ARG TFTP_PORT=69
ARG SERVER_LOW=60005
ARG SERVER_TOP=60010

# Document the the ports we need to expose
EXPOSE $TFTP_PORT/udp
EXPOSE ${SERVER_LOW}-${SERVER_TOP}/udp

ENV PORT_LOW=${SERVER_LOW}
ENV PORT_TOP=${SERVER_TOP}

# Need to change permissions or ownership of the tftpboot directory
# By default tftpd runs as user nobody
RUN mkdir /files && chown nobody /files && chmod a+w /files

ENTRYPOINT [ "in.tftpd" ]
#CMD [ "-L", "--secure", "--create", "--port-range", "$PORT_LOW:$PORT_TOP", "/files" ]
CMD [ "-L", "--secure", "/files", "--create", "-v", "-v", "-v", "--port-range", "$SERVER_LOW:$SERVER_TOP" ]

