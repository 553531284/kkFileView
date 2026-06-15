FROM ubuntu:24.04

RUN ARCH=$(dpkg --print-architecture) && \
    if [ "$ARCH" = "arm64" ]; then \
        sed -i 's|http://ports.ubuntu.com/ubuntu-ports|https://mirrors.aliyun.com/ubuntu-ports|g' /etc/apt/sources.list.d/ubuntu.sources; \
    else \
        sed -i 's|http://archive.ubuntu.com/ubuntu|https://mirrors.aliyun.com/ubuntu|g' /etc/apt/sources.list.d/ubuntu.sources && \
        sed -i 's|http://security.ubuntu.com/ubuntu|https://mirrors.aliyun.com/ubuntu|g' /etc/apt/sources.list.d/ubuntu.sources; \
    fi && \
    apt-get -o Acquire::https::Verify-Peer=false update && \
    apt-get -o Acquire::https::Verify-Peer=false install -y --no-install-recommends ca-certificates && \
    apt-get update && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get install -y --no-install-recommends openjdk-21-jre tzdata locales xfonts-utils fontconfig libreoffice-nogui && \
    echo 'Asia/Shanghai' > /etc/timezone && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    localedef -i zh_CN -c -f UTF-8 -A /usr/share/locale/locale.alias zh_CN.UTF-8 && \
    locale-gen zh_CN.UTF-8 && \
    apt-get install -y --no-install-recommends ttf-mscorefonts-installer && \
    apt-get install -y --no-install-recommends ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 内置一些常用的中文字体，避免普遍性乱码
ADD fonts/* /usr/share/fonts/chinese/

RUN cd /usr/share/fonts/chinese &&\
    # 安装字体
    mkfontscale &&\
    mkfontdir &&\
    fc-cache -fv

ENV LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8
