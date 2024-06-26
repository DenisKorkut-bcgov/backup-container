# This image provides a postgres installation from which to run backups 
# We use the official PostgreSQL 15.1 here (in place of CentOS and PostgreSQL 13)
FROM postgres:15.6-alpine3.18

RUN apk add --no-cache bash
RUN apk add --no-cache coreutils && apk add --no-cache util-linux


# Change timezone to PST for convenience
ENV TZ=PST8PDT

# Set the workdir to be root
WORKDIR /

# Load the backup scripts into the container (must be executable).
COPY backup.* /

# ========================================================================================================
# Install go-crond (from https://github.com/webdevops/go-crond)
#
# CRON Jobs in OpenShift:
#  - https://blog.danman.eu/cron-jobs-in-openshift/
# --------------------------------------------------------------------------------------------------------
ARG SOURCE_REPO=webdevops
ARG GOCROND_VERSION=22.9.1
ADD https://github.com/$SOURCE_REPO/go-crond/releases/download/$GOCROND_VERSION/go-crond.linux.amd64 /usr/bin/go-crond

USER root

RUN chmod +x /usr/bin/go-crond
# ========================================================================================================

# ========================================================================================================
# Perform operations that require root privilages here ...
# --------------------------------------------------------------------------------------------------------
RUN echo $TZ > /etc/timezone
# ========================================================================================================

# Important - Reset to the base image's user account.
USER 26

# Set the default CMD.
CMD /bin/bash /backup.sh
