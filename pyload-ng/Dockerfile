ARG BUILD_FROM=ghcr.io/hassio-addons/base-python:17.0.0
FROM ${BUILD_FROM}

# Set workdir
WORKDIR /tmp

# Copy Python requirements file
COPY requirements.txt /tmp/

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV PYTHONUNBUFFERED=1
ENV PYCURL_SSL_LIBRARY=openssl
ARG UNRAR_VERSION="7.1.7"
ARG BUILD_ARCH="amd64"
# Execute during the build of the image

RUN \
    apk add --no-cache --virtual .build-dependencies \
        libffi-dev \
        build-base \
        curl-dev \
    \
    && pip --no-cache-dir install \
        pycurl \
    \
    && curl -sSLf -o unrarsrc.tar.gz "https://www.rarlab.com/rar/unrarsrc-${UNRAR_VERSION}.tar.gz" && \
    tar -zxf "/tmp/unrarsrc.tar.gz" && \
    cd unrar && make -f makefile && \
    install -v -m755 unrar /usr/bin && \
    \
    if [ ! "${BUILD_ARCH}" = "i386" ] &&  [ ! "${BUILD_ARCH}" = "armhf" ]; then \
        apk add --no-cache tesseract-ocr; \
    fi \
    \
    && apk add --no-cache \
        p7zip \
        sqlite \
        ffmpeg \
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