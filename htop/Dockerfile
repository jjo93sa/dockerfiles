# Title:  htop/Dockerfile
#
# Description:  Definition for Docker container to run htop
#  Based on the work of Jessie Frazelle
#  (https://blog.jessfraz.com/post/docker-containers-on-the-desktop/)
#  Monitors only xhyve VM on macOS
#
# Usage:  See .dockerfunc for more details, but in brief:
#
# docker run --rm -it \
#  --pid host \
#  jjo93sa/htop
#
# Maintainer: dr.j.osborne@gmail.com
#
FROM alpine:latest

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"

RUN apk --no-cache add \
   htop

ENTRYPOINT [ "htop" ]
