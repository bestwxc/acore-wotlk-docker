ARG BASE_IMAGE_VER=11.6
####
FROM debian:$BASE_IMAGE_VER as base-without-maps
# define the build args
ARG USER_NAME=acore
ARG GROUP_NAME=acore
ARG USER_ID=2000
ARG GROUP_ID=2000
# create user and group
RUN groupadd -g $GROUP_ID $GROUP_NAME \
    && useradd -g $GROUP_ID -u $USER_ID -m -s /bin/bash $USER_NAME
# update and install require lib.
RUN set -eux \
    && sed -i s@/deb.debian.org/@/mirrors.ustc.edu.cn/@g /etc/apt/sources.list \
    && apt update  \
    && apt upgrade -y \
    && apt install -y --no-install-recommends iputils-ping telnet \
    && rm -rf /var/lib/apt/*


####
FROM debian:11.6 as downloader
ARG USER_NAME=acore
RUN set -eux \
    &&  sed -i s@/deb.debian.org/@/mirrors.ustc.edu.cn/@g /etc/apt/sources.list \
    && apt update \
    && apt-get install -y --no-install-recommends git wget zip unzip curl
WORKDIR /home/$USER_NAME

# 下载资源
RUN wget --no-check-certificate https://github.com/wowgaming/client-data/releases/download/v16/data.zip \
    && mkdir -p data \
    && unzip data.zip -d data \
    && rm -rf data.zip

# 下载源码
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/azerothcore-wotlk.git \
    --branch master --single-branch azerothcore --depth 1
# mod-zone-difficulty
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-zone-difficulty.git
# mod-progression-system
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-progression-system.git
# mod-guildhouse
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-guildhouse.git
# mod-eluna
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-eluna.git
# mod-transmog
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-transmog.git
# mod-anticheat
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-anticheat.git
# mod-weapon-visual
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-weapon-visual.git
# mod-autobalance
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-autobalance.git
# mod-ah-bot
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-ah-bot.git
# mod-item-level-up
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-item-level-up.git
# mod-duel-reset
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-duel-reset.git
# 动态xp
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-dynamic-xp.git
# mod-keep-out
RUN GIT_SSL_NO_VERIFY=1 git clone https://github.com/azerothcore/mod-keep-out.git



####
FROM base-without-maps as builder
RUN set -eux \
    &&  sed -i s@/deb.debian.org/@/mirrors.ustc.edu.cn/@g /etc/apt/sources.list \
    && apt update \
    && apt-get install -y --no-install-recommends git cmake make gcc g++ clang default-libmysqlclient-dev \
          libssl-dev libbz2-dev libreadline-dev libncurses-dev mariadb-server libboost-all-dev
ARG USER_NAME=acore
WORKDIR /home/$USER_NAME

####
FROM base-without-maps as base-with-maps
# copy data to base with maps
ARG USER_NAME=acore

####
FROM base-without-maps as ac-wotlk-authserver-without-maps
ARG USER_NAME=acore

####
FROM base-without-maps as ac-wotlk-worldserver-without-maps
ARG USER_NAME=acore

####
FROM base-without-maps as ac-wotlk-all-without-maps
ARG USER_NAME=acore

####
FROM base-with-maps as ac-wotlk-authserver-with-maps
ARG USER_NAME=acore

####
FROM base-with-maps as ac-wotlk-worldserver-with-maps
ARG USER_NAME=acore

####
FROM base-with-maps as ac-wotlk-all-with-maps
ARG USER_NAME=acore

