FROM ac2-registry.cn-hangzhou.cr.aliyuncs.com/ac2/base:ubuntu24.04

# 1. 配置镜像源为清华大学TUNA源
RUN sed -i 's|http://.*|http://mirrors.tuna.tsinghua.edu.cn/ubuntu|g' /etc/apt/sources.list.d/ubuntu.sources

# 2. 更新包列表（验证源是否可用）
RUN apt-get update

# 3. 安装基础软件包
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y --no-install-recommends \
    openjdk-21-jre \
    tzdata \
    locales \
    xfonts-utils \
    fontconfig \
    libreoffice-nogui

# 4. 配置时区
RUN echo 'Asia/Shanghai' > /etc/timezone && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 5. 配置locale
RUN localedef -i zh_CN -c -f UTF-8 -A /usr/share/locale/locale.alias zh_CN.UTF-8 && \
    locale-gen zh_CN.UTF-8

# 6. 安装字体相关软件包
RUN apt-get install -y --no-install-recommends \
    ttf-mscorefonts-installer \
    ttf-wqy-microhei \
    ttf-wqy-zenhei \
    xfonts-wqy

# 7. 清理
RUN apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 8. 添加自定义字体
ADD fonts/* /usr/share/fonts/chinese/

# 9. 生成字体缓存
RUN mkfontscale /usr/share/fonts/chinese && \
    mkfontdir /usr/share/fonts/chinese && \
    fc-cache -fv

# 10. 设置环境变量
ENV LANG=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8
