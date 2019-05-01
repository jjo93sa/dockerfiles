#
# Title:  unix2dos/Dockerfile
#
# Description:  Definition for Docker container to run unix2dos.
#
# Usage:  See .dockerfunc for more details, but in brief:
#
# docker run --rm -it \
#  -v host_dir:/input \
#  -v host_dir2:/output \
#  --name unix2dos \
#  jjo93sa/unix2dos [-n] input [output]
#
# Maintainer: dr.j.osborne@gmail.com
#

FROM alpine:latest

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "work in progress"

RUN apk --no-cache add \
   dos2unix \
   --repository http://dl-3.alpinelinux.org/alpine/edge/community/ --allow-untrusted

#TODO: check we don't need these two volumes
#VOLUME [ "/input" ]
#VOLUME [ "/output" ]

ENTRYPOINT [ "unix2dos" ]
