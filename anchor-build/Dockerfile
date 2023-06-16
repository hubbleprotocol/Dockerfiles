
#
# Docker image to generate deterministic, verifiable builds of Anchor programs.
# This must be run *after* a given ANCHOR_CLI version is published and a git tag
# is released on GitHub.
#

FROM ubuntu:22.10

ARG DEBIAN_FRONTEND=noninteractive

ARG SOLANA_CLI
ARG ANCHOR_CLI

ENV NODE_VERSION="v18.5.0"
ENV HOME="/root"
ENV PATH="${HOME}/.cargo/bin:${PATH}"
ENV PATH="${HOME}/.local/share/solana/install/active_release/bin:${PATH}"
ENV PATH="${HOME}/.nvm/versions/node/${NODE_VERSION}/bin:${PATH}"

# Create base folders
RUN mkdir -p /workdir && mkdir -p /tmp

# Update
RUN apt-get update -qq && \
    apt-get upgrade -qq

# install base utilities.
RUN apt-get install -qq \
    build-essential git curl wget jq pkg-config python3-pip libssl-dev \
    parallel libssl-dev libudev-dev python3-base58 base58

# Install openssl 1.1.1 (required for solana-cli)
RUN curl "http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.19_amd64.deb" -o libssl.deb && \
    dpkg -i ./libssl.deb && \
    rm -f libssl.deb

# Install rust.
RUN curl "https://sh.rustup.rs" -sSf | sh -s -- -y --default-toolchain 1.66.1

# Install node / npm / yarn.
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
ENV NVM_DIR="${HOME}/.nvm"
RUN . $NVM_DIR/nvm.sh && \
    nvm install ${NODE_VERSION} && \
    nvm use ${NODE_VERSION} && \
    nvm alias default node && \
    npm install -g yarn

# Install Solana tools.
RUN sh -c "$(curl -sSfL https://release.solana.com/${SOLANA_CLI}/install)"

# Install anchor.
RUN cargo install --git https://github.com/coral-xyz/anchor --tag ${ANCHOR_CLI} anchor-cli --locked

# Build a sample from scope repository to bootstrap the BPF SDK (doing this speeds up builds).
RUN git clone https://github.com/hubbleprotocol/scope.git && \
    cd scope/programs/test_cpi_caller && \
    cargo build-sbf && \
    cd - && \
    rm -rf scope

WORKDIR /workdir

