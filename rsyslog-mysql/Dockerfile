# Dockerfile for rsyslog/rsyslog (Standard Image) based on Ubuntu 24.04 LTS
# This container provides a general-purpose rsyslog setup with common modules.
# It is based on the rsyslog/rsyslog-minimal image.

# Build arguments passed from the Makefile.
# BASE_IMAGE_TAG will be the full tag of the minimal image (e.g., rsyslog/rsyslog-minimal:2025-04).
ARG BASE_IMAGE_TAG="rsyslog/rsyslog-minimal:latest" # Or a specific version
ARG UBUNTU_VERSION="24.04" # Inherited from base, but good to keep for consistency/labels
ARG RSYSLOG_IMG_VERSION="unset" # Version passed from Makefile for image metadata

# Use the rsyslog/rsyslog-minimal image as the base.
# This ensures all the core rsyslog setup (PPA, basic install, permissions) is inherited.
FROM ${BASE_IMAGE_TAG}

# Re-declare ARGs after FROM to make them available to subsequent instructions like LABEL.
ARG UBUNTU_VERSION
ARG RSYSLOG_IMG_VERSION
ARG BASE_IMAGE_TAG

LABEL maintainer="Weslley Silva <weslleycsil@gmail.com>"
LABEL description="rsyslog container based on Ubuntu ${UBUNTU_VERSION} with mysql."
LABEL com.adiscon.rsyslog.image.version="${RSYSLOG_IMG_VERSION}"
# Explicitly label the base image used for traceability.
LABEL com.adiscon.rsyslog.base.image="${BASE_IMAGE_TAG}"

# Set DEBIAN_FRONTEND to noninteractive to prevent interactive prompts during package installation.
ENV DEBIAN_FRONTEND=noninteractive

# Install common rsyslog modules suitable for a standard image.
# - rsyslog-imhttp: For the built-in HTTP server (health, metrics, potential future ingestion).
# - rsyslog-omhttp: For forwarding logs via HTTP.
# apt-get update is run to ensure the package lists are fresh before installing.
RUN apt-get update && \
    apt-get install -y --no-install-recommends rsyslog-imhttp rsyslog-omhttp rsyslog-mysql && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /etc/rsyslog.d/mysql.conf

# Copy any standard configuration snippets if needed.
COPY 10-collector.conf 50-mysql-logging.conf /etc/rsyslog.d/

# Expose standard syslog ports.
# These ports will be exposed by default for UDP and TCP syslog reception.
EXPOSE 514/udp
EXPOSE 514/tcp

ENV ENABLE_UDP=on
ENV ENABLE_TCP=on

ENV WRITE_MYSQL=on
# Define the container role for the entrypoint script
ENV RSYSLOG_ROLE=collector

# Inherit CMD from base image (rsyslogd -n -f /etc/rsyslog.conf), no need to set explicitly
# unless specific default arguments for the standard role are needed.