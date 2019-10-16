#
# Title:  proxy/Dockerfile
#
# Description:
#   An http/https proxy based on the official nginx:alpine image. Added tools
#   to help with debugging connections. An exmaple nginx.conf file is included
#   as a guideline.
#
# Credits:
#   None.
#
# Usage:
#   Usage is as simple as these three steps:
#
#   (1) Build an image from this Dockerfile
#
#         docker build -t <reg>/proxy .
#
#   (2) Launch a container from the image somthing like this:
#
#         docker run -it --rm --name roxy \
#           -v /path/to/nginx.conf:/etc/nginx/nginx.conf:ro \
#           -v /path/to/nginx.htpasswd:/etc/nginx/conf.d/nginx.htpasswd:ro \
#           -v /path/to/dhparams.pem:/etc/nginx/conf.d/dhparams.pem \
#           -v /path/to/certs:/etc/letsencrypt:ro
#           <reg>/proxy
#
#       Of course, the file locations can be modified in your nginx.conf file.
#       I'll leave the network aspects up to the reader.
#
#   (3) Enjoy proxy'd services and debugging with the new tools.
#
#   Alternatively, you can download pre-built images, see r.j2o.it.
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see file in repository root.
#
ARG TAG=alpine
FROM nginx:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.2"
LABEL status "development"

# Add the following packages for debugging purposes
RUN apk --no-cache add bash \
                       bind-tools \
                       curl \
                       nmap

EXPOSE 443
