# Title:  vsftpd/Dockerfile
#
# Description:
#   Definition for Docker container to run vsftpd, the FTP daemon, which is
#   useful for macOS, which doesn't have an FTP server by default. Using Debian
#   here because there seems to be a missing PAM library (pam_userdb) in
#   Alpine, which is exactly the library we need to use for virtual user
#   authentication with the Berkley DB.
#
# Dependencies:
#   We use confd (compiled in a multi-stage build) to create the configuration
#   files for vsFTPd.
#
# Credits:
#   Based in part on this image: https://github.com/fauria/docker-vsftpd
#
# Usage:
#   To use the Docker container either:
#
#   (1) Build the image whilst in the nmap directory:
#
#       docker build -t <repo>/vsftpd .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/vsftpd          # for x86-64
#       docker pull r.j2o.it/arm32v6/vsftpd  # for arm32, Raspberry Pi-compatible
#
#   Run the container:
#
#       docker run -it -e FTP_USER=james -e FTP_PASS=james -p 21:21 -p 20:20 \
#          -p 3000-3009:3000-3009 <repo>/vsftpd
#
#   Changing the FTP_USER and FTP_PASS variables, and the port mappings, to suit your use case.
#
#   There are additional variables that may be used to control FTP PASV mode:
#
#       PASV_ADDRESS  if undeclared, vsftpd used the incoming socket address
#       PASV_MIN_PORT if undeclared, defaults to 3000
#       PASV_MAX_PORT if undeclared, defaults to 3009
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#


ARG ARCH=library
ARG SRCT=stretch-slim

# Build #1
FROM golang:stretch as confd

ARG CONFD_VERSION=0.16.0

ADD https://github.com/kelseyhightower/confd/archive/v${CONFD_VERSION}.tar.gz /tmp/
ENV GOPATH /go
RUN apt-get update && apt-get install -y \
        bzip2 \
        git \
    make && \
    mkdir -p /go/src/github.com/kelseyhightower/confd && \
    cd /go/src/github.com/kelseyhightower/confd && \
    tar --strip-components=1 -zxf /tmp/v${CONFD_VERSION}.tar.gz && \
    go install github.com/kelseyhightower/confd && \
    rm -rf /tmp/v${CONFD_VERSION}.tar.gz

# Build #2
FROM $ARCH/debian:$SRCT

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"
LABEL version "1.0"

# Copy the confd binary from Build #1
COPY --from=confd /go/bin/confd /usr/local/bin/confd

RUN apt-get update && apt-get -y install \
        db-util \
        vsftpd && apt-get clean

# Copy our confd templates and template resources
COPY confd/ /etc/confd/

# Copy the vsftpd virtual user configuration
COPY assets/vsftpd_virtual /etc/pam.d/

# Our entrypoint script
COPY assets/entrypoint /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint

# Create some directories we need
RUN mkdir -p /etc/vsftpd/
RUN mkdir -p /var/ftp/

# Create a virtual user and home directory
RUN groupadd virtual
RUN useradd  -m -d /ftp -g virtual virtual

EXPOSE 20 21

ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
