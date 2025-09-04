FROM node:22-trixie-slim

ENV DEBIAN_FRONTEND=noninteractive

# 基础依赖
RUN apt-get -qq update && \
    apt-get install -y \
    gcc \
    g++ \
    make \
    wget \
    curl \
    build-essential \
    zlib1g-dev \
    libssl-dev \
    libffi-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    tk-dev \
    openjdk-21-jdk-headless \
    fpc \
    fp-compiler \
    rustc \
    ghc \
    cabal-install \
    libjavascriptcoregtk-4.0-bin \
    golang \
    ruby \
    mono-runtime \
    mono-mcs \
    kotlin \
    php \
    php-cli \
    nodejs \
    bash \
    python3 \
    pypy3 \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 手动安装 Python2.7.18
RUN cd /tmp && \
    wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz && \
    tar -xzf Python-2.7.18.tgz && \
    cd Python-2.7.18 && \
    ./configure --prefix=/usr/local --enable-unicode=ucs4 && \
    make -j$(nproc) && \
    make altinstall && \
    ln -s /usr/local/bin/python2.7 /usr/bin/python2 && \
    cd / && rm -rf /tmp/Python-2.7.18*

# 设置 python 命令优先使用 python3
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 2 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python2 1

# 验证工具链
RUN gcc --version && \
    g++ --version && \
    fpc -iV && \
    javac -version && \
    rustc --version && \
    ghc --version && \
    cabal --version && \
    go version && \
    ruby --version && \
    mono --version && \
    kotlinc -version && \
    php --version && \
    node --version && \
    python2 --version && \
    python3 --version && \
    pypy3 --version

# 安装 pm2 与 hydrojudge，并自动根据架构下载 sandbox
RUN yarn global add pm2 @hydrooj/hydrojudge && \
    arch=$(dpkg --print-architecture) && \
    case "$arch" in \
      amd64)  url="https://github.com/criyle/go-judge/releases/download/v1.8.0/go-judge_1.8.0_linux_amd64" ;; \
      arm64)  url="https://github.com/criyle/go-judge/releases/download/v1.8.0/go-judge_1.8.0_linux_arm64" ;; \
      *) echo "Unsupported architecture: $arch" && exit 1 ;; \
    esac && \
    wget "$url" -O /usr/bin/sandbox && \
    chmod +x /usr/bin/sandbox

ENTRYPOINT ["/root/entrypoint.sh"]