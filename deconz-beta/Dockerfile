ARG BUILD_FROM
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install deCONZ dependencies
ARG BUILD_ARCH
RUN \
    set -x \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        iproute2 \
        iputils-ping \
        kmod \
        libatomic1 \
        libcap2-bin \
        libqt5core5a \
        libqt5gui5 \
        libqt5network5 \
        libqt5serialport5 \
        libqt5sql5 \
        libqt5websockets5 \
        libqt5widgets5 \
        libqt5qml5 \
        qtdeclarative5-dev \
        libssl1.1 \
        lsof \
        netcat \
        nginx \
        novnc \
        sqlite3 \
        tigervnc-common \
        tigervnc-standalone-server \
        udev \
        wget \
        websockify \
        openbox \
        xfonts-base \
        xfonts-scalable \
    && rm -rf /var/lib/apt/lists/* \
    && if [[ "armhf aarch64" = *"$BUILD_ARCH"* ]]; \
        then \
            apt-get update \
            && apt-get install -y --no-install-recommends \
                build-essential \
                git \
            && git clone --depth 1 https://github.com/WiringPi/WiringPi /usr/src/wiringpi \
            && cd /usr/src/wiringpi/wiringPi \
            && make \
            && make install \
            && cd ../devLib \
            && make \
            && make install \
            && cd ../gpio \
            && make \
            && make install \
            && apt-get purge -y --auto-remove \
                build-essential \
                git \
            && rm -rf \
                /var/lib/apt/lists/* \
                /usr/src/wiringpi; \
        fi

# Install deCONZ
ARG DECONZ_VERSION

RUN \
    set -x \
    && if [ "${BUILD_ARCH}" = "armhf" ]; \
        then \
            curl -q -L -o /deconz.deb http://deconz.dresden-elektronik.de/raspbian/beta/deconz-${DECONZ_VERSION}-qt5.deb; \
        elif [ "${BUILD_ARCH}" = "aarch64" ]; \
        then \
            curl -q -L -o /deconz.deb http://deconz.dresden-elektronik.de/debian/beta/deconz_${DECONZ_VERSION}-debian-buster-beta_arm64.deb; \
        else \
            curl -q -L -o /deconz.deb http://deconz.dresden-elektronik.de/ubuntu/beta/deconz-${DECONZ_VERSION}-qt5.deb; \
        fi \
    && dpkg -i /deconz.deb \
    && rm -f /deconz.deb \
    && chown root:root /usr/bin/deCONZ* \
    && sed -i 's/\/root/\/data/' /etc/passwd

COPY rootfs /
