FROM node:20

# 设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone

# 安装 ssh 服务，用于支持 VSCode 客户端通过 Remote-SSH 访问开发环境
RUN apt-get update && \
    apt-get install -y \
    git-lfs sudo curl wget git axel nload libgl1-mesa-glx ffmpeg build-essential gcc g++ \
    libgl1 libglu1 libsm6 libxrender1 libxext6 htop unzip openssh-server \
    --no-install-recommends tzdata libsox-dev parallel aria2 ninja-build && \
    apt-get -y autoremove && \
    git lfs install

# COPY /workspace/VSIX/markdown-preview-enhanced.vsix .

# 安装 code-server 和 vscode 常用插件
RUN curl -fsSL https://code-server.dev/install.sh > install-code-server.sh && \
    chmod +x install-code-server.sh && \
    ./install-code-server.sh && \
    rm install-code-server.sh && \
    # 安装VS Code扩展
    mkdir -p /tmp/vscode-extensions && \
    cd /tmp/vscode-extensions && \
    # 下载扩展
    wget -q https://open-vsx.org/api/redhat/vscode-yaml/1.14.0/file/redhat.vscode-yaml-1.14.0.vsix -O yaml.vsix && \
    wget -q https://open-vsx.org/api/dbaeumer/vscode-eslint/2.4.2/file/dbaeumer.vscode-eslint-2.4.2.vsix -O eslint.vsix && \
    wget -q https://open-vsx.org/api/eamodio/gitlens/14.5.1/file/eamodio.gitlens-14.5.1.vsix -O gitlens.vsix && \
    wget -q https://marketplace.windsurf.com/api/shd101wyy/markdown-preview-enhanced/0.8.19/file/shd101wyy.markdown-preview-enhanced-0.8.19.vsix -O markdown-preview-enhanced.vsix && \
    # 安装扩展
    code-server --install-extension yaml.vsix && \
    code-server --install-extension eslint.vsix && \
    code-server --install-extension gitlens.vsix && \
    code-server --install-extension markdown-preview-enhanced.vsix && \
    # 清理
    cd / && \
    rm -rf /tmp/vscode-extensions

# 指定字符集支持命令行输入中文（根据需要选择字符集）
ENV LANG C.UTF-8
ENV LANGUAGE C.UTF-8

# 安装 git-lfs
RUN git lfs install --force

RUN npm install -g hexo-cli && \
    wget -O hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.148.1/hugo_extended_withdeploy_0.148.1_linux-amd64.deb && \
    dpkg -i hugo.deb && \
    npm install -g pnpm && \
    npm install -g edgeone
    


WORKDIR /workspace
