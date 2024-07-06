# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM AS build

WORKDIR /root

RUN apk add rust cargo

RUN \
    curl -sSLf -o standaert-ha-main-bridge.tar.bz2 \
    "https://gitlab.com/arres/standaert-ha/-/archive/main/standaert-ha-main.tar.bz2?path=bridge" && \
    tar xf standaert-ha-main-bridge.tar.bz2 && \
    rm standaert-ha-main-bridge.tar.bz2

RUN \
    cd standaert-ha-main-bridge/bridge && \
    cargo build --release

FROM build

COPY --from=build /root/standaert-ha-main-bridge/bridge/target/release/sha_bridge /usr/bin/sha_bridge

# Copy root filesystem
COPY rootfs /
