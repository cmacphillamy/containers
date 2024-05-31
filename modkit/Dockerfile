FROM continuumio/miniconda3:latest

# update ubuntu packages
RUN apt-get update --fix-missing && \
    yes|apt-get upgrade && \
    apt-get install -y \
        curl \
        wget \
        bzip2 \
        make \
        g++ \
        git \
        libboost-graph-dev && \
    rm -rf /bar/lib/apt/lists/* && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y && \
    . "$HOME/.cargo/env" && \
    mkdir /opt/bin && cd /opt/bin && \
    git clone https://github.com/nanoporetech/modkit.git && \
    cd modkit && \
    cargo install --path .
    