#
# Title:  aws-cli/Dockerfile
#
# Description:
#   Definition for Docker image to run the AWS CLI. Running this in Docker
#   for isolation and to avoid any problems with Python version clashes
#
# Dependencies:
#   You need an aws account and in particular secret keys for aws-cli access.
#
# Credits:
#   As usual with all things Docker much of this is due to Jess Frazelle:
#   https://github.com/jessfraz/dockerfiles.git
#
# Usage:
#   To use this Docker container:
#
#   (0) Sign-up for an AWS account and retrieve the Access Key ID and Secret
#       Access Key ID;
#
#   (1) Create an aws-cli.env file containing the parameters from (0), an
#       example file is supplied in the repository;
#
#   (2) Build docker image from within the directory containing this Dockerfile:
#
#       docker build -i <repo>/aws-cli .
#
#       Replacing <repo> with your repository name;
#
#   (3) Run Docker container like this:
#
#          docker run -it --name aws-cli \
#               --env-file aws-cli.env   \
#               <repo>/aws-cli <params>
#
#       Where <params> is the list ot commands and parameters to pass to the
#       aws-cli executable. If no <params> are given, executes help
#
# Maintainer: dr.j.osborne@gmail.com
#
# License: MIT, see LICENSE file in repoistory root.
#
FROM alpine:latest

LABEL maintainer "dr.j.osborne@gmail.com"
LABEL status "production"
LABEL version "1.0"

RUN apk --no-cache add ca-certificates \
	groff \
	less \
	python \
	py2-pip

RUN pip install awscli

# We don't need to run as root, so create new user & group and use that
RUN addgroup -S aws && adduser -S aws -G aws
USER aws

ENTRYPOINT [ "aws" ]
CMD [ "help" ]
