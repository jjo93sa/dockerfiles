#
# Title:  glances/Dockerfile
#
# Description:
#
# Dependencies:
#
# Credits:  None
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

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.1"
LABEL status "development"

# Install python, plus some development tools to build later packages
RUN apk --no-cache update && \
	apk --no-cache add alpine-sdk \
					linux-headers \
					python \
					python-dev \
					py-pip && \
	# Use pip to install glances and modules we need
    pip install glances[ip,folders,docker,cpuinfo,action] && \
	pip install bernhard \
				couchdb \
			   	elasticsearch \
			   	influxdb \
			   	kafka-python \
			   	pika \
			   	potsdb \
			   	prometheus-client \
			   	statsd \
			   	protobuf \
			   	futures \
			   	python-dateutil \
			   	pytz && \
	# Tidy-up the development tools, which we don't need any more.
	apk del alpine-sdk python-dev py-pip linux-headers

# Comment out these two lines because they cause exceptions on the RPi
RUN sed -i -e "s/self.stats\['cpu_hz_current'\] = /#self.stats['cpu_hz_current'] = /g" \
		-e "s/self.stats\['cpu_hz'\] = /#self.stats['cpu_hz'] = /g" \
		/usr/lib/python2.7/site-packages/glances/plugins/glances_quicklook.py

# Copy over the asstes files: config and entry script
RUN mkdir -p /etc/glances
COPY glances.conf /etc/glances/
COPY start-glances.sh /usr/local/bin

# Give the entry script executable perms
RUN chmod 755 /usr/local/bin/start-glances.sh

# Define default command.
ENTRYPOINT [ "/usr/local/bin/start-glances.sh", "-C", "/etc/glances/glances.conf" ]

# Define default parameters, making this glances output to InfluxDB
# Can override these commands, for example with --time <s> to see curses output
CMD [ "--export", "influxdb", "--quiet" ]
