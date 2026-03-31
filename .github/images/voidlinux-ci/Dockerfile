# CI bootstrap image for Void Linux.
#
# voidlinux/voidlinux:latest ships bash but lacks git, tar, gzip, and
# ca-certificates — all required by actions/checkout and actions/setup-go
# before any run: step can execute. This image adds those tools so the
# GitHub Actions runner can launch steps normally.
#
# Void uses xbps; there is no /bin/sh by default in the slim image so we
# create the symlink explicitly.
#
# VOIDLINUX_REPO can be overridden at build time to use a different mirror
# if the default (alpha.de.repo.voidlinux.org) is unreachable from CI.
ARG VOIDLINUX_REPO=https://repo-us.voidlinux.org/current

FROM voidlinux/voidlinux:latest

ARG VOIDLINUX_REPO

RUN echo "repository=${VOIDLINUX_REPO}" > /etc/xbps.d/00-repository-main.conf \
 && xbps-install -Syu xbps \
 && xbps-install -Sy \
      bash \
      tar \
      gzip \
      git \
      ca-certificates \
 && ln -sf /bin/bash /bin/sh \
 && xbps-remove -Oo

# Verify the symlink and tools are present.
RUN sh --version && tar --version && git --version
