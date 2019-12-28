FROM debian:stretch
EXPOSE 40772
ENV DOCKER="YES"
ENV DEV="libpcsclite-dev build-essential curl libssl-dev yasm libtool autoconf pkg-config gcc git g++ make automake"

RUN apt update && \
    apt -y install pcscd libpcsclite1 libccid pcsc-tools && \
    apt -y install $DEV && \
\
# recpt1
\
    git clone https://github.com/stz2012/recpt1 /tmp/recpt1 && \
    cd /tmp/recpt1/recpt1 && \
    ./autogen.sh && \
    ./configure && \
    make && \
    make install && \
\
# nodejs
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt install -y nodejs && \
\
# pm2
\
    npm install pm2 -g && \
\
# arib-b25-stream-test
\
    npm install arib-b25-stream-test -g --unsafe && \
\
# mirakurun
    npm install mirakurun -g --unsafe --production && \
\
# 不要なパッケージを削除
\
    apt -y remove $DEV && \
    apt autoremove -y && \
    apt clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/recpt1

COPY ./libpcsclite.so.1.0.0 /usr/lib/x86_64-linux-gnu/libpcsclite.so.1
WORKDIR /usr/lib/node_modules/mirakurun

ENTRYPOINT /etc/init.d/pcscd start && \
    npm start

