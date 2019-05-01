# Title:  jq/Dockerfile
#
# Description:  Definition for Docker container to run jq json parser
#
# Usage:  See .dockerfunc for more details, but in brief:
#
# docker run -i jjo93sa/jq "$*"
#
# Maintainer: dr.j.osborne@gmail.com
#
FROM debian:jessie

LABEL maintainer "James Osborne <dr.j.osborne@gmail.com>"
LABEL status "work in progress"

RUN apt-get update && apt-get install -y \
   jq \
   --no-install-recommends \
   && rm -rf /var/lib/apt/lists/*

ENTRYPOINT [ "jq" ]
