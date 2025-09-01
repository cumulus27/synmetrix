# 使用 Debian Bookworm 作为基础镜像（ARM64 支持）
FROM debian:bookworm

# 设置工作目录
WORKDIR /usr/src/node

# 替换为国内镜像源并清除原有源配置
RUN rm -rf /etc/apt/sources.list /etc/apt/sources.list.d/* && \
    echo "deb http://mirrors.aliyun.com/debian/ bookworm main non-free contrib" > /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian/ bookworm main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security/ bookworm-security main" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian-security/ bookworm-security main" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian/ bookworm-updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian/ bookworm-updates main non-free contrib" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    python3 \
    python3-dev \
    python3-pip \
    g++ \
    make \
    gcc \
    libssl-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 复制本机 Node.js 源码到容器
COPY node-22.19.0.tar.gz .

# 解压并编译 Node.js
RUN tar -xzf node-22.19.0.tar.gz --strip-components=1 && \
    ./configure --prefix=/usr/local && \
    make -j$(nproc) && \
    make install && \
    rm -rf /usr/src/node

# 设置 PATH 环境变量
ENV PATH="/usr/local/bin:$PATH"

# 验证 Node.js 和 npm 安装
RUN node -v && npm -v

# 设置npm国内镜像源
RUN npm config set registry https://registry.npmmirror.com

# 设置默认命令
CMD ["node"]
