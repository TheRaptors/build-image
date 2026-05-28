FROM rockylinux/rockylinux:10

ARG NODE_VERSION=24.16.0
ENV NODE_VERSION=${NODE_VERSION}

# Dev Container 默认不设置；docker run 时可传 --build-arg WORKDIR_PATH=/root/ansible/
ARG WORKDIR_PATH=""
ENV WORKDIR_PATH=${WORKDIR_PATH}

RUN if [ -n "${WORKDIR_PATH}" ]; then mkdir -p "${WORKDIR_PATH}"; fi

WORKDIR ${WORKDIR_PATH:-/}

# 先安装 epel-release 并刷新缓存，再安装 epel 中的 ansible 等软件
RUN dnf install -y epel-release \
    && dnf makecache \
    && dnf install -y ansible dos2unix git libatomic openssh-clients tree vim xz \
    && dnf clean all \
    && rm -rf /var/cache/dnf

RUN curl -fsSL "https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz" \
    | tar -xJ -C /usr/local --strip-components=1

ENV PATH="/usr/local/bin:${PATH}"

RUN curl -fsSL https://cursor.com/install | bash \
    && npm install -g \
        @anthropic-ai/claude-code \
        @github/copilot \
        @google/gemini-cli \
        @openai/codex \
        @qoder-ai/qodercli \
    && npm cache clean --force

CMD ["bash"]
