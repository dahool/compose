FROM node:lts AS builder

ENV CRONICLE_VERSION=0.9.80

RUN apt update -y && apt install curl unzip -y

WORKDIR /opt/cronicle

RUN curl -L -o /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz https://github.com/jhuckaby/Cronicle/archive/refs/tags/v${CRONICLE_VERSION}.tar.gz
RUN tar zxvf /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz -C /tmp/ && \
    mv /tmp/Cronicle-${CRONICLE_VERSION}/* . && \
    rm -rf /tmp/* && \
    npm install

COPY ./patches /tmp/patches
RUN patch -p3 < /tmp/patches/engine.patch lib/engine.js
COPY docker-entrypoint.js ./bin/

FROM node:lts-alpine

RUN apk add --no-cache python3 rclone procps curl bash

COPY --from=builder /opt/cronicle/ /opt/cronicle/
WORKDIR /opt/cronicle

ENV RCLONE_CONFIG=/opt/rclone/rclone.conf
ENV CRONICLE_foreground=1
ENV CRONICLE_echo=1
ENV CRONICLE_color=1
ENV debug_level=1

RUN node bin/build.js dist && bin/control.sh setup

CMD ["node", "bin/docker-entrypoint.js"]