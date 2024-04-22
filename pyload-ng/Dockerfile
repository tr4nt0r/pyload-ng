# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV PYTHONUNBUFFERED=1
ENV PYCURL_SSL_LIBRARY=openssl
# Execute during the build of the image
ARG UNRAR_VERSION PYLOADNG_VERSION BUILD_ARCH

# COPY --from=ghcr.io/linuxserver/unrar:7.0.7-ls5 /usr/bin/unrar-alpine /usr/bin/unrar

RUN set -o pipefail && \
    apk add --no-cache --virtual .build-dependencies build-base curl-dev && \
    pip --no-cache-dir install pycurl && \
    curl -sSLf -o /tmp/unrarsrc.tar.gz "https://www.rarlab.com/rar/unrarsrc-${UNRAR_VERSION}.tar.gz" && \
    tar -C /tmp -zxf "/tmp/unrarsrc.tar.gz" && \
    cd /tmp/unrar && make -f makefile && \
    install -v -m755 unrar /usr/bin && \
    rm -rf /tmp/* /root/.cache && \
    apk del --purge .build-dependencies

RUN if [ ! "${BUILD_ARCH}" = "i386" ] &&  [ ! "${BUILD_ARCH}" = "armhf" ]; then \
    apk add --no-cache tesseract-ocr; \
    fi && \
    apk add --no-cache p7zip sqlite ffmpeg && \
    pip --no-cache-dir install pyload-ng==${PYLOADNG_VERSION}

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