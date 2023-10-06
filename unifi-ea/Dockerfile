ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base:8.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        binutils=2.34-6ubuntu1.6 \
        jsvc=1.0.15-8 \
        libcap2=1:2.32-1 \
        logrotate=3.14.0-4ubuntu3 \
        mongodb-server \
        openjdk-17-jre-headless \
        ca-certificates-java \
    \
    && curl -J -L -o /tmp/unifi.deb \
        "https://dl.ui.com/unifi/7.5.187-f57f5bf7ab/unifi_sysvinit_all.deb" \
    \
    && dpkg --force-all --install /tmp/unifi.deb \
    && apt-get clean \
    && rm -fr \
        /tmp/* \
        /var/cache/* \
        /var/lib/apt/lists/* \
        /var/log/*.log \
        /var/log/apt

# Copy root filesystem
COPY rootfs /

# Health check
HEALTHCHECK --start-period=30m \
    CMD curl --insecure --fail https://localhost:8443 || exit 1

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} 
