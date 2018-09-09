ARG BASE_IMAGE=alpine
FROM $BASE_IMAGE

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.1"
LABEL status "test"

RUN apk update && apk add --no-cache \
	bash \
	git \
	openssh

# Generate keys for the ssh server
RUN ssh-keygen -A

# Add a custom sshd_config file
COPY sshd_config /etc/ssh/sshd_config

# Add a user called git, set shell and activate (with no password?)
# git shell is installed as part of git, it restricts shell access to git cmds
# See: https://git-scm.com/docs/git-shell
RUN adduser -D -s /usr/bin/git-shell git \
	&& passwd -u git

# Set the working directory, we'll mount this in a data container
WORKDIR /home/git

# Create the files and directories needed for user git's ssh
RUN  mkdir .ssh                   \
	&& touch .ssh/authorized_keys \
	&& chown -R git:git .ssh      \
	&& chmod 700 .ssh             \
	&& chmod 600 .ssh/authorized_keys

# Copy over our custom git-shell-commands: mkrepo, addkey
COPY git-shell-commands /home/git/git-shell-commands

# Copy over our template README.md
COPY README.md /home/git/

# Create a directory for our repos
RUN mkdir repos &&            \
	chown -R git:git repos && \
	chmod -R ug+rwX repos

EXPOSE 22

# Run the ssh server (as root?); -D ensures server doesn't run as a daemon
ENTRYPOINT [ "/usr/sbin/sshd" ]
CMD [ "-D" ]
