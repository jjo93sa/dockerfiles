#
# Title: pductl/Dockerfile
#
# Description:
#   Definition for Docker image to control a LINDY IP Power Switch Classic 8
#   PDU using SNMP. We include a copy of the LINDY PDU MIB in this container
#   image.
#
# Dependencies:
#   Change the write community variable to your setting. This can be done at
#   run time.
#
# Credits:
#   None
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the pductl directory:
#
#       docker build -t <reg>/pductl .
#
#   Or
#
#   (2) Download a pre-built image from r.j2o.it:
#
#       docker pull r.j2o.it/pductl
#
#   Then run the container:
#
#       docker run --rm --name pductl \
#          <reg>/pductl <params>
#
#   If no parameters are provided, the container defaults to running the help
#   command to show the help text. Otherwise the following commands are
#   available:
#
#   pductl status    - Display the current status of the PDU outlets and total
#                       current draw;
#   pductl on <num>  - Turn on PDU outlet <num> where outlets are numbered
#                       1 - 8;
#   pductl off <num> - Turn off PDU outlet <num> where outlets are numbered
#                       1 - 8.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repository root.

ARG TAG=buster-slim
FROM debian:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"
LABEL version "0.1"

RUN sed -i '/^deb/ s/$/ non-free/' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
            bsdmainutils \
            snmp \
            snmp-mibs-downloader \
            --no-install-recommends \
            && rm -rf /var/lib/apt/lists/*

COPY assets/PDUMIB.mib /var/lib/snmp/mibs/lindy/
RUN chmod 644 /var/lib/snmp/mibs/lindy/PDUMIB.mib

RUN mkdir -p /var/lib/snmp/mib_indexes

COPY assets/entrypoint /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint

RUN sed -i -e '$ a mibdirs /var/lib/snmp/mibs/lindy:/var/lib/snmp/mibs/ietf' \
     -e '/^mibs/c\mibs PDU-MIB'  \
     /etc/snmp/snmp.conf

ENV WRITE_COMMUNITY=public

ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
CMD [ "help" ]
