# syntax=dockerfile/dockerfile:1
FROM debian:bullseye as debi 

RUN apt-get update && apt-get install -y \
        build-essential \
        wget \
        git \
        libssl-dev \
        cmake \
        pkg-config \
        libtool \
    && rm -rf /var/lib/apt/lists/*

FROM rust:latest as backend
WORKDIR /usr/src/gateway
COPY . .

RUN rustup default nightly

RUN cargo test
RUN cargo clippy
RUN cargo fmt -- --check

RUN cargo build --release 

RUN cp target/release/gateway /srv/gateway
RUN setcap cap_net_raw,cap_net_admin,cap_dac_override+eip /srv/gateway

