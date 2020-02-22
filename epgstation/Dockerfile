FROM jrottenberg/ffmpeg:4.1-nvidia
EXPOSE 8888
ENV DEV="make gcc git g++ automake curl build-essential python2.7"
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES video,compute,utility

RUN apt update && \
    apt -y install $DEV && \
# create dummy libGL.so.1
# see https://github.com/NVIDIA/nvidia-docker/issues/531
    echo | gcc -shared -o /usr/lib/x86_64-linux-gnu/libGL.so.1 -Wl,-soname=libGL.so.1 -x c - && \
\
# nodejs install
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt install -y nodejs && \
\
# install EPGStation
    cd /usr/local/ && \
    git clone https://github.com/l3tnun/EPGStation.git && \
    cd /usr/local/EPGStation && \
    npm install && \
    npm run build && \
\
# 不要なパッケージを削除
\
    apt -y remove $DEV && \
    apt autoremove -y && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

COPY ./libpcsclite.so.1.0.0 /usr/lib/x86_64-linux-gnu/libpcsclite.so.1
COPY ./config /usr/local/EPGStation/config
COPY ./data /usr/local/EPGStation/data
COPY ./thumbnail /usr/local/EPGStation/thumbnail
COPY ./logs /usr/local/EPGStation/logs
WORKDIR /usr/local/EPGStation

ENTRYPOINT ["npm", "start"]

