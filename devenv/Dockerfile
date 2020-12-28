#
# Title: devenv/Dockerfile
#
# Description:
#   Definition for Docker image to enable a common development environment
#   across a number of platforms.
#
# Dependencies:
#   We set an empty password for the user, you should change this on first
#   login
#
# Credits:
#   None
#
# Usage:
#   To use the Docker image, either:
#
#   (1) Build the image whilst in the devenv directory:
#
#       docker build -t <reg>/devenv .
#
#   Or
#
#   (2) Download a pre-built image from r.j2o.it:
#
#       docker pull r.j2o.it/devenv
#
#   Then run the container:
#
#       docker run --rm --name devenv \
#          <reg>/devenv
#
#   This image runs Bash as the entry point
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repository root.

ARG TAG=focal
FROM ubuntu:$TAG

ARG USER_EMAIL="dr.j.osborne@gmail.com"
ARG USER_NAME="james"
ARG GIT_NAME="poshboy"

LABEL maintainer="dr.j.osborne@gmail.com"
LABEL status="work in progress"
LABEL version="0.1"

ENV TERM xterm-256color

RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
            apt-get install -y \
            bash \
            ca-certificates \
            curl \
            dnsutils \
            git \
            gnupg \
            iproute2 \
            iputils-ping \
            locales \
            net-tools \
            rsync \
            screen \
            ssh \
            sudo \
            unzip \
            vim \
            wget \
            --no-install-recommends

RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/$(sed -n 's/^DISTRIB_CODENAME=//p' /etc/lsb-release).gpg | apt-key add -
RUN curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/$(sed -n 's/^DISTRIB_CODENAME=//p' /etc/lsb-release).list | tee /etc/apt/sources.list.d/tailscale.list

RUN apt-get update
RUN apt-get install -y \
        tailscale \
        --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

COPY vpn /usr/local/bin/vpn
RUN chmod 0755 /usr/local/bin/vpn

# Install 1Password CLI tool
RUN wget -O /tmp/op_linux_arm_v1.8.0.zip https://cache.agilebits.com/dist/1P/op/pkg/v1.8.0/op_linux_arm_v1.8.0.zip
RUN unzip /tmp/op_linux_arm_v1.8.0.zip -d /usr/local/bin/ && rm /tmp/op_linux_arm_v1.8.0.zip || true;
RUN chmod 0755 /usr/local/bin/op

RUN useradd -ms /bin/bash -p U6aMy0wojraho $USER_NAME
RUN usermod -aG sudo $USER_NAME

USER $USER_NAME
WORKDIR /home/$USER_NAME
RUN mkdir -p -m 0700 ~/.ssh && ssh-keyscan github.com >> ~/.ssh/known_hosts
RUN ls -al ~
VOLUME [ "/home/$USER_NAME/.ssh" ]

RUN git config --global user.email $USER_EMAIL
RUN git config --global user.name $GIT_NAME

RUN mkdir -p Documents/Development
RUN --mount=type=ssh,mode=777,id=github ssh-add -l
RUN --mount=type=ssh,mode=777,id=github git clone --recurse-submodules git@github.com:jjo93sa/dotfiles.git Documents/Development/dotfiles
WORKDIR /home/$USER_NAME/Documents/Development/dotfiles
RUN ./install -f

WORKDIR /home/$USER_NAME

ENTRYPOINT [ "bash" ]
