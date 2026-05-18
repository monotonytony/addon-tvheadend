ARG BUILD_FROM
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# hadolint ignore=DL3003
RUN \
    apk update \
    && apk upgrade \
    && apk add --no-cache --upgrade \
        libcrypto3=3.0.19-r0 \
        libssl3=3.0.19-r0 \
        musl=1.2.3-r6 \
    && apk add --no-cache --virtual .build-deps \
        autoconf \
        automake \
        avahi-dev \
        cmake \
        patch \
        gcc \
        g++ \
        git \
        gettext-dev \
        libtool \
        libxml2-dev \
        libxslt-dev \
        linux-headers \
        make \
        openssl-dev \
        pkgconfig \
        python3-dev \
        zlib-dev \
    && apk add --no-cache \
        avahi \
        openssl \
        py3-pip \
        libxslt \
        libgcc \
        zlib \
    \
    && echo "Building tvheadend v4.3 from source..." \
    && git clone --depth 1 --branch v4.3 \
        https://github.com/tvheadend/tvheadend.git /tmp/tvheadend-src \
    && cd /tmp/tvheadend-src \
    && ./configure \
        --prefix=/usr \
        --enable-avahi \
        --disable-libx264 \
        --disable-libx265 \
        --disable-libvpx \
        --disable-libtheora \
        --disable-libvorbis \
        --disable-libfdkaac \
        --disable-ffmpeg \
    && make -j"$(nproc)" \
    && make install \
    && cd / && rm -rf /tmp/tvheadend-src \
    \
    && pip3 install \
        --no-cache-dir \
        --prefer-binary \
        -r /tmp/requirements.txt \
    && apk del --no-cache --purge .build-deps

# Copy root filesystem
COPY rootfs /

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
    io.hass.version=${BUILD_VERSION} \
    maintainer="Gautham Varma K <43441192+GauthamVarmaK@users.noreply.github.com>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="GauthamVarmaK" \
    org.opencontainers.image.authors="Gautham Varma K <43441192+GauthamVarmaK@users.noreply.github.com>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/GauthamVarmaK/addon-tvheadend" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
