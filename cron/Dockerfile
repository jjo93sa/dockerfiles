#
# Title:  cron/Dockerfile
#
# Description:
#   Template image definition that uses cron to execute a bash script, named
#   "task". This image is designed to be used as the basis of another,
#   concrete, image and won't execute on its own.
#
#   The cron specification is incomplete, leaving it up to the dependent image
#   to define the cron frequency of script execution. Users of this template
#   have two methods for applying a cron specification:
#
#     (1) Define a CRON_SPEC ARG _before_ the _first_ FROM in your docker
#         file, which causes it to be a global ARG, and the ONBUILD
#         instructions in this image will adopt the value so specified:
#
#           ARG CRON_SPEC="* * * * *"
#           FROM r.j2o.it/cron
#
#     (2) Define the CRON_SPEC as a build argument in the docker build
#         command, for example:
#
#           docker build --build-arg CRON_SPEC="*/2 * * * *"
#
#   ONBUILD is used in this Dockerfile to trigger instructions in images using
#   this as a base image. One of the ONBUILD instructions assumes that a script
#   named "task" is present in the build context. The build of your image may
#   fail if "task" doesn't exist. And, certainly, nothing will happen when that
#   container is executed. Your image can include any other files needed for
#   your purpose, for example other scripts needed by "task".
#
#   Overall, your "concrete" image file might be as simple as:
#
#     ARG CRON_SPEC=<your spec>
#     FROM r.j2o.it/cron
#
#     # Copy anything else needed in your image:
#     # Funky script myawesomescript is called from task.
#     # Both must be included in build context, i.e. where this Dockerfile is
#     # located.
#     COPY ./myawesomescript /usr/local/bin
#
#   And that's it! -- no ENTRYPOINT/CMD, nothing about task and no other
#   crontab stuff.
#
# Credits:
#   Based on https://gist.github.com/andyshinn/3ae01fa13cb64c9d36e7 and
#   https://forums.docker.com/t/how-to-run-a-cron-job-inside-a-container-alpine/7759/8
#   See also https://www.ekito.fr/people/run-a-cron-job-with-docker/
#
# Usage:
#   Usage is as simple as downloading the latest image of this Docker file from
#   r.j2o.it/cron, and then using that image in the "FROM" of your own
#   Dockerfile. If you want to build your own image of this file, there are
#   four steps:
#
#     (1) Build an image from this Dockerfile
#     (2) Create a task script executing your desired function(s)
#     (3) Create a Dockerfile for your concrete image, copying any other
#         binaries or scripts you need for it to work
#     (4) Build your image
#     (5) Execute your concrete image
#
# Maintainer:
#   James Osborne, dr.j.osborne@gmail.com
#
# License:
#   MIT, see file in repository root.
#
ARG TAG=latest
FROM alpine:$TAG

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "1.1"
LABEL status "production"

# Install bash
RUN apk --no-cache add bash

# We declare CRON_SPEC here so that it might be changed by Dockerfiles
# implementing this image, wherein these "derived" images either need to
# specify CRON_SPEC before the first FROM in the Dockerfile, or pass it as an
# --build-arg parameter on the docker build command line.
ONBUILD ARG CRON_SPEC

# Edit the root crontab, executing script task as requested at build time
# (through build-arg or global ARG definition before first FROM)
ONBUILD RUN sed -i -e "\$aSHELL=/bin/bash" \
                   -e "\$a${CRON_SPEC}    /usr/local/bin/task\n" \
                   /etc/crontabs/root

# Set our working directory using ONBUILD to ensure this is done in image
# based off this
ONBUILD WORKDIR /usr/local/bin

# Copy task script but do this step in the image based off this
ONBUILD COPY ./task .

# Change permission of task to executable
ONBUILD RUN chmod 0755 /usr/local/bin/task

# Execute this command in images based on this
# ENTRYPOINT runs the crond executable, in the foreground (-f), and sets a log
# level (-l) in conjunction with CMD. Loglevel can be changed at runtime
ONBUILD ENTRYPOINT [ "crond",  "-f", "-l" ]
ONBUILD CMD [ "8" ]
