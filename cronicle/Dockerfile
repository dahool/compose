FROM node:lts

ENV CRONICLE_VERSION=0.9.80

# install the tools I may going to need to run jobs
RUN apt update -y && apt install curl unzip python3 -y

#RUN curl -L -o /tmp/rclone-current-linux-amd64.zip https://downloads.rclone.org/rclone-current-linux-amd64.zip
#RUN unzip /tmp/rclone-current-linux-amd64.zip -d /tmp/ && \
#    cd /tmp/rclone-*-linux-amd64 && \
#    cp rclone /usr/bin

RUN curl https://rclone.org/install.sh | bash
RUN rclone config touch

WORKDIR /opt/cronicle

RUN curl -L -o /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz https://github.com/jhuckaby/Cronicle/archive/refs/tags/v${CRONICLE_VERSION}.tar.gz
RUN tar zxvf /tmp/Cronicle-${CRONICLE_VERSION}.tar.gz -C /tmp/ && \
    mv /tmp/Cronicle-${CRONICLE_VERSION}/* . && \
    rm -rf /tmp/* && \
    npm install

COPY ./patches /tmp/patches
RUN patch -p3 < /tmp/patches/engine.patch lib/engine.js

COPY docker-entrypoint.js ./bin/

ENV CRONICLE_foreground=1
ENV CRONICLE_echo=1
ENV CRONICLE_color=1
ENV debug_level=1

RUN node bin/build.js dist && bin/control.sh setup

CMD ["node", "bin/docker-entrypoint.js"]