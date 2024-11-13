ARG GRPC_VERSION="1.67.1"
ARG UBUNTU_VERSION="24.04"
ARG ALPINE_VERSION="3.20"

ARG USE_OPENSSL="false"

FROM alpine/git:2.45.2 AS git

ARG GRPC_VERSION
RUN git clone --recurse-submodules -b "v${GRPC_VERSION}" --depth 1 \
    --shallow-submodules https://github.com/grpc/grpc /grpc-src


FROM ubuntu:${UBUNTU_VERSION} AS builder

ARG USE_OPENSSL

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y --no-install-recommends \
        autoconf \
        build-essential \
        cmake \
        $(if [ "${USE_OPENSSL}" = "true" ]; then echo "libssl-dev"; fi) \
        libtool \
        pkg-config \
    && rm -rf /var/lib/apt/lists/*

ARG GRPC_SRC_DIR="/tmp/grpc-src"
COPY --from=git /grpc-src ${GRPC_SRC_DIR}

ARG GRPC_DIR="/grpc"
RUN mkdir -p ${GRPC_DIR} \
    && cd ${GRPC_SRC_DIR} \
    && mkdir -p cmake/build \
    && cd cmake/build \
    && cmake -DgRPC_INSTALL=ON \
        -DgRPC_BUILD_TESTS=OFF \
        $(if [ "${USE_OPENSSL}" = "true" ]; then echo "-DgRPC_SSL_PROVIDER=package"; fi) \
        -DCMAKE_INSTALL_PREFIX=${GRPC_DIR} \
        ../.. \
    && make -k -j 4 \
    && make install


FROM alpine:${ALPINE_VERSION}

ARG GRPC_DIR="/opt/grpc"

COPY --from=builder /grpc ${GRPC_DIR}

ENV PATH="${PATH}:${GRPC_DIR}/bin"
