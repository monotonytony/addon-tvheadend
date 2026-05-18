ARG BUILD_FROM
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# hadolint ignore=DL3003
RUN \
    apk update \
    && apk add --no-cache --virtual .build-deps \
        autoconf=2.71-r2 \
        automake=1.16.5-r2 \
        cmake=3.24.4-r1 \
        gcc=12.2.1_git20220924-r4 \
        g++=12.2.1_git20220924-r4 \
        git=2.38.5-r1 \
        gettext-dev=0.21.1-r1 \
        libtool=2.4.7-r1 \
        liburiparser-dev=0.9.7-r1 \
        libxml2-dev=2.10.3-r1 \
        libxslt-dev=1.1.37-r0 \
        linux-headers=5.19.6-r0 \
        make=4.3-r0 \
        musl-dev=1.2.3-r4 \
        openssl-dev=3.0.8-r4 \
        pkgconfig=1.8.1-r0 \
        python3-dev=3.10.10-r0 \
        zlib-dev=1.2.13-r0 \
    && apk add --no-cache \
        avahi=0.8-r4 \
        liburiparser=0.9.7-r1 \
        openssl=3.0.8-r4 \
        py3-pip=22.3.1-r1 \
        libxslt=1.1.37-r0 \
        zlib=1.2.13-r0 \
    \
    && echo "Building tvheadend v4.3 from source..." \
    && git clone --depth 1 --branch v4.3 \
        https://github.com/tvheadend/tvheadend.git /tmp/tvheadend-src \
    && cd /tmp/tvheadend-src \
    && ./configure \
        --prefix=/usr \
        --enable-avahi \
        --enable-bundle \
        --enable-libx264 \
        --enable-libx265 \
        --enable-libvpx \
        --enable-libtheora \
        --enable-libvorbis \
        --disable-ffmpeg_static \
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
