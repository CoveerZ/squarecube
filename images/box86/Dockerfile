FROM debian:stable-slim

ENV DEBIAN_FRONTEND noninteractive

# install dependencies

RUN dpkg --add-architecture armhf

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y git build-essential cmake

RUN apt-get install -y gcc-arm-linux-gnueabihf libc6:armhf libncurses5:armhf libstdc++6:armhf

RUN apt install -y gcc g++ libgcc1 libc++-dev gdb libc6 git wget curl tar zip unzip binutils xz-utils liblzo2-2 cabextract iproute2 net-tools netcat telnet libatomic1 libsdl1.2debian libsdl2-2.0-0 \
    libfontconfig libicu67 icu-devtools libunwind8 libssl-dev sqlite3 libsqlite3-dev libmariadbclient-dev-compat libduktape205 locales ffmpeg gnupg2 apt-transport-https software-properties-common ca-certificates \
    liblua5.3-0 libz-dev rapidjson-dev tzdata libevent-dev libzip4 libtcmalloc-minimal4:armhf libcurl3-gnutls:armhf libnm0:armhf

RUN update-locale lang=en_US.UTF-8 \
    && dpkg-reconfigure --frontend noninteractive locales

# clone box86 git repo
RUN git clone https://github.com/ptitSeb/box86 && mkdir /box86/build

# compile box86
WORKDIR /box86/build
RUN cmake .. -DRPI4ARM64=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
RUN make -j3
RUN make install

RUN useradd -m -d /home/container -s /bin/bash container
RUN ln -s /home/container/ /nonexistent

ENV USER=container HOME=/home/container
ENV BOX86_PATH=/box86/build/box86

COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]
