## -*- docker-image-name: "scaleway/alpine:latest-stable" -*-
ARG MULTIARCH_ARCH
FROM multiarch/alpine:${MULTIARCH_ARCH}-latest-stable

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)


# Environment
ENV SCW_BASE_IMAGE scaleway/alpine:latest-stable


# Adding and calling builder-enter
COPY ./overlay-base/usr/local/sbin/scw-builder-enter /usr/local/sbin/
RUN /bin/sh -e /usr/local/sbin/scw-builder-enter


# Install packages
RUN apk update \
 && apk add \
    bash \
    busybox-suid \
    curl \
    openssh \
    tar \
    wget \
 && apk upgrade \
    openssl \
 && rm -rf /var/cache/apk/*


# Patch rootfs
COPY ./overlay/ ./overlay-base/ /


# Configure autostart packages
RUN rc-update add sshd default\
 && rc-update add scw-ssh-keys default \
 && rc-update add ntpd default \
 && rc-update add hostname default \
 && rc-update add update-motd default \
 && rc-update add sysctl default \
 && rc-update add scw-sync-kernel-extra default \
 && rc-update add scw-signal-booted default \
 && rc-update add scw-initramfs-shutdown shutdown \
 && rc-status


# Update permissions
RUN chmod 700 /root


# Clean rootfs from image-builder
RUN /usr/local/sbin/scw-builder-leave
