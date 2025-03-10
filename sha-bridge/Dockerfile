# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM AS build

ARG BUILD_VERSION

WORKDIR /root

RUN apk add rust cargo

RUN \
    curl -sSLf -o standaert-ha-${BUILD_VERSION}.tar.gz \
    "https://gitea.com/arres/standaert-ha/archive/${BUILD_VERSION}.tar.gz" && \
    tar xf standaert-ha-${BUILD_VERSION}.tar.gz && \
    rm standaert-ha-${BUILD_VERSION}.tar.gz

RUN \
    cd standaert-ha/bridge && \
    cargo build --release

FROM build

COPY --from=build /root/standaert-ha/bridge/target/release/sha_bridge /usr/bin/sha_bridge

# Copy root filesystem
COPY rootfs /
