FROM alpine:latest AS builder

RUN mkdir -p /usr/src/tdlib
WORKDIR /usr/src/tdlib

RUN apk update
RUN apk upgrade
RUN apk add --update alpine-sdk linux-headers git zlib-dev openssl-dev gperf php cmake
RUN apk add --update make gcc g++
RUN git clone https://github.com/tdlib/td.git
WORKDIR /usr/src/tdlib/td
RUN rm -rf build
RUN mkdir -p /usr/src/tdlib/td/build
WORKDIR /usr/src/tdlib/td/build
RUN cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr/local ..
RUN cmake --build . --target prepare_cross_compiling
RUN cmake --build . --target install
WORKDIR /usr/src/tdlib
RUN ls -l /usr/local

FROM alpine:latest AS release

LABEL org.opencontainers.image.source https://github.com/ender-null/tdlib-docker

WORKDIR /usr/local/lib
COPY --from=builder /usr/local/lib/libtdjson.so ./
