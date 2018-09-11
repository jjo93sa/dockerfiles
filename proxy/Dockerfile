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
#   Usage is as simple as these four steps:
#   (1) Build an image from this Dockerfile 
#   (2) Launch a container from the image somthing like this:
#
#         docker run -it --rm --name roxy \
#           -v /path/to/nginx.conf:/etc/nginx/nginx.conf:ro \
#           -v /path/to/nginx.htpasswd:/etc/nginx/conf.d/nginx.htpasswd:ro \
#           -v /path/to/dhparams.pem:/etc/nginx/conf.d/dhparams.pem \
#           -v /path/to/certs:/etc/letsencrypt:ro
#           jjo93sa/proxy
#
#       Of course, the file locations can be modified in your nginx.conf file.
#       I'll leave the network aspects up to the reader.
#
#   (3) Enjoy proxy'd services and debugging with the new tools.
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see file in repository root.
#
ARG BASE_IMAGE=nginx:alpine
FROM $BASE_IMAGE

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "0.1"
LABEL status "development"

# Add the following packages for debugging purposes
RUN apk --no-cache update && apk --no-cache add bash bind-tools curl nmap

EXPOSE 443


