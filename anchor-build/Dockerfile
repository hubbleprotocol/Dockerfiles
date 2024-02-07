
#
# Docker image to generate deterministic, verifiable builds of Anchor programs.
# This must be run *after* a given ANCHOR_CLI version is published and a git tag
# is released on GitHub.
#

FROM ubuntu:23.10

ARG DEBIAN_FRONTEND=noninteractive

ARG SOLANA_CLI
ARG ANCHOR_CLI
ARG RUST_VERSION

ENV NODE_VERSION="v20.11.0"
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

# Install rust.
RUN curl "https://sh.rustup.rs" -sSf | sh -s -- -y --default-toolchain ${RUST_VERSION}

# Install node / npm / yarn.
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
ENV NVM_DIR="${HOME}/.nvm"
RUN . $NVM_DIR/nvm.sh && \
    nvm install ${NODE_VERSION} && \
    nvm use ${NODE_VERSION} && \
    nvm alias default node && \
    npm install -g yarn

# Install Solana tools.
RUN sh -c "$(curl -sSfL https://release.solana.com/v${SOLANA_CLI}/install)"

# Install anchor.
RUN cargo install --git https://github.com/coral-xyz/anchor --tag v${ANCHOR_CLI} anchor-cli --locked

# Build a sample from scope repository to bootstrap the BPF SDK (doing this speeds up builds).
RUN anchor init sample && \
    cd sample && \
    cargo update -p solana-program --precise ${SOLANA_CLI} && \
    anchor build && \
    cd .. && \
    rm -rf sample

WORKDIR /workdir

