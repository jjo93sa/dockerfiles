#
# Title:  cron-certbot/Dockerfile
#
# Description:
#   Docker file specifying a container to run EFF/Let's Encrypt client in cron
#   for automated certificate generation and renewal. I based this on the
#   Dockerfile found here: https://github.com/pslobo/dockerized-certbot
#
#   Support for Intel & ARM builds is provided by the "ARCH" ARG parameter,
#   which may be set at build time using --build-arg.
#
# Dependencies:
#   (1) Uses the Docker image r.j2o.it/cron
#
# Credits:
#   Credit to pslobo for working out the incantations for getting certbot
#   to run under Alpine Linux. I found the official image in Alpine didn't work
#   and was defeated in my own attempts to build the software in Docker. Do
#   check out the repo at: https://github.com/pslobo/dockerized-certbot
#
# Usage:
#   Usage of this Docker image is very simple, pull and execute
#   r.j2o.it/cron_certbot.
#
#   Alternatively, if you want more control over the build-time parameters, you
#   can build the image as follows:
#
#     (1) Create an image of cron/Dockerfile (or pull it from r.j2o.it/cron),
#         which is required in the next step
#     (2) Build this file, specifying the correct repository and cron times:
#
#           docker build --build-arg CRON_SPEC="<insert>" \
#                        -t <reg>/cron_certbot .
#
#     (3) Run a container from the image built in (2):
#
#           docker run -d -p80:80 -v /path/on/host:/etc/letsencrypt \
#                    -e "YOUR_EMAIL=you@mail.og" \
#                    -e "CERT_DOMAIN=you.org,another.org" \
#                    -e "STAGING=no" \
#                     --rm --name cron-certbot <reg>/cron-certbot
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   GPLv3 - this is a derivative work made on 2018-09-09 from this repository:
#   https://github.com/pslobo/dockerized-certbot Hence this file, and only this
#   file, is covered under the same license, see LICENSE-GPL.
#
#   In accordance with the aggregate clause in GPLv3, every other file of this repository
#   (https://github.com/jjo93sa/dockerfiles.git) is licensed under MIT. Only
#   this file is covered by GPLv3. See the LICENSE file in the repoistory root.
#
ARG CRON_SPEC="29 17 * * 3"
ARG REG=r.j2o.it
ARG TAG=latest
FROM alpine:$TAG AS intermediate

# Install git in intermediate
RUN apk --no-cache add git

# Clone the repos we need
RUN git clone https://github.com/certbot/certbot.git


# Second phase
FROM $REG/cron:$TAG
LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.5"
LABEL status "test"

VOLUME /etc/letsencrypt /var/lib/letsencrypt

WORKDIR /opt/certbot

# Copy certbot code
COPY --from=intermediate /certbot/CHANGELOG.md src/
COPY --from=intermediate /certbot/README.rst src/
COPY --from=intermediate /certbot/tools tools
COPY --from=intermediate /certbot/acme src/acme
COPY --from=intermediate /certbot/certbot src/certbot

# Build Python because distro package doesn't seem to work
#
# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# runtime dependencies
RUN set -eux; \
	apk add --no-cache \
# install ca-certificates so that HTTPS works consistently
		ca-certificates \
	;
# other runtime dependencies for Python are installed later

ENV GPG_KEY E3FF2839C048B25C084DEBE9B26995E310250568
ENV PYTHON_VERSION 3.8.11

RUN set -ex \
	&& apk add --no-cache --virtual .fetch-deps \
		gnupg \
		tar \
		xz \
	\
	&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& { command -v gpgconf > /dev/null && gpgconf --kill all || :; } \
	&& rm -rf "$GNUPGHOME" python.tar.xz.asc \
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& apk add --no-cache --virtual .build-deps  \
		bluez-dev \
		bzip2-dev \
		coreutils \
		dpkg-dev dpkg \
		expat-dev \
		findutils \
		gcc \
		gdbm-dev \
		libc-dev \
		libffi-dev \
		libnsl-dev \
		libtirpc-dev \
		linux-headers \
		make \
		ncurses-dev \
		openssl-dev \
		pax-utils \
		readline-dev \
        rust \
		sqlite-dev \
		tcl-dev \
		tk \
		tk-dev \
		util-linux-dev \
		xz-dev \
		zlib-dev \
# add build deps before removing fetch deps in case there's overlap
	&& apk del --no-network .fetch-deps \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-loadable-sqlite-extensions \
		--enable-optimizations \
		--enable-option-checking=fatal \
		--enable-shared \
		--with-system-expat \
		--with-system-ffi \
		--without-ensurepip \
	&& make -j "$(nproc)" \
# set thread stack size to 1MB so we don't segfault before we hit sys.getrecursionlimit()
# https://github.com/alpinelinux/aports/commit/2026e1259422d4e0cf92391ca2d3844356c649d0
		EXTRA_CFLAGS="-DTHREAD_STACK_SIZE=0x100000" \
		LDFLAGS="-Wl,--strip-all" \
	&& make install \
	&& rm -rf /usr/src/python \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
			-o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name '*.a' \) \) \
			-o \( -type f -a -name 'wininst-*.exe' \) \
		\) -exec rm -rf '{}' + \
	\
	&& find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec scanelf --needed --nobanner --format '%n#p' '{}' ';' \
		| tr ',' '\n' \
		| sort -u \
		| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
		| xargs -rt apk add --no-cache --virtual .python-rundeps \
	&& apk del --no-network .build-deps \
	\
	&& python3 --version

# make some useful symlinks that are expected to exist
RUN cd /usr/local/bin \
	&& ln -s idle3 idle \
	&& ln -s pydoc3 pydoc \
	&& ln -s python3 python \
	&& ln -s python3-config python-config

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 21.2.1
# https://github.com/pypa/get-pip
ENV PYTHON_GET_PIP_URL https://github.com/pypa/get-pip/raw/a1675ab6c2bd898ed82b1f58c486097f763c74a9/public/get-pip.py
ENV PYTHON_GET_PIP_SHA256 6665659241292b2147b58922b9ffe11dda66b39d52d8a6f3aa310bc1d60ea6f7

RUN set -ex; \
	\
	wget -O get-pip.py "$PYTHON_GET_PIP_URL"; \
	echo "$PYTHON_GET_PIP_SHA256 *get-pip.py" | sha256sum -c -; \
	\
	python get-pip.py \
		--disable-pip-version-check \
		--no-cache-dir \
		"pip==$PYTHON_PIP_VERSION" \
	; \
	pip --version; \
	\
	find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests -o -name idle_test \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' +; \
	rm -f get-pip.py

# Install certbot runtime dependencies
RUN apk add --no-cache --virtual .certbot-deps \
        libffi \
        libssl1.1 \
        openssl \
        ca-certificates \
        binutils

# Install certbot from sources
RUN apk add --no-cache --virtual .build-deps \
        gcc \
        linux-headers \
        openssl-dev \
        musl-dev \
        libffi-dev \
        python3-dev \
        cargo \
        && python3 tools/pipstrap.py \
        && python3 tools/pip_install.py --no-cache-dir \
            --editable src/acme \
            --editable src/certbot \
        && apk del .build-deps \
        && rm -rf ${HOME}/.cargo

EXPOSE 80 443

# NOTE: ENTRYPOINT is provided in the base image
