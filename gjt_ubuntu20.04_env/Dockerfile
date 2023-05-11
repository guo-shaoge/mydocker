FROM ubuntu:20.04

# source
COPY ./source /etc/apt/sources.list.new
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && cp /etc/apt/sources.list.new /etc/apt/sources.list && apt-get update && \
	apt-get install -y git && apt-get install -y wget && apt-get install -y vim && apt-get install -y curl

WORKDIR /root
# tiflash related
# Install Rust toolchain, see https://rustup.rs for details
RUN curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain none -y
# RUN source "/root/.cargo/env"
# Install LLVM, see https://apt.llvm.org for details
# Clang will be available as /usr/bin/clang++-15
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh && ./llvm.sh 15 all
# Install other dependencies
RUN apt-get install -y ninja-build zlib1g-dev libcurl4-openssl-dev ccache

RUN apt-get install -y software-properties-common lsb-release
RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
RUN apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
RUN apt-get update && apt-get install -y cmake
