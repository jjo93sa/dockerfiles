#
# Title: ansible-playbook/Dockerfile
#
# Description:
#   Definition for Docker image to run ansible-playbook in a container.
#
# Dependencies:
#   None
#
# Credits:
#   I got the install steps from somewhere, but can't remember where. If I find
#   it again, I'll update this with the link.
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the ansible-playbook directory:
#
#       docker build -t <reg>/ansible-playbook .
#
#   Or
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/ansible-playbook
#
#   Then run the container:
#
#       docker run --rm --name ansible-playbook \
#          <reg>/ansible-playbook <params>
#
#   If no parameters are provided, the container defaults to running the --help
#   switch.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
ARG TAG=latest
FROM alpine:$TAG

# Install rsync, openssh-client
RUN apk --no-cache add rsync openssh-client

RUN builddeps=' \
        python-dev \
        py2-pip \
        musl-dev \
        openssl-dev \
        libffi-dev \
        gcc \
    ' \
    && apk --no-cache add \
        ca-certificates \
        python \
        py-paramiko \
        py-yaml \
        py-jinja2 \
        py-httplib2 \
        $builddeps \
        && pip install --upgrade pip \
        && pip install \
        ansible \
        six \
        && apk del --purge $builddeps

RUN mkdir -p /ansible/playbooks

WORKDIR /ansible/playbooks

ENTRYPOINT [ "ansible" ]
