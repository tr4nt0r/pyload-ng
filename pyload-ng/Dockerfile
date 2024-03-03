# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV PYTHONUNBUFFERED=1
ENV PYCURL_SSL_LIBRARY=openssl
# Execute during the build of the image
ARG UNRAR_VERSION PYLOADNG_VERSION BUILD_ARCH

RUN set -o pipefail && \
    apk add --no-cache --virtual .build-dependencies build-base curl-dev && \
    pip --no-cache-dir install pycurl && \
    curl -sSLf -o /tmp/unrarsrc.tar.gz "https://www.rarlab.com/rar/unrarsrc-${UNRAR_VERSION}.tar.gz" && \
    tar -C /tmp -zxf "/tmp/unrarsrc.tar.gz" && \
    cd /tmp/unrar && make -f makefile && \
    install -v -m755 unrar /usr/bin && \
    cd / && rm "/tmp/unrarsrc.tar.gz" && rm -rf /tmp/unrar && \
    apk del .build-dependencies && \
    if [ ! "${BUILD_ARCH}" = "i386" ] &&  [ ! "${BUILD_ARCH}" = "armhf" ]; then \
    apk add --no-cache tesseract-ocr; \
    fi && \
    apk add --no-cache p7zip sqlite ffmpeg && \
    pip --no-cache-dir install --pre pyload-ng[all]==${PYLOADNG_VERSION}

# Copy root filesystem
COPY rootfs /
