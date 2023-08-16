ARG BUILD_FROM=ghcr.io/hassio-addons/ubuntu-base:8.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        dirmngr \
        gnupg2 \
    && echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/3.6 main" \
        > /etc/apt/sources.list.d/mongodb-org.list \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        binutils=2.34-6ubuntu1.6 \
        jsvc=1.0.15-8 \
        libcap2=1:2.32-1 \
        logrotate=3.14.0-4ubuntu3 \
        mongodb-org-server \
        openjdk-17-jre-headless \
        ca-certificates-java \
    \
    && curl -J -L -o /tmp/unifi.deb \
        "https://dl.ui.com/unifi/7.5.171-8c80ec945b/unifi_sysvinit_all.deb" \
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
HEALTHCHECK --start-period=5m \
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
