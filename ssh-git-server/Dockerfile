#
# Title:  ssh-git-server/Dockerfile
#
# Description:
#   Definition of a Dockerfile to build an image running ssh, with the sole
#   purpose of providing a Git server. No shell access is allowed, but there
#   are commands that may be executed on the server for common Git and ssh
#   operations, like adding public keys, creating repositories.
#
# Dependencies:
#   None
#
# Credits:
#   Inspiration for the git-shell-commands from the official Git repository:
#   https://github.com/git/git/tree/master/contrib/git-shell-commands
#
#   And other inspiration from: https://github.com/jkarlosb/git-server-docker
#
# Usage:
#   Because all access to the Git server is through ssh, and only public-key
#   authentication is allowed, it is necessary to configure a public key in
#   the container. There are two chances to do this: at build time or at run
#   time:
#
#   (a) Build time - copy your own authorized_keys file into the build
#       context and build an image from this Dockerfile. The authorized_keys
#       file will be automatically copied into user git's .ssh folder with
#       the correct permissions;
#   (b) Run time - after building the default image from this Docker file, it
#       is possible to specify a single public key as the only argument to the
#       Docker run command:
#
#          docker run -p 22122:22 -it r.j2o.it/ssh-git-server "<pub-key>"
#
#   Either way, additional public keys may be added using the addkey command:
#
#          ssh -p 22122 git@ssh-git-server addkey "<pub-key>"
#
#   Building this Dockerfile is standard. Or simply docker pull it from:
#   r.j2o.it.
#
#   Currently, the following commands are supported from ssh:
#
#       - addkey -- adds a new public key to authorized_keys
#       - lsrepo -- lists all known repositories
#       - mkrepo -- makes a new repository
#
#   All git repositories are stored in /home/git/repos. You can mount this
#   directory on the host to ensure the repositories are retained across
#   restarts:
#
#       docker run -p 22:22 -v /host/path:/home/git/repos ...
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
LABEL version "0.2"
LABEL status "test"

RUN apk add --no-cache bash \
                       git \
                       openssh

# Generate keys for the ssh server
RUN ssh-keygen -A

# Add a custom sshd_config file
COPY sshd_config /etc/ssh/sshd_config

# Add our entrypoint script
COPY entrypoint /usr/local/bin
RUN chmod 755 /usr/local/bin/entrypoint

# Add a user called git, set shell and activate (with no password?)
# git shell is installed as part of git, it restricts shell access to git cmds
# See: https://git-scm.com/docs/git-shell
RUN adduser -D -s /usr/bin/git-shell git && \
    passwd -u git

# Set the working directory, we'll mount this in a data container
WORKDIR /home/git

# Create the files and directories needed for user git's ssh
RUN  mkdir .ssh              && \
     chown -R git:git .ssh   && \
     chmod 700 .ssh

# Copy the authorized_keys file and ensure permissions and ownership are good
COPY authorized_keys .ssh/
RUN chmod 600 .ssh/authorized_keys && chown git:git .ssh/authorized_keys

# Copy over our custom git-shell-commands: mkrepo, addkey
COPY git-shell-commands /home/git/git-shell-commands

# Copy over our template README.md
COPY README.md /home/git/

# Create a directory for our repos
RUN mkdir repos &&            \
    chown -R git:git repos && \
    chmod -R ug+rwX repos

EXPOSE 22

# Execute the entrypoint script, which copies the specified public key to
# authorized keys (if given) and exec's the ssh server; -D ensures server
# doesn't run as a daemon
ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
