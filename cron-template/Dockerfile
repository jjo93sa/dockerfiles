#
# Title:  cron-template/Dockerfile
#
# Description:
#   Template to execute bash script, named "task", regularly with cron. This
#   image is designed to be used as the basis of another, concrete image and
#   won't execute on its own.
#
#   Cron frequency defaults to every minute (* * * * * task) but may be
#   overridden at image build time using --build-arg CRON_SPEC="<string>". For
#   example:
#
#   docker build --build-arg CRON_SPEC="*/2 * * * *" <rest of build command>
#
#   will build an image executing task every two minutes.
#
#   ONBUILD is used in this Dockerfile to trigger instructions in images using
#   this as a base image. One of the ONBUILD instructions assumes that a script
#   named "task" is present in the build context. The build of your image may
#   fail if "task" doesn't exist. And, certainly, nothing will happen when that
#   container is run. Your image can include any other files needed for your
#   purpose, for example other scripts needed by "task".
#
#   Overall, your "concrete" image file might be as simple as:
#
#     FROM jjo93sa/cron-template
#
#     # Copy anything else needed in your image:
#     # Funky script myawesomescript is called from task.
#     # Both must be included in build context, i.e. where this Dockerfile is
#     # located.
#     COPY ./myawesomescript /usr/local/bin
#
#   And that's it - no ENTRYPOINT/CMD, nothing about task.sh or crontab stuff.
#
# Credits:
#	 Based on https://gist.github.com/andyshinn/3ae01fa13cb64c9d36e7 and
#	 https://forums.docker.com/t/how-to-run-a-cron-job-inside-a-container-alpine/7759/8
#	 See also https://www.ekito.fr/people/run-a-cron-job-with-docker/
#
# Usage:
#   Usage is as simple as these four steps:
#   (1) Build an image from this Dockerfile with the CRON_SPEC of your choice;
#   (2) Create a task script executing your desired function;`
#   (3) Create a Dockerfile for your concrete image, copying any other binaries
#       or scripts you need for it to work;
#   (4) Build your image;
#   (5) Execute your concrete image.
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see file in repository root.
#
ARG BASE_IMAGE=alpine
FROM $BASE_IMAGE

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL version "1.0"
LABEL status "production"

# Install bash
RUN apk --no-cache update && apk --no-cache add bash

# Default, every minute, cron entry. Can be changed during container build.
ONBUILD ARG CRON_SPEC="* * * * *"

# Edit the root crontab, executing script task.sh as requested at build time
# (through build-arg), default to running once per minute per ARG above
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
