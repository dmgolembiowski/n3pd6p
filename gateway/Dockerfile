# syntax=docker/dockerfile:1
FROM rust:latest as backend
WORKDIR /usr/src/gateway
COPY . /usr/src/gateway/
RUN rm -rf target || true
RUN ls -lathr .
RUN cat src/main.rs
# RUN exit 1
RUN rustup toolchain install nightly --component clippy,rust-docs,rustfmt
RUN rustup default nightly
RUN cargo +nightly test
RUN cargo +nightly clippy
RUN cargo +nightly fmt -- --check
RUN cargo +nightly build --release

FROM debian:bullseye as debi 

RUN apt-get update && apt-get install -y \
        build-essential \
        wget \
        git \
        libssl-dev \
        cmake \
        pkg-config \
        libtool \
        libcap2-bin \
    && rm -rf /var/lib/apt/lists/*

COPY --from=backend /usr/src/gateway/target/release/gateway /usr/bin/gateway

RUN groupadd -r uwu 
RUN useradd -r -g uwu uwu

RUN setcap cap_net_raw,cap_net_bind_service=+ep /usr/bin/gateway
RUN chmod +x /usr/bin/gateway

EXPOSE 80
EXPOSE 443

USER uwu
CMD ["/usr/bin/gateway"]
