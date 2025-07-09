ARG BUILD_FROM=ghcr.io/hassio-addons/base-python:17.0.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set workdir
WORKDIR /tmp

# Copy Python requirements file
COPY requirements.txt /tmp/

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV PYTHONUNBUFFERED=1
ENV PYCURL_SSL_LIBRARY=openssl
ARG UNRAR_VERSION="7.1.8"
ARG BUILD_ARCH="amd64"
# Execute during the build of the image

# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        libffi-dev=3.4.8-r0 \
        build-base=0.5-r3 \
        curl-dev=8.14.1-r1 \
    \
    && pip --no-cache-dir install \
        pycurl==7.45.6 \
    \
    && curl -sSLf -o unrarsrc.tar.gz "https://www.rarlab.com/rar/unrarsrc-${UNRAR_VERSION}.tar.gz" && \
    tar -zxf "/tmp/unrarsrc.tar.gz" && \
    cd unrar && make -f makefile && \
    install -v -m755 unrar /usr/bin && \
    apk add --no-cache \
        7zip=24.09-r0 \
        sqlite=3.49.2-r0 \
        ffmpeg=6.1.2-r2 \
        tesseract-ocr=5.5.0-r2 \
    \
    && pip --no-cache-dir install -r /tmp/requirements.txt --no-binary cffi \
    \
    && rm -rf /tmp/* /root/.cache \
    && apk del --purge .build-dependencies


# Copy root filesystem
COPY rootfs /


ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="tr4nt0r (https://github.com/tr4nt0r)" \
