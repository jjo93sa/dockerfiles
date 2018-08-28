ARG BASE_IMAGE=alpine
FROM $BASE_IMAGE

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.1"
LABEL status "test"

RUN apk update && apk add --no-cache \
	bash \
	git \
	openssh

# Key generation on the server
RUN ssh-keygen -A

# sshd_config file is edited for enable access key and disable access password
COPY sshd_config /etc/ssh/sshd_config

RUN adduser -D -s /usr/bin/git-shell git \
	#&& echo git:12345 | chpasswd \
	&& passwd -u git

WORKDIR /home/git

RUN  mkdir .ssh     \
	&& touch .ssh/authorized_keys \
	&& chown -R git:git .ssh  \
	&& chmod 700 .ssh             \
	&& chmod 600 .ssh/authorized_keys

# More info: https://git-scm.com/docs/git-shell
COPY git-shell-commands /home/git/git-shell-commands
COPY README.md /home/git/

RUN mkdir repos && \
	chown -R git:git repos && \
	chmod -R ug+rwX repos

EXPOSE 22

VOLUME /home/git/repos

ENTRYPOINT [ "/usr/sbin/sshd" ]
CMD [ "-D" ]
