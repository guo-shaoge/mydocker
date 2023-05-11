FROM ubuntu:20.04

COPY ./source /etc/apt/sources.list.new
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak && cp /etc/apt/sources.list.new /etc/apt/sources.list
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update -y && apt install -y cmake build-essential wget git \
        protobuf-compiler libprotobuf-dev libgrpc-dev libgrpc++-dev libc-ares-dev protobuf-compiler-grpc libpoco-dev

ENV GOLANG_VERSION 1.19.2
RUN cd /root && wget -O go.tgz "https://dl.google.com/go/go$GOLANG_VERSION.linux-amd64.tar.gz" && tar -C /usr/local -xzf go.tgz && export PATH="/usr/local/go/bin:$PATH" && go version

ENV GOPATH /root/go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN cd /root && git clone https://github.com/tikv/mock-tikv.git && cd mock-tikv && git checkout 388e21d3bab5d3c4d3ce26e643ab7453188b0288 && \
	go get google.golang.org/grpc/internal/channelz@v1.24.0 && make failpoint-enable && make
RUN cd /root && git clone https://github.com/tikv/client-c.git && \
	cd client-c && sed -i 's/Protobuf_LIBRARY/Protobuf_LIBRARIES/g' src/CMakeLists.txt && \
	git submodule update --init --recursive && mkdir build && cd build && cmake ../ -DENABLE_TESTS=On && make kv_client_ut -j5
