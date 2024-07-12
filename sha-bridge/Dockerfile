# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM AS build

ARG BUILD_VERSION

WORKDIR /root

RUN apk add rust cargo

RUN \
    curl -sSLf -o standaert-ha-${BUILD_VERSION}-bridge.tar.bz2 \
    "https://gitlab.com/arres/standaert-ha/-/archive/${BUILD_VERSION}/standaert-ha-${BUILD_VERSION}.tar.bz2?path=bridge" && \
    tar xf standaert-ha-${BUILD_VERSION}-bridge.tar.bz2 && \
    rm standaert-ha-${BUILD_VERSION}-bridge.tar.bz2

RUN \
    cd standaert-ha-${BUILD_VERSION}-bridge/bridge && \
    cargo build --release

FROM build

COPY --from=build /root/standaert-ha-${BUILD_VERSION}-bridge/bridge/target/release/sha_bridge /usr/bin/sha_bridge

# Copy root filesystem
COPY rootfs /
