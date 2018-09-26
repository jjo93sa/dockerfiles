#!/bin/sh

# Define some defaults if no environment variables set on command line
INFLUX_HOST="${INFLUX_HOST:-10.25.10.100}"
INFLUX_PORT="${INFLUX_PORT:-8086}"
INFLUX_USER="${INFLUX_USER:-influx}"
INFLUX_PASS="${INFLUX_PASS:-password}"
INFLUX_DBID="${INFLUX_DBID:-glances}"
INFLUX_PREF="${INFLUX_PREF:-'localhost'}"
INFLUX_TAGS="${INFLUX_TAGS:-'server:docker_def,owner:docker'}"
INFLUX_TIME="${INFLUX_TIME:-30}"

# Make the requisite changes to the configuration file
sed -i "s/^port=.*/port=$INFLUX_PORT/g" /etc/glances/glances.conf
sed -i "s/^user=.*/user=$INFLUX_USER/g" /etc/glances/glances.conf
sed -i "s/^password=.*/password=$INFLUX_PASS/g" /etc/glances/glances.conf
sed -i "s/^host=.*/host=$INFLUX_HOST/g" /etc/glances/glances.conf
sed -i "s/^db=.*/db=$INFLUX_DBID/g" /etc/glances/glances.conf
sed -i "s/^prefix=.*/#prefix=$INFLUX_PREF/g" /etc/glances/glances.conf
sed -i "s/^#tags=.*/tags=$INFLUX_TAGS/g" /etc/glances/glances.conf

# Execute the glances module, specifying the refresh time
# Refresh time can be overwritten with --time in docker run statement
/usr/bin/python -m glances --time ${INFLUX_TIME} "$@"
