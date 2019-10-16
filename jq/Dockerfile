# Title:  jq/Dockerfile
#
# Description:
#   Definition for Docker container to run jq, which is useful for macOS,
#   which doesn't have jq.
#
# Dependencies:
#   None
#
# Credits:
#   None
#
# Usage:
#   To use the Docker container either:
#
#   (1) Build the image whilst in the nmap directory:
#
#       docker build -t <reg>/jq .
#
#   (2) Download a prebuilt image from r.j2o.it:
#
#       docker pull r.j2o.it/jq
#
#   Run the container:
#
#       docker run -it --rm --name jq <reg>/jq <params>
#
#   If the container is run without any parameters, it defaults to showing
#   the jq help (jq -h).
#
#   A function like this:
#
#      function jq()
#       {
#           read -d '' IN;
#           echo -n "$IN" | docker run -i <repo>/jq "$*"
#       }
#
#   can be used to add pipe like functionality to your container:
#
#       curl -s ipinfo.io | jq '.ip'
#
#   Where the jq invoked here is the function above, which creates the
#   container and passes the data received from the pipe.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see LICENSE file in repoistory root.
#
ARG TAG=latest
FROM alpine:$TAG

LABEL maintainer "James Osborne <dr.j.osborne@gmail.com>"
LABEL status "work in progress"
LABEL version "0.3"

RUN apk --no-cache add jq

ENTRYPOINT [ "jq" ]
CMD [ "-h" ]
