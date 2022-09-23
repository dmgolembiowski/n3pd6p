# syntax=docker/dockerfile:1
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

FROM rust:latest as backend
WORKDIR /usr/src/gateway
COPY . .

RUN rustup toolchain install nightly --component clippy,rust-docs,rustfmt
RUN rustup default nightly
RUN cargo test
RUN cargo clippy
RUN cargo fmt -- --check
RUN cargo build --release 

FROM debi 
COPY --from=backend /usr/src/gateway/target/release/gateway /srv/gateway

RUN addgroup uwugruwp
RUN adduser --system --gecos 'UwU' --disabled-password --disabled-login -h /srv uwu uwugruwp
RUN setcap cap_net_raw,cap_net_admin,cap_dac_override+eip /srv/gateway

RUN chmod +x /srv/gateway

EXPOSE 80
EXPOSE 443

USER uwu
CMD ["/srv/gateway"]
