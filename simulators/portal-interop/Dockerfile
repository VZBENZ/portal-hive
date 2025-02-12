FROM rust:1.71.1 AS builder

# create a new empty shell project
RUN USER=root cargo new --bin portal-interop
WORKDIR /portal-interop

# copy over manifests and source to build image
COPY ./simulators/portal-interop/Cargo.lock ./Cargo.lock
COPY ./simulators/portal-interop/Cargo.toml ./Cargo.toml
COPY ./simulators/portal-interop/src ./src
COPY ./hivesim-rs ./../../hivesim-rs

# build for release
RUN cargo build --release

# final base
FROM ubuntu:22.04

RUN apt update && apt install wget -y

# copy build artifacts from build stage
COPY --from=builder /portal-interop/target/release/portal-interop .
COPY --from=builder /portal-interop/src/test-data ./test-data

ENV RUST_LOG=debug

ENTRYPOINT ["./portal-interop"]
